source "https://rubygems.org"

gem "jekyll", "~> 4.3"
gem "minima", "~> 2.5"

group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
  gem "jekyll-sitemap"
end

platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

# `wdm` speeds up file watching on older Windows Ruby builds, but 0.1.1 fails to
# compile under Ruby 3.3.x. Excluding it there keeps `jekyll build` usable.
gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin] if Gem::Version.new(RUBY_VERSION) < Gem::Version.new("3.3")
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]
