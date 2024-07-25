# Ruby-on-rails-Basics
### Ruby on Rails Guide for Autodidacts

#### Table of Contents

1. [Introduction](#introduction)
2. [Getting Started with Ruby](#getting-started-with-ruby)
   - [Installing Ruby](#installing-ruby)
   - [Running Ruby](#running-ruby)
   - [Interactive Ruby (irb)](#interactive-ruby-irb)
   - [Ruby Basics](#ruby-basics)
3. [Ruby Pocket Reference](#ruby-pocket-reference)
   - [Variables](#variables)
   - [Symbols](#symbols)
   - [Predefined Global Variables](#predefined-global-variables)
   - [Methods](#methods)
   - [Classes](#classes)
4. [Ruby on Rails Basics](#ruby-on-rails-basics)
   - [Setting Up Rails](#setting-up-rails)
   - [Rails Directory Structure](#rails-directory-structure)
   - [Routing](#routing)
   - [Controllers](#controllers)
   - [Views](#views)
   - [Models](#models)
5. [Advanced Rails Topics](#advanced-rails-topics)
   - [Active Record](#active-record)
   - [Migrations](#migrations)
   - [Associations](#associations)
   - [Validations](#validations)
   - [Callbacks](#callbacks)
6. [Testing in Rails](#testing-in-rails)
   - [Unit Testing](#unit-testing)
   - [Integration Testing](#integration-testing)
   - [System Testing](#system-testing)
7. [Deploying Rails Applications](#deploying-rails-applications)
   - [Preparing for Deployment](#preparing-for-deployment)
   - [Deploying to Heroku](#deploying-to-heroku)
   - [Deploying to AWS](#deploying-to-aws)

---

### Introduction

This guide is designed for autodidacts who wish to learn Ruby and Ruby on Rails. It covers both basic and advanced topics, providing a comprehensive resource for self-learners.

### Getting Started with Ruby

#### Installing Ruby

To install Ruby on your machine, follow the instructions for your operating system:

- **Windows**: Download the RubyInstaller from [rubyinstaller.org](https://rubyinstaller.org/).
- **macOS**: Use Homebrew: `brew install ruby`.
- **Linux**: Use your package manager, e.g., `sudo apt install ruby` for Debian-based systems.

Verify the installation by running:

```sh
ruby --version
```

#### Running Ruby

To run Ruby code, you can create a `.rb` file and execute it with the Ruby interpreter:

```sh
ruby filename.rb
```

Alternatively, you can use the Interactive Ruby (irb) shell to execute Ruby code interactively.

#### Interactive Ruby (irb)

Start irb by typing `irb` in your terminal. You can then type Ruby code and see the results immediately.

```sh
irb
```

Example:

```ruby
irb(main):001:0> puts "Hello, world!"
Hello, world!
=> nil
```

#### Ruby Basics

Ruby is an object-oriented scripting language. Everything in Ruby is an object, including numbers, strings, and even classes.

### Ruby Pocket Reference

#### Variables

Ruby supports different types of variables:

- **Local Variables**: Start with a lowercase letter or an underscore.
- **Instance Variables**: Start with `@`.
- **Class Variables**: Start with `@@`.
- **Global Variables**: Start with `$`.

Example:

```ruby
x = 10          # Local variable
@name = "Ruby"  # Instance variable
@@count = 1     # Class variable
$global = true  # Global variable
```

#### Symbols

Symbols are immutable, reusable constants represented by a colon followed by a name.

```ruby
:name
```

#### Predefined Global Variables

Ruby has several predefined global variables. Some of them include:

- `$!`: The error message from the last raised exception.
- `$@`: The stack backtrace from the last raised exception.
- `$_`: The last read line from `gets` or `readline`.

#### Methods

Methods are defined using the `def` keyword. They can take parameters and return values.

```ruby
def greet(name)
  "Hello, #{name}!"
end

puts greet("World") # => "Hello, World!"
```

#### Classes

Classes are defined using the `class` keyword. They encapsulate methods and variables.

```ruby
class Person
  def initialize(name, age)
    @name = name
    @age = age
  end

  def greet
    "Hello, my name is #{@name} and I am #{@age} years old."
  end
end

person = Person.new("Alice", 30)
puts person.greet
```

### Ruby on Rails Basics

#### Setting Up Rails

To install Rails, use the gem command:

```sh
gem install rails
```

Create a new Rails application:

```sh
rails new myapp
```

Navigate into the application directory:

```sh
cd myapp
```

#### Rails Directory Structure

A typical Rails application has the following structure:

- `app`: Contains the main application code.
  - `models`: Contains the application's models.
  - `views`: Contains the application's views.
  - `controllers`: Contains the application's controllers.
- `config`: Configuration files for the application.
- `db`: Database-related files.
- `public`: Static files served by the web server.
- `test`: Test files.

#### Routing

Routing in Rails is handled by the `config/routes.rb` file. It maps URLs to controller actions.

Example:

```ruby
Rails.application.routes.draw do
  root 'welcome#index'
  resources :articles
end
```

#### Controllers

Controllers handle incoming HTTP requests, process them, and return a response.

Example:

```ruby
class ArticlesController < ApplicationController
  def index
    @articles = Article.all
  end

  def show
    @article = Article.find(params[:id])
  end

  def new
    @article = Article.new
  end

  def create
    @article = Article.new(article_params)
    if @article.save
      redirect_to @article
    else
      render 'new'
    end
  end

  private

  def article_params
    params.require(:article).permit(:title, :text)
  end
end
```

#### Views

Views are templates that are rendered by controllers. They are located in the `app/views` directory.

Example:

```erb
<!-- app/views/articles/index.html.erb -->
<h1>Articles</h1>

<ul>
  <% @articles.each do |article| %>
    <li><%= link_to article.title, article %></li>
  <% end %>
</ul>
```

#### Models

Models represent the data and business logic of the application. They interact with the database.

Example:

```ruby
class Article < ApplicationRecord
  validates :title, presence: true,
                    length: { minimum: 5 }
end
```

### Advanced Rails Topics

#### Active Record

Active Record is the ORM layer supplied with Rails. It maps database tables to Ruby classes.

Example:

```ruby
class User < ApplicationRecord
  has_many :posts
end
```

#### Migrations

Migrations are used to change the database schema over time in a consistent and easy way.

Example:

```ruby
class CreateArticles < ActiveRecord::Migration[6.0]
  def change
    create_table :articles do |t|
      t.string :title
      t.text :text

      t.timestamps
    end
  end
end
```

Run migrations with:

```sh
rails db:migrate
```

#### Associations

Associations set up relationships between different models.

Example:

```ruby
class Article < ApplicationRecord
  has_many :comments
end

class Comment < ApplicationRecord
  belongs_to :article
end
```

#### Validations

Validations ensure that only valid data is saved into the database.

Example:

```ruby
class User < ApplicationRecord
  validates :name, presence: true
end
```

#### Callbacks

Callbacks are hooks into the lifecycle of an Active Record object.

Example:

```ruby
class Article < ApplicationRecord
  before_save :capitalize_title

  private

  def capitalize_title
    self.title = title.capitalize
  end
end
```

### Testing in Rails

#### Unit Testing

Unit tests are written for individual components of the application.

Example:

```ruby
require 'test_helper'

class ArticleTest < ActiveSupport::TestCase
  test "should not save article without title" do
    article = Article.new
    assert_not article.save, "Saved the article without a title"
  end
end
```

#### Integration Testing

Integration tests cover multiple components interacting together.

Example:

```ruby
require 'test_helper'

class ArticlesFlowTest < ActionDispatch::IntegrationTest
  test "can create an article" do
    get "/articles/new"
    assert_response :success

    post "/articles", params: { article: { title: "My title", text: "My text" } }
    assert_response :redirect
    follow_redirect!
    assert_response :success
    assert_select "p", "My text"
  end
end
```

#### System Testing

System tests simulate user interactions with the application.

Example:

```ruby
require 'application_system_test_case'

class ArticlesTest < ApplicationSystemTestCase
  test "visiting the index" do
    visit articles_url

    assert_selector "h1", text: "Articles"
  end
end
```

### Deploying

 Rails Applications

#### Preparing for Deployment

Ensure your application is production-ready:

- Set the `RAILS_ENV` to `production`.
- Precompile assets: `rails assets:precompile`.
- Migrate the database: `rails db:migrate`.

#### Deploying to Heroku

To deploy your application to Heroku:

1. Install the Heroku CLI.
2. Log in to Heroku: `heroku login`.
3. Create a new Heroku app: `heroku create`.
4. Deploy your code: `git push heroku main`.
5. Run database migrations: `heroku run rails db:migrate`.

#### Deploying to AWS

To deploy your application to AWS:

1. Set up an EC2 instance.
2. Install Ruby, Rails, and a web server (e.g., Nginx).
3. Deploy your code to the EC2 instance.
4. Configure the web server to serve your Rails application.

---

This guide provides a comprehensive overview of Ruby and Ruby on Rails. For further reading and in-depth tutorials, refer to the provided resources and books. Happy coding!

The guide covers a comprehensive range of topics for both Ruby and Ruby on Rails. However, there's more content that could be included, such as detailed examples, additional advanced topics, and specific case studies or tutorials. If you want to expand on any specific sections or add more advanced content, I can certainly continue to develop the guide further.

Here are a few more sections that could be included:

1. **Advanced Active Record**:
   - Scopes
   - Custom validations
   - Polymorphic associations

2. **API Development**:
   - Building APIs with Rails
   - Serialization with Active Model Serializers
   - API versioning

3. **Authentication and Authorization**:
   - Using Devise for authentication
   - Pundit for authorization

4. **Performance Optimization**:
   - Caching strategies
   - Query optimization
   - Background jobs with Sidekiq

5. **Front-end Integration**:
   - Integrating Rails with React or Vue.js
   - Using Webpacker

6. **Deployment Strategies**:
   - Continuous Integration/Continuous Deployment (CI/CD)
   - Using Docker with Rails
