---
layout: post
title: "Homelab Milestone 2: Dual Monitoring Platform Deployed"
date: 2025-10-27 01:04:28 +0000
categories: infrastructure homelab
tags: [homelab, infrastructure, automation, ai-orchestration, milestone-2]
phase: "Phase 1-3: Foundation - Monitoring Infrastructure"
milestone: 2
---

# Homelab Milestone 2: Dual Monitoring Platform Deployed

> **Security Note**: All credentials, IP addresses, MAC addresses, and sensitive
> information in this post are **fictional examples** sanitized for publication.
> Your environment will have different values. Never share real credentials publicly.

## Phase: Phase 1-3: Foundation - Monitoring Infrastructure

## What We Built

**LibreNMS Platform (Container 150 - 10.0.1.160)**
- Debian 12 LXC container on Proxmox
- LibreNMS 25.11.0-dev installed and configured
- 11 core infrastructure devices monitored
- Automatic polling every 5 minutes
- Automatic discovery every 6 hours
- Full API access with authentication token

**PRTG Platform (VM 151 - 10.0.1.161)**
- Windows 11 Enterprise VM on Proxmox
- PRTG Network Monitor 25.4.112.1189
- 68 devices across 4 VLANs monitored
- 346+ active sensors
- Multi-protocol monitoring (SNMP, ICMP, HTTP, HTTPS, RDP)
- SSL-enabled web interface
- Full API access

**Monitored Infrastructure** (100% overlap on critical devices):
1. OPNsense Firewall (10.0.1.254)
2. HP 48-Port Managed Switch (10.0.1.251)
3. Proxmox Virtualization Host (10.0.1.250)
4. Ruckus Wireless Access Point (10.0.1.100)
5-11. All 7 NAS devices (Synology and WD)

**PRTG Additional Coverage** (57 extra devices):
- 4 additional Ruckus APs
- 17+ smart home devices
- 3 security cameras
- Smart appliances
- IoT sensors
- Wireless clients

**Dual Monitoring Philosophy**: "Two is one, one is none"
- No single point of failure
- Cross-validation capability
- Automatic failover readiness
- Comprehensive coverage (core + IoT)

## Implementation Details

### Creating LibreNMS LXC Container

```bash
# Create Debian 12 LXC on Proxmox
pct create 150 local:vztmpl/debian-12-standard_12.7-1_amd64.tar.zst \
  --hostname librenms \
  --cores 2 \
  --memory 4096 \
  --swap 512 \
  --rootfs local-lvm:8 \
  --net0 name=eth0,bridge=vmbr0,ip=10.0.1.160/24,gw=10.0.1.254 \
  --nameserver 10.0.1.254 \
  --unprivileged 1 \
  --features nesting=1 \
  --onboot 1 \
  --start 1

# Install LibreNMS (manual method after automated installer hung)
pct enter 150
apt update && apt upgrade -y
# ... manual installation steps following LibreNMS docs
```

### LibreNMS Discovery and Polling

```bash
# Enter LibreNMS container
pct exec 150 -- su - librenms

# Network SNMP scan (adds devices)
cd /opt/librenms
./snmp-scan.py 192.168.1.0/24
# Found: 7 new devices in 71 seconds

# CRITICAL: Run discovery immediately (don't wait 6 hours!)
./discovery.php -h all
# Discovers: sensors, storage, ports, processors

# Create RRD files for graphs
./poller-wrapper.py 1
# Polled: 11 devices in 27 seconds

# Verify cron is working
crontab -l | grep librenms
# */5 * * * * /opt/librenms/cronic /opt/librenms/poller-wrapper.py 16
# 33 */6 * * * /opt/librenms/cronic /opt/librenms/discovery.php -h all
```

### Creating PRTG Windows VM

```bash
# Create Windows 11 VM on Proxmox
qm create 151 \
  --name PRTG-Monitor \
  --memory 8192 --cores 4 --cpu host \
  --ostype win11 --machine q35 --bios ovmf \
  --efidisk0 local-lvm:1,efitype=4m,pre-enrolled-keys=1 \
  --tpmstate0 local-lvm:1,version=v2.0 \
  --scsi0 local-lvm:100,cache=writeback,discard=on,ssd=1 \
  --ide0 synology-isos:iso/Win11_Enterprise_26200.6725_Multi.iso,media=cdrom \
  --ide2 synology-isos:iso/virtio-win-0.1.285.iso,media=cdrom \
  --boot order=scsi0;ide0 \
  --net0 virtio,bridge=vmbr0 \
  --agent enabled=1

# Start VM
qm start 151

# After Windows install, fix boot order
qm set 151 --boot 'order=scsi0;ide0'  # Disk first!
```

