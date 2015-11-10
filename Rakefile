require 'bundler'
Bundler::GemHelper.install_tasks
# Don't push the gem to rubygems
ENV["gem_push"] = "false" # Utilizes feature in bundler 1.3.0

require "rspec/core/rake_task"
require "sax-machine"

desc "Run all specs"
RSpec::Core::RakeTask.new do |t|
  t.rspec_opts = ['--options', "\"#{File.dirname(__FILE__)}/spec/spec.opts\""]
end

task :default => [:spec]

task :test do
    sh 'rspec spec'
end

task :install do
  rm_rf "*.gem"
  puts `gem build sax-machine.gemspec`
  puts `sudo gem install sax-machine-#{SAXMachine::VERSION}.gem`
end

# Let bundler's release task do its job, minus the push to Rubygems,
# and after it completes, use "gem inabox" to publish the gem to our
# internal gem server.
Rake::Task["release"].enhance do
  spec = Gem::Specification::load(Dir.glob("*.gemspec").first)
  sh "gem inabox pkg/#{spec.name}-#{spec.version}.gem"
end
