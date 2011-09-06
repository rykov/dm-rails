require 'pathname'

source 'http://rubygems.org'

SOURCE         = ENV.fetch('SOURCE', :git).to_sym
REPO_POSTFIX   = SOURCE == :path ? ''                                : '.git'
DATAMAPPER     = SOURCE == :path ? Pathname(__FILE__).dirname.parent : 'http://github.com/datamapper'
DM_VERSION     = '~> 1.2.0.rc1'
DO_VERSION     = '~> 0.10.6'
RAILS_VERSION  = '~> 3.1.0'
DM_DO_ADAPTERS = %w[ sqlite postgres mysql oracle sqlserver ]

# DataMapper dependencies
gem 'dm-core',         DM_VERSION, SOURCE => "#{DATAMAPPER}/dm-core#{REPO_POSTFIX}"
gem 'dm-active_model', DM_VERSION, SOURCE => "#{DATAMAPPER}/dm-active_model#{REPO_POSTFIX}"

# Rails dependencies
gem 'actionpack', RAILS_VERSION, :require => 'action_pack'
gem 'railties',   RAILS_VERSION, :require => 'rails'

group :development do

  gem 'jeweler', '~> 1.6.4'
  gem 'rake',    '~> 0.9.2'
  gem 'rspec',   '~> 1.3.2'

end

platforms :mri_18 do
  group :quality do

    gem 'rcov',      '~> 0.9.10'
    gem 'yard',      '~> 0.7.2'
    gem 'yardstick', '~> 0.4'

  end
end

group :datamapper do
  adapters = ENV['ADAPTER'] || ENV['ADAPTERS']
  adapters = adapters.to_s.tr(',', ' ').split.uniq - %w[ in_memory ]

  if (do_adapters = DM_DO_ADAPTERS & adapters).any?
    do_options = {}
    do_options[:git] = "#{DATAMAPPER}/do#{REPO_POSTFIX}" if ENV['DO_GIT'] == 'true'

    gem 'data_objects', DO_VERSION, do_options.dup

    do_adapters.each do |adapter|
      adapter = 'sqlite3' if adapter == 'sqlite'
      gem "do_#{adapter}", DO_VERSION, do_options.dup
    end

    gem 'dm-do-adapter', DM_VERSION, SOURCE => "#{DATAMAPPER}/dm-do-adapter#{REPO_POSTFIX}"
  end

  gem 'dm-migrations', DM_VERSION, SOURCE => "#{DATAMAPPER}/dm-migrations#{REPO_POSTFIX}"

  adapters.each do |adapter|
    gem "dm-#{adapter}-adapter", DM_VERSION, SOURCE => "#{DATAMAPPER}/dm-#{adapter}-adapter#{REPO_POSTFIX}"
  end
end
