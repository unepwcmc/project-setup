project-set-up
==============

some scripts to set-up out various apps and folders

git clone git://github.com/unepwcmc/project-setup.git
gem install bundler 
bundle install

rename example_config.yaml to config.yaml and fill in the tokens and password



to set up toggle, github and pivotal tracker:

`$rake -v informatics:setup_all name=myproject description="geoffs app"`

to set up the project docs and directory for the planning phase:

`rake planning:setup_all name=myproject`

next-up:  wireframing, usecases
