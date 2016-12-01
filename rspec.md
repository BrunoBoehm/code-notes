# RSPEC
With TDD you descripe the **design and not the implementation/programming**. You think about "what should my code do if it works" you don't care about "how to make it work". We don't care about how it works, we care about the bahavior of the interface.

Start by listing requirements 
I want to be able to ....
this page should list ...


rspec --format=gtml > spec.html
open spec.html

rspec --format=documentation


rspec --init
=> create .rspec
=> create spec/rspec_helper.rb

Create a spec/file for each model in your domain

```ruby
describe Author do
    it 'has a name' do
        # setup
        author = Author.new
        author.name = "Hemingway"
        
        # expectation
        expect(author.name).to eq("Hemingway")
    end
    
    context 'with stories' do
        describe '#stories' do
            it 'has an empty array of stories when initialized/born' do
                author = Author.new
                
                expect(author.stories).to match_array([])
            end
        end
    end
end

---

describe 'Music Library CLI' do
  it 'allows a user to list songs' do
    music_library_controller = MusicLibraryController.new("./spec/fixtures/mp3s")

    expect(MusicLibraryController).to receive(:new).and_return(music_library_controller)
    expect(music_library_controller).to receive(:gets).and_return("list songs", "exit")

    output = capture_puts {run_file("./bin/musiclibrary")}

    expect(output).to include("1. Action Bronson - Larry Csonka - indie")
  end
end  

---

describe "Song Basics" do
  describe '#initialize with #name' do
    it 'accepts a name for the song' do
      song = Song.new("In the Aeroplane Over the Sea")
      expect(song.name).to eq("In the Aeroplane Over the Sea")
    end
  end

  describe '#name=' do
    it "sets the song name" do
      song = Song.new("In the Aeroplane Over the Sea")
      song.name = "Jump Around"

      expect(song.name).to eq("Jump Around")
    end
  end

  describe '.all' do
    it 'returns the class variable @@all' do
      Song.class_variable_set(:@@all, [])

      expect(Song.all).to match_array([])
    end
  end

  describe '.destroy_all' do
    it 'resets the @@all class variable to an empty array' do
      Song.class_variable_set(:@@all, ["Song"])

      Song.destroy_all
      expect(Song.all).to match_array([])
    end
  end

  describe '#save' do
    it 'adds the song instance to the @@all class variable' do
      song = Song.new("In the Aeroplane Over the Sea")

      song.save

      expect(Song.all).to include(song)
    end
  end

  describe '.create' do
    it 'initializes and saves the song' do
      song = Song.create("In the Aeroplane Over the Sea")

      expect(Song.all).to include(song)
    end
  end
end
```
`context` and `describe` are interchangeable, it's more about the readability ("I'm describing Authors with stories, it has...").

rspec --fail-fast

Write the minimum code to make the test pass

Make sure your `spec_helper` knows about the files it needs to test, it's best practice to require in `spec_helper` the environment `require_relative '../config/environment'`.

```ruby
expect(author.name).to eq("Hemingway")
expect(@artist.songs.count).to eq(1)
expect(@song.artist).to eq(@artist)
expect(page.status_code).to eq(200)
expect(rendered).to match(/Title/)

expect(author.stories).to match_array([])
expect(artist.genres).to match_array([indie_rock, electro_pop])

expect(defined?(Concerns::Findable)).to be_truthy
expect(artist.instance_variable_defined?(:@genres)).to be_falsey

expect(Concerns::Findable).to be_a(Module)
expect(song_1).to be_a(Song)
expect(assigns(:post)).to be_a_new(Post)

expect(assigns(:post)).to be_persisted

expect(Artist).to respond_to(:find_by_name)

expect(post).to be_valid

expect(author.stories).to include(story)
expect(output).to include("Action Bronson")

expect{author.add_story(story)}.to raise_error(InventedCustomError)

expect(@artist).to be_an_instance_of(Artist)

expect{
    post :create, { :first_name => "Sam", :last_name => "Smith" }
    }.to change(Student,:count).by(1)
    
expect {
    delete :destroy, {:id => post.to_param}, valid_session
    }.to change(Post, :count).by(-1)    

expect(music_library_controller).to receive(:gets).and_return("list songs", "exit")

expect(response).to redirect_to(Post.last)
expect(:get => "/posts").to route_to("posts#index")

expect{MusicLibraryController.new('./spec/fixtures/mp3s')}.to_not raise_error

expect(MusicImporter).to receive(:new).with('./db/mp3s').and_return(MusicImporter.new('./db/mp3s'))

expect(response).to redirect_to Student.last
```

## Before and After actions