### Testing Both APIs

```bash
# Test LibreNMS API
curl http://10.0.1.160/api/v0/devices \
  -H "X-Auth-Token: [YOUR_API_TOKEN]"
# Returns: {"status":"ok","count":11,"devices":[...]}

# Test PRTG API (username+password method)
curl -k "https://10.0.1.161/api/table.json?\
  content=devices&output=json&\
  username=prtgadmin&password=[REDACTED]"
# Returns: {"prtg-version":"25.4.112.1189","treesize":68,"devices":[...]}
```

### Future Monitoring Bridge Example

```python
import asyncio
import aiohttp
from fastapi import FastAPI

app = FastAPI()

# Concurrent API polling
async def get_device_status(device_ip):
    """Query both platforms concurrently"""

    async def query_librenms():
        async with aiohttp.ClientSession() as session:
            headers = {"X-Auth-Token": "[API_TOKEN]"}
            async with session.get(
                f"http://10.0.1.160/api/v0/devices/{device_ip}",
                headers=headers
            ) as resp:
                return await resp.json()

    async def query_prtg():
        async with aiohttp.ClientSession() as session:
            url = f"https://10.0.1.161/api/table.json?content=devices&filter_host={device_ip}"
            async with session.get(url, ssl=False) as resp:
                return await resp.json()

    # Wait for first successful response (failover)
    libre_task = asyncio.create_task(query_librenms())
    prtg_task = asyncio.create_task(query_prtg())

    done, pending = await asyncio.wait(
        [libre_task, prtg_task],
        return_when=asyncio.FIRST_COMPLETED
    )

    # Return first result, cancel pending
    for task in pending:
        task.cancel()

    return list(done)[0].result()

@app.get("/api/v1/devices/{ip}/status")
async def device_status(ip: str):
    return await get_device_status(ip)
```

## Challenges Encountered

**1. LibreNMS Graph Generation Errors**
- **Problem**: After adding devices via SNMP scan, graphs showed "Error opening RRD file"
- **Symptom**: Devices appeared in list but no historical data or graphs
- **Impact**: Unable to visualize infrastructure metrics

**2. IP Conflict During VM Creation**
- **Problem**: Created VM without verifying IP availability first
- **Risk**: Could have caused network conflicts
- **Previous incident**: Same issue occurred during LibreNMS container creation

**3. UEFI Boot Manager Issues**
- **Problem**: Windows VM stuck in UEFI boot manager after installation
- **Symptom**: System repeatedly booted to firmware instead of Windows
- **Impact**: VM unusable until boot order fixed

**4. Windows OOBE Microsoft Account Requirement**
- **Problem**: Needed local account but Shift+F10 didn't work in Proxmox console
- **Limitation**: Keyboard shortcuts intercepted by host system
- **Goal**: Create local "PRTGBox" account without Microsoft account

**5. PRTG API Authentication**
- **Problem**: Initial API test with passhash token returned "Unauthorized"
- **Confusion**: Documentation showed token-based auth but didn't work
- **Impact**: Delayed API integration testing

## Solutions

**1. Graph Generation Fix - Discovery Process**
```bash
# The issue: SNMP scan only ADDS devices, doesn't discover sensors
./snmp-scan.py 192.168.1.0/24  # Only adds to database

# Solution: Run full discovery immediately after scan
cd /opt/librenms
./discovery.php -h all          # Discovers sensors/storage/interfaces
./poller-wrapper.py 1           # Creates RRD files immediately

# Result: All graphs generated successfully
```
**Lesson**: Don't wait for 6-hour automatic discovery cron. Run it manually!

**2. IP Verification Checklist**
Created mandatory pre-flight checklist for all VM/container creation:
```bash
# Step 1: ALWAYS ping test first
ping 10.0.1.161 -n 2 -w 1000
# Expected: "Request timed out" = FREE

# Step 2: Check monitoring platforms
# LibreNMS database check
mysql -u librenms -p[PASSWORD] librenms \
  -e "SELECT hostname, ip FROM devices WHERE ip LIKE '%161%';"

# Step 3: Only proceed if both checks pass
```
**Documentation**: Created `VM-CREATION-CHECKLIST.md` with all steps

**3. UEFI Boot Order Fix**
```bash
# Problem: Boot order was CD-ROM first
qm config 151 | grep boot
# Showed: order=ide0;scsi0  (CD first!)

# Solution: Prioritize disk over CD-ROM
qm set 151 --boot 'order=scsi0;ide0'

# Result: Windows boots from disk successfully
```

