require 'rspec/core/rake_task'
require 'yaml'

# DO NOT USE DIRECTLY, the reality of RSpec
desc '' # empty string for hidden task
RSpec::Core::RakeTask.new(:spec_run, :example_name) do |t, args|
  t.rspec_opts = "-e #{args[:example_name]}" if args.has_key? :example_name
end

desc 'Run RSpec test with checking env bot needs'
task :spec, :example_name do |t, args|
  envs = ['TWITTER_BEARER_TOKEN', 'DEVIANTART_ID', 'DEVIANTART_SECRET']
  if envs.reject(&ENV.method(:include?)).size == 0
    Rake::Task[:spec_run].invoke args[:example_name]
  else
    puts "Read README.mkd for setting env bot needs: #{envs.join(', ')}"
    puts "You can use default setting; bundle exec rake spec_with_env"
  end
end

desc 'Run RSpec test with using env what is in .travis.yml'
task :spec_with_env, :example_name do |t, args|
  yaml = YAML.load_file('.travis.yml')
  yaml['env']['global'].each do |e|
    key, value = e.split(?=)
    ENV[key] = value.gsub(/^'(.+)'$/, '\1')
  end
  Rake::Task[:spec_run].invoke args[:example_name]
end

task :default => :spec
