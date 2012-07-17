require 'pivotal-tracker'
require 'github_api'
require 'yaml'
require 'httparty'


# add overall tasks to toggl
# add same tasks as epics to PT
# set up rails and some basic cap tasks
# add basic info into github readme - completion date, other stuff
# set-up system so it only needs an email

# inherited http class to the toggl api
class Toggl
  include HTTParty
  
  #debug_output $stderr #debugger to check httparty output
  base_uri 'https://www.toggl.com'
  basic_auth YAML.load_file("config.yaml")["config"]["toggl_token"], 'api_token'
  headers "Content-Type" => "application/json"
end



  desc "setting up all the code and repo stuff to start a live project"
  namespace :live do
    desc "setup github, PT and toggl in one go"
    task :setup_all, :name, :description do 
      #grab vars from commandline
    
      @@proj_name = ENV["name"]
      @@proj_description = ENV["description"]
    
      Rake::Task['informatics:setup_toggl'].invoke
      Rake::Task['informatics:setup_PT'].invoke
      Rake::Task['informatics:setup_github'].invoke
    
      puts 'completes all setup'
    end

    desc "setup toggl"
    task :setup_toggl do
      # options for the toggle post
  
      config = YAML.load_file("config.yaml")
      @toggl_ws_id = config["config"]["toggl_ws_id"]
  
      options = {:body => 
        {
        :project => 
          {
          :billable => true, 
          :workspace => {:id => @toggl_ws_id}, 
          :name => @@proj_name, 
          :automatically_calculate_estimated_workhours => false
          }
          }, :headers => {'ContentType' => "application/json"}}


      responser = Toggl.post('/api/v6/projects.json', options)
      puts "#{@@proj_name} toggl project done  - BOOM"
    end

    desc "setup pivotal tracker"
    task :setup_PT do
  
      config = YAML.load_file("config.yaml")
      @pt_token = config["config"]["pt_token"]
  
      # set up PT project
      PivotalTracker::Client.token = @pt_token
      new_proj = PivotalTracker::Project.new(:name => @@proj_name)
      # create new proj in PT
      new_proj.create()

      puts "#{@@proj_name} Pivotal T completed - BOOOOOOOM"
    end

    desc "setup github repo"
    task :setup_github do
  
      config = YAML.load_file("config.yaml")
      @gh_un = config["config"]["gh_un"]
      @gh_pw = config["config"]["gh_pw"]
      @gh_org = config["config"]["gh_org"]
  
      # setup github repo
      github = Github.new :basic_auth => "#{@gh_un}:#{@gh_pw}", :org => @gh_org
      github.repos.create :name => @@proj_name, :description => @@proj_description, :org => @gh_org

      puts "#{@@proj_name} github repo created - BOOOOOOOOOOOOOOOOOOM"
    end
  end
  
  desc "setting up all the folders and documents needed to complete the planning phase"
  namespace :planning do
    # stick checklist in the folder
    # set-up basic folder structure docs, docs/specs, docs/wireframes, docs/

    desc "setup all planning folders and docs"
    task :setup_all do
      @@proj_name = ENV["name"]
      Rake::Task['planning:setup_project_folder'].invoke
    end
  
    desc "setup folders for the project"
    task :setup_project_folder do
      config = YAML.load_file("config.yaml")
      db_folder = config["config"]["dropbox_folder"]
      
      base_dir = "#{db_folder}/planning/#{@@proj_name}"
    
      mkdir base_dir
      mkdir "#{base_dir}/docs" unless File.exists?("#{base_dir}/docs")
      mkdir "#{base_dir}/docs/specs" unless File.exists?("#{base_dir}/docs/specs")
      mkdir "#{base_dir}/docs/wireframes" unless File.exists?("#{base_dir}/docs//wireframes")
      mkdir "#{base_dir}/docs/designs" unless File.exists?("#{base_dir}/docs/designs")
      cp "#{db_folder}/planning/Project_checklist.taskpaper", "#{base_dir}/checklist.taskpaper"
      
     
    end
  
  end
  
  
 