```ruby
describe FiguresController do
  before do
    queenb = Figure.create(:name => "Beyonce")
    kanye = Figure.create(:name => "Kanye")
    bqe = Landmark.create(name: 'BQE', year_completed: 1961)
    mr_president = Title.create(name: "Mr. President")
    bqe.figure = queenb
    bqe.save
  end

  after do
    Figure.destroy_all
    Title.destroy_all
    Landmark.destroy_all
  end
  
  let(:valid_attributes) {
    {title: "My String", description: "My desc"}
  }
  let(:valid_session) { {} }
  
  before(:each) do
    @post = assign(:post, Post.create!(
      :title => "MyString"
    ))
  end  

  it "allows you to view form to create a new figure" ...
  
end  
```

It is also possible to generate symbols thta can be used in all of the following tests.
```ruby
describe 'Song' do 
  let(:song) {Song.new('Man in the Mirror')}
  let(:file_name) {'Michael Jackson - Black or White - pop.mp3'}
  ...
end  
```

## Capybara
Capybara gives us a few methods to simulate how the user would interact with the page:
- `visit` & `page` – Capybara methods for controlling the test user's browser and examining the current state of the page as they 'see' it.
- `have_selector`, `have_field`, & `have_text` – Capybara matchers for ensuring that the page contains certain matching HTML or text.
- `visit`, `fill_in` & `click_button` – Capybara methods for mimicking user activities, such as visiting a page, filling in form fields or clicking buttons.
```ruby
visit '/'
fill_in(:user_name, with: "Avi")
fill_in :figure_name, :with => "Doctor Who"
fill_in 'post_title', with: 'My post title'
check "title_#{Title.first.id}"
click_button "Submit"

expect(page.body).to include("Welcome!")
expect(page.current_path).to eq(student_path(@student))

expect(page).to have_text("Hi Avi, nice to meet you!")
expect(page).to have_css("h1", text: "My Post")
expect(page).to have_selector("form")
expect(page).to have_content('Post Form')
expect(page).to have_field(:user_name)
expect(page).to have_link(second_post.title, href: post_path(second_post))
```

As you are updating the code, make sure to test it out in the browser – don't just rely on the tests. It's important to see the errors in both the tests and the browser since you'll want to become familiar with both types of failure messages.

## About the spec-helper
Each spec file begins by `require 'spec_helper'`.
```ruby
ENV["SINATRA_ENV"] = "test"

require_relative '../config/environment'
require 'rack/test'
require 'capybara/rspec'
require 'capybara/dsl'

if defined?(ActiveRecord::Migrator) && ActiveRecord::Migrator.needs_migration?
  raise 'Migrations are pending run `rake db:migrate SINATRA_ENV=test` to resolve the issue.'
end

RSpec.configure do |config|
  config.run_all_when_everything_filtered = true
  config.filter_run :focus
  config.include Rack::Test::Methods
  config.include Capybara::DSL
  config.order = 'default'
end

ActiveRecord::Base.logger.level = 1

def app
  Rack::Builder.parse_file('config.ru').first
end

Capybara.app = app
```

## Rails Generators
A primary goal of the Rails team was to make it efficient to build core application functionality. The Rails system has a number of generators that will do some of the manual work for us, with nice extra benefits:
- They can setup some basic specs for an application's test suite. They won't write our complex logic tests for us, but they will provide some basic examples.
- They are setup to work the same way each time. This helps standardize your code and enables your development to be more efficient since you don't have to worry as much about bugs related to spelling, syntax errors, or anything else that can occur when writing code manually.
- They follow Rails best practices, which includes utilizing RESTful naming patterns, removing duplicate code, using partials and a number of other best of breed design patterns (if you don't know what all of these are, don't worry, we will cover them shortly).

Careful though: certain generators create quite a bit of code, and if that code is not going to be used it will clutter the application code and cause confusion for future developers.

Each of the generators are entered into the terminal and will follow the syntax:
```
rails generate <name of generator>
rails g <name of generator>

rails g migration add_published_status_to_posts published_status:string
rails g migration remove_published_status_from_posts published_status:string
rails g migration add_post_status_to_posts post_status:boolean
rails g migration change_post_status_data_type_to_posts post_status:string
```

Model generators are used regularly. It does a great job of creating the core code needed to create a model and associated database table without adding a lot of bloat to the application.
At a high level, this creates:
- A database migration that will add a table and add the columns name, genre, and bio.
- A model file that will inherit from ActiveRecord::Base
- Some mocked RSpec test files
```
rails g model Author name:string genre:string bio:text
invoke  active_record
create    db/migrate/20151127225446_create_authors.rb
create    app/models/author.rb
invoke    rspec
create      spec/models/author_spec.rb
invoke      factory_girl
create        spec/factories/authors.rb
```

