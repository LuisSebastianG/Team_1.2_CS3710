# Setting Up Capybara and FactoryBot to simulate real-world user actions in your tests

## Required Gems in Gemfile

In your Gem file add or insure these gems are in the `Gemfile`:

```ruby
# Gemfile
group :development, :test do
  gem 'rspec-rails', '~> 6.0'
  gem 'factory_bot_rails'
  gem 'faker'
end

group :test do
  gem "capybara"
  gem "selenium-webdriver"
end
```
if you added any gems run `bundle install` to install the new gems.

## Setting Up System Testing

Follow these steps to configure system testing with RSpec, Capybara, FactoryBot, and Faker.

1. Generate a System Test File
    
    Generate a test file by running `rails generate rspec:system student`
    
    This will create a new test file under the `spec/system` directory called `students_spec.rb` or something similar. 
2. Generate and Configure the Required Support Files

    Add a folder called `support` under the `spec/` directory and add two files: `chrome.rb`, which sets up Capybara with Selenium, and `factory_bot.rb`, which configures FactoryBot.

    In the `spec/support/chrome.rb` file add this code:
    ```ruby
    # spec/support/chrome.rb
    RSpec.configure do |config|
        config.before(:each, type: :system) do
            driven_by :selenium, using: :headless_chrome do |driver_options|
                driver_options.add_argument('--headless')
                driver_options.add_argument('--disable-gpu')
                driver_options.add_argument('--no-sandbox')
                driver_options.add_argument('--disable-dev-shm-usage')
                driver_options.add_argument('--window-size=1400,1400')
            end
        end
    end
    ```

    In the `spec/support/factory_bot.rb` file add this code:
    ```ruby
    # spec/support/factory_bot.rb
    RSpec.configure do |config|
        config.include FactoryBot::Syntax::Methods
    end
    ```

    Finally update with the following code in the file `spec/rails_helper.rb` to load the support files:
    ```ruby
    # Add additional requires below this line. Rails is not loaded until this point!
    require_relative 'support/factory_bot'
    require_relative 'support/chrome'
    ```
3. Create a Factory with FactoryBot

    Create a new file under the `spec/` directory called `factory.rb`.

    Add this code to the new file to set up a factory for the student model in this example.
    ```ruby
    # spec/factories.rb
    FactoryBot.define do
        factory :student do
            first_name { Faker::Name.first_name }
            last_name { Faker::Name.last_name }
            major { Student::VALID_MAJORS.sample }
            expected_graduation_date { Faker::Date.between(from: 2.years.ago, to: 2.years.from_now) }
            email { "student1@msudenver.edu" }
            password { "password" }
            password_confirmation { "password" }
        end
    end
    ```

    Now all support files are configured.

## Test Functionality

Create a simple test and run it to test if the System is functioning correctly.

Here is an example test in the newly created file `spec/system/students_spec.rb` that was made after the `rails generate rspec:system student` command was run:
```ruby
# spec/system/students_spec.rb
require 'rails_helper'

RSpec.describe "Students", type: :system do
  before do
    driven_by(:selenium, using: :headless_chrome) # Use Capybara with headless Chrome
  end

  it "displays a student's name on the index page" do
    # Use FactoryBot to create a student
    student = create(:student, first_name: "Aaron", last_name: "Gordon")

    # Visit the students index page
    visit students_path

    # Check if the page displays the student's name
    expect(page).to have_content("Aaron")
    expect(page).to have_content("Gordon")
  end
end
```

To run the test there are a few options:
`rspec spec/system/students_spec.rb --format documentation`

If that command gives you issues try `bundle exec spec/system/students_spec.rb --format documentation`

If you are like me and were not able to get it to work at all. I was not able to find a solution in time unfortunately.