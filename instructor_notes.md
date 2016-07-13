# Instructor Notes

## Walkthrough

### Setup

```bash
$ git clone git@github.com:ga-wdi-exercises/inventory_tracker.git
$ cd inventory_tracker
$ git checkout rails-starter
```

### [Rails New](https://github.com/ga-wdi-exercises/inventory_tracker/commit/8bf95e9a46f267db0bb1203587d75c7f3576f2b7)

```bash
$ rails _4.2.6_ new . -d postgresql
```

### [Move Angular Files](https://github.com/ga-wdi-exercises/inventory_tracker/commit/82d1d134059d62c81c7a1ae9988c9debd58715ce)

```bash
$ mkdir app/views/products
$ mv index.html app/views/products/index.html.erb
$ mv angular.js vendor/assets/javascripts
$ mv angular-resource.js vendor/assets/javascripts
$ mv bootstrap.css vendor/assets/stylesheets
$ mv app.js app/assets/javascripts/products.js
$ mv data.js db/products_data.json
```

### [Render Product#Index page](https://github.com/ga-wdi-exercises/inventory_tracker/commit/afe2b529dd3e3e336fa8031a281af049cdd5428b)

```bash
$ touch app/controllers/products_controller.rb
```

in `app/controllers/products_controller.rb` add:

```ruby
class ProductsController < ApplicationController
  def index
  end
end
```

in `config/routes.rb` add:

```ruby
resources :products
```

then start the server and view page:

```bash
$ rake db:create
$ rails s
```

### [Make Angular Assets load on Products#index page](https://github.com/ga-wdi-exercises/inventory_tracker/commit/60c34f80b932ceda3320975a73ebb0ce48d8d73b)

Remove `require_tree` from `app/assets/stylesheets/application.css` and `app/assets/javascripts/application.js`

Require `angular` and `angular-resource` to `app/assets/javascripts/products.js`

```js
//= require angular
//= require angular-resource
```

Require `bootstrap` in `app/assets/stylesheets/product.css`

```css
/*
*= require bootstrap
*/
```

Configure Rails to serve our custom static assets

in `config/initializers/assets.rb`

```ruby
Rails.application.config.assets.precompile += %w( products.js products.css )
```

Fix html in `app/views/products/index.html.erb` and load in custom assets

```html
<%= stylesheet_link_tag "products", property: "stylesheet" %>
<%= javascript_include_tag "products" %>
```

Make sure to add `ng-app="inventory"` to container `div`

Restart Server and should see working Angular app

### Add Models, Migrations, and Seeds

create a new model for products: `$ touch app/models/product.rb`

In `app/models/product.rb`:

```ruby
class Product < ActiveRecord::Base
end
```

Create a new migration with the appropriate fields:

```bash
$ rails g migration create_products name image_url cost:decimal url quantity:integer notes:text country
```

That should create a migration file that looks like:

```ruby
class CreateProducts < ActiveRecord::Migration
  def change
    create_table :products do |t|
      t.string  :name
      t.string  :image_url
      t.decimal :cost
      t.string  :url
      t.integer :quantity
      t.string  :country
      t.text    :notes
    end
  end
end
```

Fix `db/products_data.json` to be valid JSON by removing `var data = `

Then, load the json file and seed the database with it

In `db/seeds.rb`:

```ruby
data = JSON.parse(File.read("db/products_data.json"))
Product.destroy_all
Product.create!(data)
```

### [Make Products#index render HTML or JSON](https://github.com/ga-wdi-exercises/inventory_tracker/commit/f3f49067aabebbc334ac83d7ed93f28e217d8286)

In `app/controllers/products_controller.rb`, use `respond_to` in the `index` action:

```ruby
def index
  respond_to do |format|
    format.html
    format.json{ render json: Product.all }
  end
end
```

Restart your server and visit `http://localhost:3000/products.json` and you should see `json`
