source 'https://rubygems.org'

ruby '3.4.7'
gem 'bootsnap', require: false
gem 'importmap-rails'

gem 'puma'
gem 'rails', '~> 8.1'
gem "propshaft"

# Use sqlite3 as the database for Active Record
gem "sqlite3", ">= 2.1"

gem 'stimulus-rails'
gem 'turbo-rails'
gem 'tzinfo-data', platforms: %i[windows jruby]
gem 'vite_rails'

gem "solid_cache"
gem "solid_queue"
gem "solid_cable"

group :development, :test do
  gem 'debug', platforms: %i[mri windows]
  gem 'dotenv'
  gem 'erb_lint'
  gem 'factory_bot_rails'
  gem 'pry-rails'
  gem 'rspec-rails'
  gem 'rubocop'
  gem 'rubocop-ast'
  gem 'rubocop-performance'
  gem 'rubocop-rails'
  gem 'rubocop-rake'
  gem 'rubocop-rspec'
end

group :development do
  gem 'ruby-lsp'
  gem 'web-console'
end

group :test do
  gem 'capybara-playwright-driver'
end
