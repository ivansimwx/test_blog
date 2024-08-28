# README

This README would normally document whatever steps are necessary to get the
application up and running.

Things you may want to cover:

* Ruby version

* System dependencies

* Configuration

* Database creation

* Database initialization

* How to run the test suite

* Services (job queues, cache servers, search engines, etc.)

* Deployment instructions

* ...

# Notes on this lesson
# to start a server
$ bin/rails server 

# in windows, you have to pass the scripts under the bin folder directly to the Ruby interpreter
ruby bin\rails server to

# This will start up Puma, a web server distributed with Rails by default. To see your application in action, open a browser window and navigate to http://localhost:3000.

# Let's start by adding a route to our routes file, config/routes.rb, at the top of the Rails.application.routes.draw block:

# Rails.application.routes.draw do
  get "/articles", to: "articles#index"
  end

#The route above declares that GET /articles requests are mapped to the index action of ArticlesController.
To create ArticlesController and its index action, we'll run the controller generator (with the --skip-routes option because we already have an appropriate route):
$ bin/rails generate controller Articles index --skip-routes

# Rails will create several files for you:
The most important of these is the controller file, app/controllers/articles_controller.rb. Let's take a look at it:
The index action is empty. When an action does not explicitly render a view (or otherwise trigger an HTTP response), Rails will automatically render a view that matches the name of the controller and action. Convention Over Configuration! Views are located in the app/views directory. So the index action will render app/views/articles/index.html.erb by default.

Let's open app/views/articles/index.html.erb, and replace its contents with:
<h1>Hello, Rails!</h1>


#kill -9 pidnumber to end server

#At the moment, http://localhost:3000 still displays a page with the Ruby on Rails logo. Let's display our "Hello, Rails!" text at http://localhost:3000 as well. To do so, we will add a route that maps the root path of our application to the appropriate controller and action.

Let's open config/routes.rb, and add the following root route to the top of the Rails.application.routes.draw block:
Rails.application.routes.draw do
  root "articles#index"

  get "/articles", to: "articles#index"
end

# Rails applications do not use require to load application code.

You may have noticed that ArticlesController inherits from ApplicationController, but app/controllers/articles_controller.rb does not have anything like

require "application_controller" # DON'T DO THIS.
Copy
Application classes and modules are available everywhere, you do not need and should not load anything under app with require. This feature is called autoloading, and you can learn more about it in Autoloading and Reloading Constants.

You only need require calls for two use cases:

To load files under the lib directory.
To load gem dependencies that have require: false in the Gemfile.

# So far, we've discussed routes, controllers, actions, and views. All of these are typical pieces of a web application that follows the MVC (Model-View-Controller) pattern. MVC is a design pattern that divides the responsibilities of an application to make it easier to reason about. Rails follows this design pattern by convention.

Since we have a controller and a view to work with, let's generate the next piece: a model.

A model is a Ruby class that is used to represent data. Additionally, models can interact with the application's database through a feature of Rails called Active Record.

To define a model, we will use the model generator:
$ bin/rails generate model Article title:string body:text

This will create several files. The two files we'll focus on are the migration file (db/migrate/<timestamp>_create_articles.rb) and the model file (app/models/article.rb).


# Model names are singular, because an instantiated model represents a single data record. To help remember this convention, think of how you would call the model's constructor: we want to write Article.new(...), not Articles.new(...).

# Migrations are used to alter the structure of an application's database. In Rails applications, migrations are written in Ruby so that they can be database-agnostic.

Let's take a look at the contents of our new migration file:
(db > migrate > timestamp_create_articles.rb)
Migrations are used to alter the structure of an application's database. In Rails applications, migrations are written in Ruby so that they can be database-agnostic.


class CreateArticles < ActiveRecord::Migration[7.2]
  def change
    create_table :articles do |t|
      t.string :title
      t.text :body

      t.timestamps
    end
  end

# The call to create_table specifies how the articles table should be constructed. By default, the create_table method adds an id column as an auto-incrementing primary key. So the first record in the table will have an id of 1, the next record will have an id of 2, and so on.

Inside the block for create_table, two columns are defined: title and body. These were added by the generator because we included them in our generate command (bin/rails generate model Article title:string body:text).

On the last line of the block is a call to t.timestamps. This method defines two additional columns named created_at and updated_at. As we will see, Rails will manage these for us, setting the values when we create or update a model object.

Let's run our migration with the following command:
bin/rails db:migrate
Now we can interact with the table using our model.

To play with our model a bit, we're going to use a feature of Rails called the console. The console is an interactive coding environment just like irb, but it also automatically loads Rails and our application code.

Let's launch the console with this command:
bin/rails console



end
