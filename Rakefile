require 'rubygems'
require 'rake/clean'

CLOBBER.include('.yardoc', 'doc')

def gemspec
  @gemspec ||= begin
    file = File.expand_path("../google_currency.gemspec", __FILE__)
    eval(File.read(file), binding, file)
  end
end

begin
  require 'spec/rake/spectask'
  Spec::Rake::SpecTask.new
rescue LoadError
  task(:spec){abort "`gem install rspec` to run specs"}
end
task :default => :spec

begin
  require 'yard'
  YARD::Rake::YardocTask.new do |t|
    t.options << "--files" << "CHANGELOG.md,LICENSE"
  end
rescue LoadError
  task(:yard){abort "`gem install yard` to generate documentation"}
end

begin
  require 'rake/gempackagetask'
  Rake::GemPackageTask.new(gemspec) do |pkg|
    pkg.gem_spec = gemspec
  end
  task :gem => :gemspec
rescue LoadError
  task(:gem){abort "`gem install rake` to package gems"}
end

desc "Install the gem locally"
task :install => :gem do
  sh "gem install pkg/#{gemspec.full_name}.gem"
end

desc "Validate the gemspec"
task :gemspec do
  gemspec.validate
end
