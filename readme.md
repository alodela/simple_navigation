# Simple Navigation [![](http://stillmaintained.com/mexpolk/simple_navigation.png)](http://stillmaintained.com/mexpolk/simple_navigation)

Ruby on Rails gem/plugin for drop down/tabbed navigation.

## Requirements

* Rails 2.3.x (may work with older versions, not tested)

## Install

Add the following line to your /config/environment.rb file:

    config.gem "mexpolk-simple_navigation",
        :lib => "simple_navigation",
        :source => "http://gems.github.com"

And from the command line run:

    rake gems:install

### Install as a Plugin

If your rather prefer to install it as a plugin, from your application
directory simply run:

    script/plugin install git://github.com/mexpolk/simple_navigation.git

And you're done!

## Usage

To create your menus create config/initializers/simple_navigation.rb file like
this:

    SimpleNavigation::Builder.config do |map|
        map.navigation :default do |navigation|

            # Root menu without child elements (menus) that points to /dashboard
            navigation.menu :home, :url => { :controller => "home", :action => "index"}

            # Root menu with child menus without anchor link
            navigation.menu :contacts do |contacts|

                # Child menu with many possible urls (or many controllers and actions)
                contacts.menu :list, :url => { :controller => "contacts", :action => "index" } do |contact_list|

                    # This menu will marked as current when you're on the following
                    # controllers and actions (including the controller and action
                    # specified in the :url option):
                    contact_list.connect :controller => "contacts" # ...current on any action from this controller
                    contact_list.connect :controller => "people", :except => "new"
                    contact_list.connect :controller => "companies", :except => "new"

                end

                # Another submenu that points to /person/new
                contacts.menu :new_person, :url => { :controller => "people", :action => "new" }

                # Another submenu that points to /company/new
                contacts.menu :new_company, :url => { :controller => "companies", :action => "new" }

            end

            # Another root menu with nested submenus
            navigation.tab :admin, :url => { :controller => "users", :action => "index" } do |admin|
                admin.menu :users, :url => { :controller => "users", :action => "index" } do |users|
                    users.menu :reports, :url => { :controller => "user_reports", :action => "index" } do |reports|
                        reports.menu :activity, :url => { :controller => "user_reports", :action => "activity" }
                        reports.menu :login_atempts, :url => { :controller => "user_reports", :action => "login_atempts" }
                    end
                    users.menu :new_user, :url => { :controller => "users", :action => "new" }
                end
            end
        end
    end

To render you newly created menu called :default, in your default layout
(layout/application.html.erb):

    <%= simple_navigation :default %>

## Internationalization (i18n)

If you want to use internationalization, set the option :i18n => true like this:

    SimpleNavigation::Builder.config do |map|
        map.navigation :default, :i18n => true do |navigation|
            ...
        end
    end

And add to your config/locales files (e.g. es-MX.yml) the following:

    es-MX:
        simple_navigation:
            default:                         # The name of your navigation menu
                home:                          # The name of your root menu
                    title: "Inicio"              # The translated title for your root menu
                    menus:
                        index: "Panel de Control"  # The title for index action child menu
                        new: "Nueva Página"        # The title for new action child menu


## License

Copyright (c) 2008 Iván "Mexpolk" Torres

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
