require "rubygems"
require 'rake'
require 'yaml'
require 'time'
require 'bundler/setup'


SOURCE = "."
CONFIG = {
  'version' => "0.3.0",
  'themes' => File.join(SOURCE, "_includes", "themes"),
  'layouts' => File.join(SOURCE, "_layouts"),
  'posts' => File.join(SOURCE, "_posts"),
  'post_ext' => "md",
  'theme_package_version' => "0.1.0"
}


# build and deploy

CONFIG_FILE = "_config.yml"
BASE_CONFIG_FILE = "_base_config.yml"






task :deploy do |t,args|
    #name = ARGV.last
    #: sets default for deploy_location
    location = 'local'
    location = ARGV.last unless ARGV.length<2 
    task location.to_sym do ; end
    tagmatch = location
    valid_opts={'local'=>'local_base_path','remote'=>'deploy_base_path'}
    location = valid_opts[location]
    if location.nil?
      print "#{location} is not a valid option. Valid options are: #{valid_opts.keys}"
      break
    end

    puts BASE_CONFIG_FILE
    puts Dir.pwd
    config=YAML.load_file(BASE_CONFIG_FILE)
  #puts config

  config["BASE_PATH"]=config[location]
  puts "BASE_PATH set  to #{config["BASE_PATH"]}"
  config["ASSET_PATH"]=File.join(config["BASE_PATH"],"assets")
  config=config.to_yaml
  IO.write "_config.yml" ,"#DO NOT MODIFY THIS FILE DIRECTLY MODIFY #{BASE_CONFIG_FILE} instead\n" + config[3..config.length]

  puts tagmatch
  if tagmatch == 'remote' then
    print "pushing to remote at #{config["JB"]["BASE_PATH"]}"
    %x{jekyll build
      cd _site
      git add .
      git commit -m  "#{Date.today.to_s}"
      git status
      git push origin master
    }
  elsif tagmatch == 'local' then 
    print "serving locally at #{config["JB"]["BASE_PATH"]}"
    Rake::Task["local_preview"].invoke()
  end
end


# Basic setup here copied from octopress rakefile, but it seems to be the standard way of doing this. 
task :local_preview do

jekyllPid = Process.spawn( "jekyll serve -w")
trap("INT") {
     [jekyllPid].each { |pid| Process.kill(9, pid) rescue Errno::ESRCH }
     exit 0
   }

[jekyllPid].each { |pid| Process.wait(pid) }

end