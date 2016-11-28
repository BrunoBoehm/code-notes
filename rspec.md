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

expect(author.stories).to match_array([])
expect(artist.genres).to match_array([indie_rock, electro_pop])

expect(defined?(Concerns::Findable)).to be_truthy
expect(artist.instance_variable_defined?(:@genres)).to be_falsey

expect(Concerns::Findable).to be_a(Module)
expect(song_1).to be_a(Song)

expect(Artist).to respond_to(:find_by_name)

expect(post).to be_valid

expect(author.stories).to include(story)
expect(output).to include("Action Bronson")

expect{author.add_story(story)}.to raise_error(InventedCustomError)

expect(@artist).to be_an_instance_of(Artist)

expect(music_library_controller).to receive(:gets).and_return("list songs", "exit")

expect{MusicLibraryController.new('./spec/fixtures/mp3s')}.to_not raise_error

expect(MusicImporter).to receive(:new).with('./db/mp3s').and_return(MusicImporter.new('./db/mp3s'))
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
check "title_#{Title.first.id}"
click_button "Submit"

expect(page.body).to include("Welcome!")

expect(page).to have_text("Hi Avi, nice to meet you!")
expect(page).to have_css("h1", text: "My Post")
expect(page).to have_selector("form")
expect(page).to have_field(:user_name)
```

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



