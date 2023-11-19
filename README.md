# Install Ruby
https://rubyinstaller.org/downloads/
# Install Dev kit
https://rubyinstaller.org/add-ons/devkit.html

# Model Creating
1. Open a terminal/command prompt.
2. Navigate to the directory where you want to create your Rails application.
3. Run the following command to create a new Rails application:
  <code>rails new SoftDelete</code>
4. Change into the newly created application directory:
  <code>cd SoftDelete</code>
5. Generate a model named "Item" with the specified attributes:
   <code>rails generate model Item name:string deleted_at:datetime</code>

6. Run the database migration to create the "items" table in databse:
   <code>rails db:migrate</code>
   <code>RAILS_ENV=test rails db:migrate</code>
   
# Soft Delete Implementation
1. Open the generated "app/models/item.rb" file and replace the code

   ```
      class Item < ApplicationRecord
        validates :name, presence: true
      
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