**4. OOBE Bypass via Network Disconnection**
```bash
# Since Shift+F10 didn't work, disconnect network at VM level
qm set 151 --net0 virtio,bridge=vmbr0,link_down=1

# This triggers "I don't have internet" path in OOBE
# Allows "Continue with limited setup" → Local account creation

# After account created, reconnect
qm set 151 --net0 virtio,bridge=vmbr0,link_down=0
```
**Innovation**: VM-level network control instead of keyboard shortcuts!

**5. PRTG API Authentication Method**
```bash
# Token method DIDN'T work:
curl -k "https://10.0.1.161/api/table.json?passhash=TOKEN"
# Returned: Unauthorized

# Solution: Use username+password instead
curl -k "https://10.0.1.161/api/table.json?\
  content=devices&output=json&\
  username=prtgadmin&password=[REDACTED]"
# Success! Returns JSON with all 68 devices
```
**Note**: PRTG API uses different auth than web UI tokens.

## Lessons Learned

**1. Discovery vs Polling in LibreNMS**
- **SNMP Scan**: Adds devices to database (minimal info)
- **Discovery**: Finds sensors, storage, ports, processors
- **Polling**: Collects current values from discovered sensors
- **All three needed** for complete monitoring
- **Always run discovery immediately** after adding devices

**2. IP Management Best Practices**
- **Never assume** an IP is free
- **Always test** with ping before assignment
- **Check databases** of all monitoring platforms
- **Document** in checklist to prevent repeated mistakes
- **Previous failure** (LibreNMS .150 conflict) proved this critical

**3. UEFI Boot Management**
- Boot order changes **after** OS installation
- CD-ROM first = infinite boot loop
- Always set disk first after install completes
- UEFI VMs require extra attention vs BIOS

**4. Proxmox Console Limitations**
- Some keyboard shortcuts **don't reach the VM**
- Host system intercepts common key combos
- **Alternative approaches** often more reliable
- VM-level controls (network, etc.) bypass console issues

**5. PRTG Auto-Discovery Power**
- Discovers **across VLANs automatically**
- Finds non-SNMP devices via ping
- Much broader coverage than LibreNMS SNMP-only
- Multi-protocol approach finds everything
- **68 devices found** vs LibreNMS 11 (SNMP-capable only)

**6. API Authentication Varies by Platform**
- LibreNMS: Token in X-Auth-Token header ✅
- PRTG: Username+password in query params ✅
- **Don't assume** same method works everywhere
- **Test early** to avoid integration delays
- Read platform-specific API docs carefully

**7. Documentation Prevents Rework**
- Comprehensive docs **save time** on repeat tasks
- Checklists **prevent repeated mistakes**
- Vault structure **organizes credentials**
- Process docs **enable handoff** to other Claudes
- **Write as you go**, not after completion

**8. Dual Monitoring Philosophy Works**
- **"Two is one, one is none"** proven valuable
- 100% overlap on critical infrastructure
- Cross-validation ready
- Automatic failover capability
- LibreNMS (depth) + PRTG (breadth) = complete coverage

## What's Next?

**Immediate Next Phase: TASK-002 - Monitoring Bridge**

Deploy a unified API that aggregates both LibreNMS and PRTG:

**Architecture**:
- Python 3.11+ with FastAPI framework
- LXC Container 152 (Monitoring Bridge)
- Concurrent API calls via asyncio/aiohttp
- Redis for caching
- PostgreSQL/SQLite for aggregated data

**Endpoints**:
```python
GET /api/v1/health          # Platform availability
GET /api/v1/devices         # Merged device list (deduplicated)
GET /api/v1/devices/{id}    # Consensus status from both platforms
GET /api/v1/alerts          # Merged alerts
```

**Failover Logic**:
```python
# Query both platforms concurrently
libre_task = asyncio.create_task(librenms_api.get_device(id))
prtg_task = asyncio.create_task(prtg_api.get_device(id))

# Return first successful response
done, pending = await asyncio.wait(
    [libre_task, prtg_task],
    return_when=asyncio.FIRST_COMPLETED
)
```

**Deduplication**:
1. Match devices by IP address (primary)
2. Match by hostname (secondary)
3. Merge metadata from both platforms
4. Flag status discrepancies for review

**Expected Timeline**: 2-4 hours deployment + 1-2 hours testing

**Future Phases**:
- Phase 2: VLAN 100 migration (management network)
- Phase 3: AI Orchestrator LXCs deployment
- Phase 4: Natural language infrastructure control
- Phase 5: Automated remediation and self-healing

---

## Project Series

This is **Milestone 2** in my AI-Powered Home Lab series.

**Previous posts:**
- [Part 1: Architecture & Planning](link-to-part-1)

**Coming soon:**
- Next milestone post

---

*Automatically logged from LLM Infrastructure Orchestrator project*
*Blog automation powered by Claude Desktop + Code*
