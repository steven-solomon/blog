---
authors:
- ssolomon
categories:
- Hexagonal
- Cloud Foundry
- Rails
- Whiteboard
- TDD
date: 2017-05-02T19:35:03-04:00
draft: true
short: |
  Deploy Matt Parker's Hexagonal TDD in Rails to Cloud Foundry.
title: Deploying a Hexagonal Rails App to Cloud Foundry
---

You may have seen [Matt Parker's Hexagonal TDD Screencasts](https://www.youtube.com/channel/UCCptggI2qaxsBXiwfit6tNQ). What?!

You haven't?

Well go watch them.

I'll wait....

![alt text](http://memeshappen.com/media/created/I39m-WAITING-meme-24587.jpg "No really...")

Okay, welcome back! So you should have the hexagonal whiteboard app in your file system somewhere
but you may be asking yourself "Self, how do I deploy this app?" Well that is the topic of this
post. We are going to walk through deploying that example code to Pivotal Cloud Foundry.

Let's get started first thing is first. If you don't already have a Pivotal Cloud Foundry account,
you will need to set one up. [Go on](http://run.pivotal.io).

Oh nice! You have one now. Well for this tutorial we are going to be using the
[CF CLI tools](https://docs.cloudfoundry.org/cf-cli/) so install those as well.

Cool now let's sign into the cli.

```
$ cf login
API endpoint: https://api.run.pivotal.io

Email> <Your email>

Password> <Your password>
Authenticating...
OK
```

When prompted select the org you wish to deploy the app to.

To start off lets try to push the app as is to Cloud Foundry. You might be tempted to
push from the root directory. However that will not work. You need to push from the directory
of the Rails application. So navigate to the ```plugins/whiteboard_web/``` directory

```
$ cd plugins/whiteboard_web/
```

Now let's try to push. Replace steve with your name and a random number.

```
$ cf push hexagonal-app-steve
```

You should see that the push fails.
```
2017-05-04T21:20:46.41-0400 [API/4]      OUT Created app with guid fe8127f5-86dd-4c20-af36-4002ff9953b3
2017-05-04T21:20:47.37-0400 [API/3]      OUT Updated app with guid fe8127f5-86dd-4c20-af36-4002ff9953b3 ({"route"=>"da4c371f-76e8-41ca-91c2-d051216951c1", :verb=>"add", :relation=>"routes", :related_guid=>"da4c371f-76e8-41ca-91c2-d051216951c1"})
2017-05-04T21:20:55.89-0400 [API/6]      OUT Updated app with guid fe8127f5-86dd-4c20-af36-4002ff9953b3 ({"state"=>"STARTED"})
2017-05-04T21:20:56.38-0400 [STG/0]      OUT Downloading java_buildpack...
2017-05-04T21:20:56.38-0400 [STG/0]      OUT Downloading binary_buildpack...
2017-05-04T21:20:56.38-0400 [STG/0]      OUT Downloading ruby_buildpack...
2017-05-04T21:20:56.38-0400 [STG/0]      OUT Downloading python_buildpack...
2017-05-04T21:20:56.40-0400 [STG/0]      OUT Downloaded binary_buildpack
2017-05-04T21:20:56.40-0400 [STG/0]      OUT Downloaded python_buildpack
2017-05-04T21:20:56.40-0400 [STG/0]      OUT Downloading staticfile_buildpack...
2017-05-04T21:20:56.41-0400 [STG/0]      OUT Downloading nodejs_buildpack...
2017-05-04T21:20:56.43-0400 [STG/0]      OUT Downloaded staticfile_buildpack
2017-05-04T21:20:56.43-0400 [STG/0]      OUT Downloading dotnet_core_buildpack...
2017-05-04T21:20:56.43-0400 [STG/0]      OUT Downloaded java_buildpack
2017-05-04T21:20:56.43-0400 [STG/0]      OUT Downloading php_buildpack...
2017-05-04T21:20:56.44-0400 [STG/0]      OUT Downloaded ruby_buildpack
2017-05-04T21:20:56.44-0400 [STG/0]      OUT Downloading go_buildpack...
2017-05-04T21:20:56.45-0400 [STG/0]      OUT Downloaded nodejs_buildpack
2017-05-04T21:20:56.45-0400 [STG/0]      OUT Downloading dotnet_core_buildpack_beta...
2017-05-04T21:20:56.46-0400 [STG/0]      OUT Downloaded php_buildpack
2017-05-04T21:20:56.46-0400 [STG/0]      OUT Downloaded go_buildpack
2017-05-04T21:20:56.48-0400 [STG/0]      OUT Downloaded dotnet_core_buildpack
2017-05-04T21:20:56.49-0400 [STG/0]      OUT Downloaded dotnet_core_buildpack_beta
2017-05-04T21:20:56.49-0400 [STG/0]      OUT Creating container
2017-05-04T21:20:56.94-0400 [STG/0]      OUT Successfully created container
2017-05-04T21:20:56.94-0400 [STG/0]      OUT Downloading app package...
2017-05-04T21:20:57.01-0400 [STG/0]      OUT Downloaded app package (342.3K)
2017-05-04T21:20:58.04-0400 [STG/0]      OUT -------> Buildpack version 1.6.38
2017-05-04T21:20:58.40-0400 [STG/0]      OUT        Downloaded [file:///tmp/buildpacks/108af0f2f1e24c8a463a78b4fd81e734/dependencies/https___buildpacks.cloudfoundry.org_dependencies_bundler_bundler-1.14.6-d7236b1f.tgz]
2017-05-04T21:20:58.52-0400 [STG/0]      OUT -----> Supplying Ruby/Rails
2017-05-04T21:20:59.06-0400 [STG/0]      OUT        Downloaded [file:///tmp/buildpacks/108af0f2f1e24c8a463a78b4fd81e734/dependencies/https___buildpacks.cloudfoundry.org_dependencies_ruby_ruby-2.3.4-linux-x64-8939735f.tgz]
2017-05-04T21:20:59.69-0400 [STG/0]      OUT -----> Using Ruby version: ruby-2.3.4
2017-05-04T21:20:59.99-0400 [STG/0]      OUT        Downloaded [file:///tmp/buildpacks/108af0f2f1e24c8a463a78b4fd81e734/dependencies/https___buildpacks.cloudfoundry.org_dependencies_node_node-4.8.3-linux-x64-0622641b.tgz]
2017-05-04T21:21:01.00-0400 [STG/0]      OUT -------> Buildpack version 1.6.38
2017-05-04T21:21:01.22-0400 [STG/0]      OUT -----> Finalizing Ruby/Rails
2017-05-04T21:21:01.33-0400 [STG/0]      OUT ###### WARNING:
2017-05-04T21:21:01.33-0400 [STG/0]      OUT        You have the `.bundle/config` file checked into your repository
2017-05-04T21:21:01.33-0400 [STG/0]      OUT        It contains local state like the location of the installed bundle
2017-05-04T21:21:01.33-0400 [STG/0]      OUT        as well as configured git local gems, and other settings that should
2017-05-04T21:21:01.33-0400 [STG/0]      OUT        not be shared between multiple checkouts of a single repo. Please
2017-05-04T21:21:01.33-0400 [STG/0]      OUT        remove the `.bundle/` folder from your repo and add it to your `.gitignore` file.
2017-05-04T21:21:01.33-0400 [STG/0]      OUT -----> Installing dependencies using bundler 1.14.6
2017-05-04T21:21:01.45-0400 [STG/0]      OUT        Running: bundle install --without development:test --path vendor/bundle --binstubs vendor/bundle/bin --jobs=4 --retry=4 --deployment
2017-05-04T21:21:01.86-0400 [STG/0]      OUT        You are trying to install in deployment mode after changing
2017-05-04T21:21:01.86-0400 [STG/0]      OUT        your Gemfile. Run `bundle install` elsewhere and add the
2017-05-04T21:21:01.86-0400 [STG/0]      OUT        updated Gemfile.lock to version control.
2017-05-04T21:21:01.86-0400 [STG/0]      OUT        the gemspecs for path gems changed
2017-05-04T21:21:01.87-0400 [STG/0]      OUT        Bundler Output: You are trying to install in deployment mode after changing
2017-05-04T21:21:01.87-0400 [STG/0]      OUT        your Gemfile. Run `bundle install` elsewhere and add the
2017-05-04T21:21:01.87-0400 [STG/0]      OUT        updated Gemfile.lock to version control.
2017-05-04T21:21:01.87-0400 [STG/0]      OUT
2017-05-04T21:21:01.87-0400 [STG/0]      OUT        the gemspecs for path gems changed
2017-05-04T21:21:01.87-0400 [STG/0]      OUT  !
2017-05-04T21:21:01.87-0400 [STG/0]      OUT  !     Failed to install gems via Bundler.
2017-05-04T21:21:01.87-0400 [STG/0]      OUT  !
2017-05-04T21:21:01.88-0400 [STG/0]      ERR Failed to compile droplet
2017-05-04T21:21:01.88-0400 [STG/0]      OUT Exit status 223
2017-05-04T21:21:02.15-0400 [STG/0]      OUT Destroying container
2017-05-04T21:21:02.97-0400 [STG/0]      OUT Successfully destroyed container

```

It appears that we didn't run bundle before attempting to push the app.
Let's bundle and try again.

```
$ bundle
Fetching gem metadata from https://rubygems.org/...........
Fetching version metadata from https://rubygems.org/...
Fetching dependency metadata from https://rubygems.org/..
Could not find gem 'whiteboard' in source at
`/Users/stevensolomon/RubymineProjects/hexagonal_tdd_in_ruby/plugins/whiteboard_web/vendor/cache/whiteboard`.
Source does not contain any versions of 'whiteboard'
```

We are greated with a new error. This time it can't find the whiteboard gem.
This makes sense as we have yet to build it. The current dependency graph
for the application is...

[insert static package diagram for whiteboard]

Notice that both the persistence and whiteboard_web plugins depend on the whiteboard gem
and the whiteboard_web plugin also depends on the persistence plugin. We could
do this by hand but that is a lot of steps, let's create a Rakefile so we don't
have to remember them.

Within the root directory of the hexagonal_tdd_in_ruby repo create a ```Rakefile```
Inside of that file add the following task.
```
#/Rakefile

task :build do
  Dir.chdir 'whiteboard' do
    system 'bundle'
  end

  Dir.chdir 'plugins/persistence' do
    system 'bundle'
  end

  Dir.chdir 'plugins/whiteboard_web' do
    system 'bundle package --all'
  end
end
```

If we navigate in the terminal back to the root of the git repo and execute our new
task
```
$ cd -
$ rake build
```

We now have built the entire app. Let's try to push again. Change back to the
```
$ cd plugins/whiteboard_web
$ cf push hexagonal-app-steve
```

We notice yet another error. We have to execute
```
$ cf logs hexagonal-app-steve --recent
```

to see the details. We see that a production database is not configured for the
application.

```
...
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/activerecord-4.2.5/lib/active_record/connection_adapters/connection_specification.rb:248:in `resolve_symbol_connection': 'production' database is not configured. Available: ["development"] (ActiveRecord::AdapterNotSpecified)
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/activerecord-4.2.5/lib/active_record/connection_adapters/connection_specification.rb:211:in `resolve_connection'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/activerecord-4.2.5/lib/active_record/connection_adapters/connection_specification.rb:139:in `resolve'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/activerecord-4.2.5/lib/active_record/connection_adapters/connection_specification.rb:169:in `spec'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/activerecord-4.2.5/lib/active_record/connection_handling.rb:50:in `establish_connection'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/activerecord-4.2.5/lib/active_record/railtie.rb:120:in `block (2 levels) in <class:Railtie>'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/activesupport-4.2.5/lib/active_support/lazy_load_hooks.rb:38:in `instance_eval'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/activesupport-4.2.5/lib/active_support/lazy_load_hooks.rb:38:in `execute_hook'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/activesupport-4.2.5/lib/active_support/lazy_load_hooks.rb:28:in `block in on_load'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/activesupport-4.2.5/lib/active_support/lazy_load_hooks.rb:27:in `each'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/activesupport-4.2.5/lib/active_support/lazy_load_hooks.rb:27:in `on_load'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/activerecord-4.2.5/lib/active_record/railtie.rb:116:in `block in <class:Railtie>'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/railties-4.2.5/lib/rails/initializable.rb:30:in `instance_exec'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/railties-4.2.5/lib/rails/initializable.rb:30:in `run'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/railties-4.2.5/lib/rails/initializable.rb:55:in `block in run_initializers'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/.cloudfoundry/0/ruby-2.3.4/lib/ruby/2.3.0/tsort.rb:228:in `block in tsort_each'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/.cloudfoundry/0/ruby-2.3.4/lib/ruby/2.3.0/tsort.rb:350:in `block (2 levels) in each_strongly_connected_component'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/.cloudfoundry/0/ruby-2.3.4/lib/ruby/2.3.0/tsort.rb:431:in `each_strongly_connected_component_from'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/.cloudfoundry/0/ruby-2.3.4/lib/ruby/2.3.0/tsort.rb:349:in `block in each_strongly_connected_component'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/.cloudfoundry/0/ruby-2.3.4/lib/ruby/2.3.0/tsort.rb:347:in `each'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/.cloudfoundry/0/ruby-2.3.4/lib/ruby/2.3.0/tsort.rb:347:in `call'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/.cloudfoundry/0/ruby-2.3.4/lib/ruby/2.3.0/tsort.rb:347:in `each_strongly_connected_component'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/.cloudfoundry/0/ruby-2.3.4/lib/ruby/2.3.0/tsort.rb:226:in `tsort_each'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/.cloudfoundry/0/ruby-2.3.4/lib/ruby/2.3.0/tsort.rb:205:in `tsort_each'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/railties-4.2.5/lib/rails/initializable.rb:54:in `run_initializers'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/railties-4.2.5/lib/rails/application.rb:352:in `initialize!'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/config/environment.rb:5:in `<top (required)>'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/activesupport-4.2.5/lib/active_support/dependencies.rb:274:in `require'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/activesupport-4.2.5/lib/active_support/dependencies.rb:274:in `block in require'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/activesupport-4.2.5/lib/active_support/dependencies.rb:240:in `load_dependency'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/activesupport-4.2.5/lib/active_support/dependencies.rb:274:in `require'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/config.ru:3:in `block in <main>'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/rack-1.6.4/lib/rack/builder.rb:55:in `instance_eval'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/rack-1.6.4/lib/rack/builder.rb:55:in `initialize'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/config.ru:in `new'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/config.ru:in `<main>'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/rack-1.6.4/lib/rack/builder.rb:49:in `eval'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/rack-1.6.4/lib/rack/builder.rb:49:in `new_from_string'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/rack-1.6.4/lib/rack/builder.rb:40:in `parse_file'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/rack-1.6.4/lib/rack/server.rb:299:in `build_app_and_options_from_config'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/rack-1.6.4/lib/rack/server.rb:208:in `app'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/railties-4.2.5/lib/rails/commands/server.rb:61:in `app'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/rack-1.6.4/lib/rack/server.rb:336:in `wrapped_app'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/rack-1.6.4/lib/rack/server.rb:272:in `start'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/railties-4.2.5/lib/rails/commands/server.rb:80:in `start'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/railties-4.2.5/lib/rails/commands/commands_tasks.rb:80:in `block in server'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/railties-4.2.5/lib/rails/commands/commands_tasks.rb:75:in `tap'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/railties-4.2.5/lib/rails/commands/commands_tasks.rb:75:in `server'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/railties-4.2.5/lib/rails/commands/commands_tasks.rb:39:in `run_command!'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from /home/vcap/app/vendor/bundle/ruby/2.3.0/gems/railties-4.2.5/lib/rails/commands.rb:17:in `<top (required)>'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from bin/rails:4:in `require'
2017-05-04T22:41:02.44-0400 [APP/PROC/WEB/0]ERR 	from bin/rails:4:in `<main>'
...
```

So let's add configuration for production by simply duplicating the development
configuration. This is only temporary we will switch to a real db next.

```
# plugins/whiteboard_web/config/database.yml
development:
  adapter: "sqlite3"
  database: "development.db"

production:
  adapter: "sqlite3"
  database: "production.db"
```


Push the app again..




































--------------------------------------------------
Remove this: this is a result of not using ```bundle package --all```
This time we are met with a new foe.
```
...
sh: 1: /tmp/persistence: not found
 !
 !     There was an error parsing your Gemfile, we cannot continue
 !     The path `/tmp/persistence` does not exist.
 !
Failed to compile droplet
Exit status 223
Destroying container
Successfully destroyed container
