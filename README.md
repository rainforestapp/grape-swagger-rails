# GrapeSwaggerRails

[![Build Status](https://travis-ci.org/TinkerDev/grape-swagger-rails.svg)](https://travis-ci.org/TinkerDev/grape-swagger-rails)

Swagger UI as Rails Engine for grape-swagger gem.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'grape-swagger-rails'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install grape-swagger-rails

## Compatibility

GrapeSwaggerRails is compatible with the following versions of grape and grape-swagger.

grape  | grape-swagger
-------|--------------
0.9.0  | 0.7.2
0.9.0  | 0.8.0
0.10.0 | 0.9.0

## Usage

Add this line to `./config/routes.rb`:

```ruby
mount GrapeSwaggerRails::Engine => '/swagger'
```

Create an initializer (e.g. `./config/initializers/swagger.rb`) and specify the URL to your Swagger API schema and app:

```ruby
GrapeSwaggerRails.options.url      = '/swagger_doc.json'
GrapeSwaggerRails.options.app_url  = 'http://swagger.wordnik.com'
```

You can set the app name, default is "Swagger".

``` ruby
GrapeSwaggerRails.options.app_name = 'Swagger'
```

You can specify additional headers to add to each request:

```ruby
GrapeSwaggerRails.options.headers['Special-Header'] = 'Some Secret Value'
```

Using the `headers` option above, you could hard-code Basic Authentication credentials.
Alternatively, you can configure Basic Authentication through the UI, as described below.

### Basic Authentication

If your application uses Basic Authentication, you can setup Swagger to send the username and password to the server with each request to your API:

```ruby
GrapeSwaggerRails.options.api_auth     = 'basic' # Or 'bearer' for OAuth
GrapeSwaggerRails.options.api_key_name = 'Authorization'
GrapeSwaggerRails.options.api_key_type = 'header'
```

Now you can specify the username and password to your API in the Swagger "API key" field by concatenating the values like this:

    username:password

The javascript that loads on the Swagger page automatically encodes the username and password and adds the authorization header to your API request.
See the official Swagger documentation about [Custom Header Parameters](https://github.com/wordnik/swagger-ui#custom-header-parameters---for-basic-auth-etc)

### API Token Authentication

If your application uses token authentication passed as a query param, you can setup Swagger to send the API token along with each request to your API:

```ruby
GrapeSwaggerRails.options.api_key_name = 'api_token'
GrapeSwaggerRails.options.api_key_type = 'query'
```

You can use the ```api_key``` input box to fill in your API token.
### Swagger UI Authorization

You may want to authenticate users before displaying the Swagger UI, particularly when the API is protected by Basic Authentication.
Use the `before` option to inspect the request before Swagger UI:

```ruby
GrapeSwaggerRails.options.before_filter do |request|
  # 1. Inspect the `request` or access the Swagger UI controller via `self`.
  # 2. Check `current_user` or `can? :access, :api`, etc.
  # 3. Redirect or error in case of failure.
end
```

### Updating Swagger UI from Dist

To update Swagger UI from its [distribution](https://github.com/wordnik/swagger-ui), run `bundle exec rake swagger_ui:dist:update`. Examine the changes carefully.

NOTE: This action should be run part of this gem (not your application). In case if you want to
make it up-to-date, clone the repo, run the rake task, examine the diff, fix any bugs, make sure
tests pass and then send PR here.

### Enabling in a Rails-API Project

The grape-swagger-rails gem uses the Rails asset pipeline for its Javascript and CSS. Enable the asset pipeline with [rails-api](https://github.com/rails-api/rails-api).

Add sprockets to `config/application.rb`.

```ruby
require 'sprockets/railtie'
```

Include JavaScript in `app/assets/javascripts/application.js`.

```javascript
//
//= require_tree .
```

Include CSS stylesheets in `app/assets/stylesheets/application.css`.

```css
/*
*= require_tree .
*/
```

## Contributors

* [unloved](https://github.com/unloved)
* [dapi](https://github.com/dapi)
* [joelvh](https://github.com/joelvh)
* [dblock](https://github.com/dblock)
* ... and [more](https://github.com/TinkerDev/grape-swagger-rails/graphs/contributors) ...

## Contributing

See [CONTRIBUTING](CONTRIBUTING.md).

## License

MIT License, see [LICENSE](LICENSE.txt).
