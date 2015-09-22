require "bundler/gem_tasks"
require "rubocop/rake_task"

namespace :test do
  namespace :rubies do
    require 'yaml'

    def config
      dottravis = File.expand_path("../.travis.yml", __FILE__)
      YAML.load(File.read(dottravis))
    end

    namespace :install do
      task :all do
        config['rvm'].each do |ver|
          `rvm install #{ver}` unless system "rvm list | grep #{ver}"
        end
      end
    end

    namespace :run do
      def _exec_in_all_rubies(cmd)
        system "rvm #{config['rvm'].join(',')} do #{cmd}"
      end

      task :install do
        ["gem install bundler",
         "bundle install"].each do |cmd|
          _exec_in_all_rubies cmd
        end
      end

      task :specs do
        _exec_in_all_rubies "bundle exec rspec -f p"
      end

      task :all => %w(install specs).map{ |t| "test:rubies:run:#{t}" }
    end

    task :all => ["test:rubies:install:all", "test:rubies:run:all"]
  end

  task :rubies => ["test:rubies:run:specs"]
  task :all => ["test:rubies"]
end
