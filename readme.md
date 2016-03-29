# Angular On Rails

## Learning objectives
- Asset pipeline
- Lib vs vendor vs app vs public
- Respond to different formats
- When and why Angular on Rails?

Solution code:

https://github.com/ga-wdi-exercises/inventory_tracker/tree/rails-solution

### Rails new

`rails new . -d postgresql`

### Put Inventory Tracker files in appropriate Rails locations

```
app.js        => app/assets/javascripts/products.js
index.html    => app/views/products/index.html.erb
data.js       => db/products_data.json
angular.js    => vendor/assets/javascripts/angular.js
bootstrap.css => vendor/assets/stylesheets/bootstrap.css
```

`lib` vs `vendor` vs `app`: http://guides.rubyonrails.org/asset_pipeline.html#asset-organization

### Define an index route for products

> Why aren't assets working?

### Configure assets to be included in Products#index only

Don't want the Inventory Tracker Angular stuff to be loaded on every page -- just the Products#index page.

Asset pipeline overview

`config/initializers/assets.rb`

stylesheets need property attribute to validate

Need to restart server

### Add model, migrations, and seeds

Only need to change 11 character to make `products_data.json` into a valid JSON file.

File.read
JSON.parse

### Make the Products#index route render either JSON or HTML

`respond_to` and `render json`

### Make Angular load the products from the database

> Need ngResource. In which `assets` folder should it go?

> What needs to change in `products.js`?

### Make the `product.cost` show up

Received from Rails as a string.

### Add destroy method

> What needs to change in application controller?

### Add create method

Can still use strong params

### Add update method

Angular doesn't "know" *when* we want to save our changes to the database, and it doesn't know *how*. Can tell it with `data-ng-change` and `vm.update`

### Deploy

Need 12Factor

```bash
$ heroku create
$ git push heroku master
$ heroku rake db:migrate
$ heroku rake db:seed
$ rake secret
# Copy the output
$ heroku config:set SECRET_KEY_BASE=whatyoucopied
$ heroku open
```
