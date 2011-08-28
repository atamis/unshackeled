
require "bundler"
Bundler.require(:default)

class Object
  def getBinding
    binding()
  end
end

class TemplatingHelpers
  def initialize

  end

  def load path
    puts "Processing #{path}"
    path = path.split("/")
    path.unshift("source")
    path = File.join(*path)
    source = File.open(path, 'r') { |f| process_erb(f.read) }
    return source
  end

  def load_css(path)
    "<!-- #{path} -->\n<style type=\"text/css\">" + load(path) + '</style>'
  end

  def load_js path
    "<!-- #{path} -->\n<script type=\"text/javascript\">" + load(path) + '</script>'
  end

  def awesome
    "awesome"
  end

  def javascripts
    ["modernizr.min.js", "jquery.min.js", "underscore-min.js", "backbone-min.js"]
  end
end

def process_erb(input)
  erb = Erubis::Eruby.new(input)
  begin
  result = erb.result(TemplatingHelpers.new.getBinding)
  rescue Exception => e
    return input
  end
end

def convert_master_template
  puts "Processing master.html.erb"
  master = File.open('source/master.html.erb') { |f| f.read }
  result = process_erb(master)
  File.open('unshackaled.html', 'w+') {|f| f.write(result) }
end

desc "Convert the master template"
task :master_template do
  convert_master_template
end

desc "Preview the file"
task :preview do

  puts ">>> Watching for changes"
  FSSM.monitor(File.dirname(__FILE__), ['**/*']) do
    update do
      puts "Updating..."
      begin
        convert_master_template
      rescue Exception => e
        puts e
      end
    end
  end

end

desc "Default task"
task :default => [:master_template]do
  
end

