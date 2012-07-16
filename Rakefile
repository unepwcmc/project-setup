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



namespace :informatics do
  
  desc "setup github, PT and toggl in one go"
  task :setup_all => [:setup_toggl, :setup_PT, :setup_github] do
    #grab vars from commandline
    @@proj_name = ARGV[0]
    @@proj_description = ARGV[1]
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
    puts "#{proj_name} toggl project done  - BOOM"
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

    puts "#{proj_name} Pivotal T completed - BOOOOOOOM"
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

    puts "#{proj_name} github repo created - BOOOOOOOOOOOOOOOOOOM"
  end
 
end
