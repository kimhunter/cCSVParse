#!/usr/bin/env ruby
require 'rubygems'
require 'rake/clean'
require 'fileutils'
require 'yaml'

SRC_FOLDER = "src"
EXCLUDE_PATTERNS = /(Reachability\.[hm])/
SOURCE_FILES = FileList["./*.[hmc]"].select {|f| !(f =~ EXCLUDE_PATTERNS) }

CLEAN.include([SRC_FOLDER])

def inc_revision version
  v = version.to_s.split('.')
  if v.size > 1
    last = v.size - 1 
    if v.last =~ /^[0-9]+(.*)?$/
      v[last] = (v.last.to_i + 1).to_s
      v[last] += $1.to_s unless $1.nil?
    end
  end
  v.join '.'
end

desc "Create KitSpec"
task :kitspec do
  unless File.exists? 'KitSpec'
    kspec = {} 
    kspec['name'] = File.basename Dir.pwd.downcase
    kspec['version'] = 0.1
    f = File.open('KitSpec', 'w')
    f.write kspec.to_yaml
    f.close    
  end
end
desc "Publish"
task :publish => [:kitspec, :kit] do
  `kit publish-local`
end

desc "Bump Version"
task :bump  do
  kitspec = YAML::load_file 'KitSpec'
  old = kitspec['version']
  kitspec['version'] = inc_revision(old)
  f = File.open('KitSpec', 'w')
  f.write kitspec.to_yaml
  f.close
  puts "Updated from #{old} -> #{kitspec['version']}"
end

desc "Find Dups"
task :find_dups do
  basenames = SOURCE_FILES.map {|path| File.basename path}
  dups = basenames.select {|fname| basenames.count(fname) > 1 }
  raise "Duplicates Found CANNOT CONTINUE Dups: \n#{dups.uniq}" unless dups.size.zero?
end


desc "Transform project into Kit Package"
task :kit => [:clean, :find_dups] do
  Dir.mkdir SRC_FOLDER
  
  SOURCE_FILES.each do |orig_file| 
    FileUtils.cp orig_file, "#{SRC_FOLDER}/#{File.basename(orig_file)}"
  end
  
  puts "Copied #{SOURCE_FILES.size} files"

end
