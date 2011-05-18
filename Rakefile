#!/usr/bin/env ruby
require 'rubygems'
require 'rake/clean'
require 'fileutils'

SRC_FOLDER = "src"
EXCLUDE_PATTERNS = /(Reachability\.[hm])/
SOURCE_FILES = FileList["./*.[hmc]"].select {|f| !(f =~ EXCLUDE_PATTERNS) }


CLEAN.include([SRC_FOLDER])

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