Controller generators are great if you are creating static views or non-CRUD related features. It will create a ton of code
Below is a list that is a little more high level:
- A controller file that will inherit from ApplicationController
- A set of routes to each of the generator arguments: dashboard, stats, financials, and settings
- A new directory for all of the view templates along with a view template file for each of the controller actions that we declared in the generator command
- A number of view based tests
- A view helper method file and spec helper file
- A Coffeescript file for specific JavaScripts for that controller
- A scss file for the styles for the controller
```
rails g controller admin dashboard stats financials settings
create  app/controllers/admin_controller.rb
 route  get 'admin/settings'
 route  get 'admin/financials'
 route  get 'admin/stats'
 route  get 'admin/dashboard'
invoke  erb
create    app/views/admin
create    app/views/admin/dashboard.html.erb
create    app/views/admin/stats.html.erb
create    app/views/admin/financials.html.erb
create    app/views/admin/settings.html.erb
invoke  rspec
create    spec/controllers/admin_controller_spec.rb
create    spec/views/admin
create    spec/views/admin/dashboard.html.erb_spec.rb
create    spec/views/admin/stats.html.erb_spec.rb
create    spec/views/admin/financials.html.erb_spec.rb
create    spec/views/admin/settings.html.erb_spec.rb
invoke  helper
create    app/helpers/admin_helper.rb
invoke    rspec
create      spec/helpers/admin_helper_spec.rb
invoke  assets
invoke    coffee
create      app/assets/javascripts/admin.coffee
invoke    scss
create      app/assets/stylesheets/admin.scss
```
This is a generator to be careful with – it can create a number of files that are never used and can cause wasted files in an application.

If you are building an API, using a front end MVC framework, or simply want to manually create your views, the resource generator is a great option for creating the code. The resource generator is a smart generator that creates some of the core functionality needed for a full featured resource without much code bloat.
```
rails g resource Account name:string payment_status:string
invoke  active_record
create    db/migrate/20151127233150_create_accounts.rb
create    app/models/account.rb
invoke    rspec
create      spec/models/account_spec.rb
invoke      factory_girl
create        spec/factories/accounts.rb
invoke  controller
create    app/controllers/accounts_controller.rb
invoke    erb
create      app/views/accounts
invoke    rspec
create      spec/controllers/accounts_controller_spec.rb
invoke    helper
create      app/helpers/accounts_helper.rb
invoke      rspec
create        spec/helpers/accounts_helper_spec.rb
invoke    assets
invoke      coffee
create        app/assets/javascripts/accounts.coffee
invoke      scss
create        app/assets/stylesheets/accounts.scss
invoke  resource_route
route    resources :accounts
```
`resources :accounts` is considered a 'magic' route that entails the full set of RESTful routes needed to perform CRUD in an application. Let's see those routes
```
rake routes | grep account
accounts      GET    /accounts(.:format)          accounts#index
              POST   /accounts(.:format)          accounts#create
new_account   GET    /accounts/new(.:format)      accounts#new
edit_account  GET    /accounts/:id/edit(.:format) accounts#edit
account       GET    /accounts/:id(.:format)      accounts#show
              PATCH  /accounts/:id(.:format)      accounts#update
              PUT    /accounts/:id(.:format)      accounts#update
              DELETE /accounts/:id(.:format)      accounts#destroy
```

## Scaffolding
It's not considered a good practice to use scaffolds in a production application. It's important to study scaffolds since they can be a great reference for how we can build CRUD functionality into our apps.
```
rails g scaffold Article title:string body:text
invoke  active_record
create    db/migrate/20151128001950_create_articles.rb
create    app/models/article.rb
invoke    rspec
create      spec/models/article_spec.rb
invoke      factory_girl
create        spec/factories/articles.rb
invoke  resource_route
 route    resources :articles
invoke  scaffold_controller
create    app/controllers/articles_controller.rb
invoke    erb
create      app/views/articles
create      app/views/articles/index.html.erb
create      app/views/articles/edit.html.erb
create      app/views/articles/show.html.erb
create      app/views/articles/new.html.erb
create      app/views/articles/_form.html.erb
invoke    rspec
create      spec/controllers/articles_controller_spec.rb
create      spec/views/articles/edit.html.erb_spec.rb
create      spec/views/articles/index.html.erb_spec.rb
create      spec/views/articles/new.html.erb_spec.rb
create      spec/views/articles/show.html.erb_spec.rb
create      spec/routing/articles_routing_spec.rb
invoke      rspec
create        spec/requests/articles_spec.rb
invoke    helper
create      app/helpers/articles_helper.rb
invoke      rspec
create        spec/helpers/articles_helper_spec.rb
invoke    jbuilder
create      app/views/articles/index.json.jbuilder
create      app/views/articles/show.json.jbuilder
invoke  assets
invoke    coffee
create      app/assets/javascripts/articles.coffee
invoke    scss
create      app/assets/stylesheets/articles.scss
invoke  scss
create    app/assets/stylesheets/scaffolds.scss
```
Scaffolds actually go beyond the other generators and create both the front- and back-end code needed for CRUD features.
What did the scaffold build for us? If we look through the files that got printed out in the console, we see:
- A migration file
- A Model file
- A controller
- View templates for each of the controller actions that render a view
- The full set of RESTful routes
- And every other component needed for a functional CRUD environment

