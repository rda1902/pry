dlext = Config::CONFIG['DLEXT']
direc = File.dirname(__FILE__)

require 'rake/clean'
require 'rake/gempackagetask'
require "#{direc}/lib/pry/version"

CLOBBER.include("**/*.#{dlext}", "**/*~", "**/*#*", "**/*.log", "**/*.o")
CLEAN.include("ext/**/*.#{dlext}", "ext/**/*.log", "ext/**/*.o",
              "ext/**/*~", "ext/**/*#*", "ext/**/*.obj",
              "ext/**/*.def", "ext/**/*.pdb", "**/*_flymake*.*", "**/*_flymake")

def apply_spec_defaults(s)
  s.name = "pry"
  s.summary = "Open an irb-like session on any object"
  s.version = Pry::VERSION
  s.date = Time.now.strftime '%Y-%m-%d'
  s.author = "John Mair (banisterfiend)"
  s.email = 'jrmair@gmail.com'
  s.description = s.summary
  s.require_path = 'lib'
  s.add_dependency("ruby_parser",">=2.0.5")
  s.homepage = "http://banisterfiend.wordpress.com"
  s.has_rdoc = 'yard'
  s.files = Dir["ext/**/extconf.rb", "ext/**/*.h", "ext/**/*.c", "lib/**/*.rb",
                     "test/*.rb", "CHANGELOG", "README.markdown", "Rakefile"]
end

task :test do
  sh "bacon -k #{direc}/test/test.rb"
end

namespace :ruby do
  spec = Gem::Specification.new do |s|
    apply_spec_defaults(s)        
    s.platform = Gem::Platform::RUBY
  end
  
  Rake::GemPackageTask.new(spec) do |pkg|
    pkg.need_zip = false
    pkg.need_tar = false
  end
end

desc "build all platform gems at once"
task :gems => [:rmgems, "ruby:gem"]

desc "remove all platform gems"
task :rmgems => ["ruby:clobber_package"]

desc "build and push latest gems"
task :pushgems => :gems do
  chdir("#{direc}/pkg") do
    Dir["*.gem"].each do |gemfile|
      sh "gem push #{gemfile}"
    end
  end
end
