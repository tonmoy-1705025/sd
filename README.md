# Install Ruby
https://rubyinstaller.org/downloads/
# Install Dev kit
https://rubyinstaller.org/add-ons/devkit.html

# Model Creating
1. Open a terminal/command prompt.
2. Navigate to the directory where you want to create your Rails application.
3. Run the following command to create a new Rails application:<br>
  <code>rails new SoftDelete</code>
4. Change into the newly created application directory:<br>
  <code>cd SoftDelete</code>
5. Generate a model named "Item" with the specified attributes:<br>
   <code>rails generate model Item name:string deleted_at:datetime</code>

6. Run the database migration to create the "items" table in the database:<br>
   a.<code>rails db:migrate</code><br>
   b.<code>RAILS_ENV=test rails db:migrate</code>
   
# Soft Delete Implementation
Open the generated "app/models/item.rb" file and replace the code: <br>

   ```
    class Item < ApplicationRecord
      validates: name, presence: true
    
      # Soft delete method
      def soft_delete
        update(deleted_at: Time.current)
      end
    
      # Restore method
      def restore
        update(deleted_at: nil)
      end
    
      # Default scope to exclude soft-deleted items
      default_scope -> { where(deleted_at: nil) }
    end
   ```


# Testing with RSpec 
1.Open your application's Gemfile and add the following lines to include the necessary testing gems:
  ```
  group: development, : test do
    gem 'rspec-rails'
  end
  ```
2.Install the gems:<br>
   <code>bundle install</code><br>
3.Generate RSpec configuration files:<br>
  <code>rails generate rspec:installcode>
4.Generate an RSpec file for the Item model:<br>
  <code>rails generate rspec:model Item</code>
# Implement RSpec tests:
  ```
  # spec/models/item_spec.rb
  require 'rails_helper'
  
  RSpec.describe Item, type: :model do
    it "soft deletes an item" do
      item = Item.create(name: "Test Item")
      item.soft_delete
  
      expect(item.deleted_at).to_not be_nil
    end
  
    it "restores a soft-deleted item" do
      item = Item.create(name: "Test Item")
      item.soft_delete
      item.restore
  
      expect(item.deleted_at).to be_nil
    end
  
    it "excludes soft-deleted items from normal queries" do
      item1 = Item.create(name: "Item 1")
      item2 = Item.create(name: "Item 2")
  
      item1.soft_delete
  
      expect(Item.all).to eq([item2])
    end
  end
  ```

# You are done! And run the test 
<code>rspec</code>









