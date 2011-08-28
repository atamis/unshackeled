
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

  def awesome
    "awesome"
  end
end

def convert_master_template
  master = File.open('source/master.html.erb') { |f| f.read }
  erb = Erubis::Eruby.new(master)
  result = erb.result(TemplatingHelpers.new.getBinding)
  File.open('unshackaled.html', 'w+') {|f| f.write(result) }
end

desc "Convert the master template"
task :master_template do
  convert_master_template
end

desc "Preview the file"
task :preview do

  puts ">>> Watching for changes"
  FSSM.monitor(File.dirname(__FILE__), ['**/*.html.erb']) do
    update do
      puts "Updating..."
      convert_master_template
    end
  end

end

desc "Default task"
task :default => [:master_template]do
  
end

