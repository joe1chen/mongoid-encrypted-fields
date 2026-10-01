source 'https://rubygems.org'
gemspec
# CI matrix (see .github/workflows/test.yml): MONGOID_VERSION / RAILS_VERSION select versions.
mongoid_version = ENV["MONGOID_VERSION"] || "7.5"
gem "mongoid", "~> #{mongoid_version}.0"
gem "activemodel", "~> #{ENV["RAILS_VERSION"]}.0" if ENV["RAILS_VERSION"]
# ActiveSupport < 7.1 breaks with concurrent-ruby >= 1.3.5 (Logger no longer preloaded).
gem "concurrent-ruby", "< 1.3.5" if ENV["RAILS_VERSION"] && Gem::Version.new(ENV["RAILS_VERSION"]) < Gem::Version.new("7.1")
