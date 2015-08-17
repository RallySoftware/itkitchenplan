# Kitchenplan Configuration

This is the default Rally IT Kitchenplan configuration repo. This repo contains all configuration to install and configure our OSX workstations. More information about Kitchenplan and on how to use it can be found in the [Kitchenplan README](https://github.com/kitchenplan/kitchenplan).

In order to run Kitchenplan, XCode must be installed on the target client. By default, Kitchenplan will install XCode for you:

-> Installing XCode CLT
         run  sw_vers -productVersion | awk -F "." '{print $2}' from "."
         run  touch /tmp/.com.apple.dt.CommandLineTools.installondemand.in-progress from "."
         run  softwareupdate -l | grep -B 1 "Developer" | head -n 1 | awk -F"*" '{print $2}' from "."
         run  softwareupdate -i  -v from "."


Installation Instructions:

Running Kitchenplan is as easy as running kitchenplan provision

  _  ___ _       _                      _
 | |/ (_) |     | |                    | |
 | ' / _| |_ ___| |__   ___ _ __  _ __ | | __ _ _ __
 |  < | | __/ __| '_ \ / _ \ '_ \| '_ \| |/ _` | '_ \
 | . \| | || (__| | | |  __/ | | | |_) | | (_| | | | |
 |_|\_\_|\__\___|_| |_|\___|_| |_| .__/|_|\__,_|_| |_|
                                 | |
                                 |_|

-> Setting up bundler
      create  /opt/kitchenplan/Gemfile
         run  mkdir -p vendor/cache from "/opt/kitchenplan"
         run  rm -rf vendor/bundle/config from "/opt/kitchenplan"
         run  bundle install --quiet --binstubs vendor/bin --path vendor/bundle from "/opt/kitchenplan"
-> Sending a ping to Google Analytics
-> Compiling configurations
         run  mkdir -p tmp from "/opt/kitchenplan"
-> Fetch the chef cookbooks
         run  vendor/bin/librarian-chef install --clean --quiet --path=vendor/cookbooks from "/opt/kitchenplan"
         run  sudo vendor/bin/chef-solo -c tmp/solo.rb -j tmp/kitchenplan-attributes.json -o applications::create_var_chef_cache,homebrewalt::default,nodejs::default,... from "/opt/kitchenplan"

At this point Chef will start installing everything you configured. Depending on your install list, this might take a while. It will hopefully go smooth and end with

-> Cleanup parsed configuration files
         run  rm -f kitchenplan-attributes.json from "/opt/kitchenplan"
         run  rm -f solo.rb from "/opt/kitchenplan"
=> Installation complete!

----------------------------------------------------------------------------------------------------------------
By default, this configuration will install the following applications via Homebrew:

    -git
    -docker
    -boot2docker
    -chrome
    -firefox
    -iterm2
    -vagrant
    -intellij
    -sublime
    -chefdk
    -inconsolata

Kitchenplan installs applications via Homebrew and is located at /opt/kitchenplan. Additional applications can be added to the configuration by adding them into the default.yml file. Additional repos can be added into the Cheffile. Kitchenplan is intended to be used as a bootstrapping method and not as a patch management solution. It will NOT push updates to clients.
