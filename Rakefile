require 'puppetlabs_spec_helper/rake_tasks'
require 'puppet-lint/tasks/puppet-lint'
require 'metadata-json-lint/rake_task'
require 'puppet_blacksmith/rake_tasks'
require 'ci/reporter/rake/rspec'
PuppetLint.configuration.send('disable_80chars')
PuppetLint.configuration.ignore_paths = ["tests/*.pp", "spec/**/*.pp", "pkg/**/*.pp"]
PuppetLint.configuration.log_format = '%{path}:%{line}:%{check}:%{KIND}:%{message}'
PuppetLint.configuration.fail_on_warnings = false

desc "Validate manifests, templates, and ruby files"
task :validate do
  Dir['manifests/**/*.pp'].each do |manifest|
    sh "puppet parser validate --noop #{manifest}"
  end
  Dir['spec/**/*.rb','lib/**/*.rb'].each do |ruby_file|
    sh "ruby -c #{ruby_file}" unless ruby_file =~ /spec\/fixtures/
  end
  Dir['templates/**/*.erb'].each do |template|
    sh "erb -P -x -T '-' #{template} | ruby -c"
  end
end

namespace :ci do
  task :all => ['ci:setup:rspec', 'rspec']
end
task :default => [:validate, :spec, :lint, :metadata_lint]