It's interesting to see how Rails set up the controller. Below are the contents of the articles_controller.rb file:
```ruby
class ArticlesController < ApplicationController
  before_action :set_article, only: [:show, :edit, :update, :destroy]
 
  # GET /articles
  # GET /articles.json
  def index
    @articles = Article.all
  end
 
  # GET /articles/1
  # GET /articles/1.json
  def show
  end
 
  # GET /articles/new
  def new
    @article = Article.new
  end
 
  # GET /articles/1/edit
  def edit
  end
 
  # POST /articles
  # POST /articles.json
  def create
    @article = Article.new(article_params)
 
    respond_to do |format|
      if @article.save
        format.html { redirect_to @article, notice: 'Article was successfully created.' }
        format.json { render :show, status: :created, location: @article }
      else
        format.html { render :new }
        format.json { render json: @article.errors, status: :unprocessable_entity }
      end
    end
  end
 
  # PATCH/PUT /articles/1
  # PATCH/PUT /articles/1.json
  def update
    respond_to do |format|
      if @article.update(article_params)
        format.html { redirect_to @article, notice: 'Article was successfully updated.' }
        format.json { render :show, status: :ok, location: @article }
      else
        format.html { render :edit }
        format.json { render json: @article.errors, status: :unprocessable_entity }
      end
    end
  end
 
  # DELETE /articles/1
  # DELETE /articles/1.json
  def destroy
    @article.destroy
    respond_to do |format|
      format.html { redirect_to articles_url, notice: 'Article was successfully destroyed.' }
      format.json { head :no_content }
    end
  end
 
  private
    # Use callbacks to share common setup or constraints between actions.
    def set_article
      @article = Article.find(params[:id])
    end
 
    # Never trust parameters from the scary internet, only allow the white list through.
    def article_params
      params.require(:article).permit(:title, :body)
    end
end
```

The main reason why scaffolds are discouraged in production is because they create so much code and so many files that they can be hard to manage. The best applications were built by following TDD principles where I created features one element at a time, whereas scaffolds build dozens of processes instantly.

A good alternative is `resource` generators. The scaffold generator tends to be very 'opinionated' with the code that it builds, it builds out the system in a very specific manner, which is rarely the way that you would want to build your application.

With modern development practices, a large number of Rails apps are leveraging client side MVC setups such as `Backbone` or `AngularJS`. These frameworks render the view templates for a Rails application pointless, so if you rely on using scaffolds you're going to have to be removing quite a bit of code after each generate command.

Let's pretend that you're integrating the `ReactJS` framework into your Rails application. If you use a scaffold you will instantly have to go through the code and remove a large percentage of the code along with around 50% of the files themselves. Whereas if you run the resource generator, it simply creates the: migrations, model, routes, controller, and asset pipeline files. This means that you will be able to instantly start implementing the ReactJS components instead of having to be concerned with which elements need to be deleted.

All the files that you currently see in your project directory, with the exception of a few `spec` files, were generated via the `rails new` command. Consequently, it built a fully fledged web app ready out of the box. This is an example of why the Rails framework is called an opinionated framework that **favors convention over configuration**. With Sinatra, we can build an app from scratch by just adding `require 'sinatra'` in a file and calling `rackup`. Rails is much heavier than that. With an app ready out of the box through generators like rails new and rails generate scaffold, we're able to build larger scale applications very quickly.

For instance, say you were making an app to help New Yorkers find apartments. Your apartments should have an address (string), a price (float), a description (text), and an image url (string). You want to make the following things:

1. an apartment model
2. an apartments table with four columns (address, price, description, image_url)
3. an apartments controller
4. routes for apartments (show, new, index, update, edit, delete, create)
5. views (show, new, edit, index)

To make all these in one command, you would run:
```
> rails generate scaffold Apartment address:string price:float description:text image_url:string
```



















