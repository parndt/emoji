# Emoji

A Ruby gem. For emoji. For everyone. :heart:

This gem includes an image library of open-source emoji and APIs for working with them.

## Installation

Add this line to your application's Gemfile:

    gem 'emoji'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install emoji
    
Install emoji image library assets:

    $ rake emoji:install_assets
    ====================================================================
    = emoji image assets install
    = Target: /Users/user/src/rails-app/app/assets/images/emoji
    = Source: /Users/user/src/emoji/assets/images
    ====================================================================
    - Creating /Users/user/src/rails-app/app/assets/images/emoji...
    - Installing assets...

## Usage

You can use this gem to replace unicode emoji characters with img tags linking to the appropriate emoji image.

Image Replacement APIs:

    > Emoji.replace_unicode_moji_with_images('I ❤ Emoji')
    => "I <img class=\"emoji\" src=\"http://localhost:3000/assets/emoji/heart.png\"> Emoji"

    > Emoji.image_url_for_unicode_moji('❤')
    => "http://localhost:3000/assets/emoji/heart.png"
    
    > Emoji.image_url_for_name('heart')
    => "http://localhost:3000/assets/emoji/heart.png"

Emoji Library Index APIs:

    > index = Emoji::Index.new
    
    > index.find_by_name('heart')
    => {"moji"=>"❤", "name"=>"heart", "name-ja"=>"ハート", "category"=>"abstract", "unicode"=>"2764"}
    
    > index.find_by_moji('❤')
    => {"moji"=>"❤", "name"=>"heart", "name-ja"=>"ハート", "category"=>"abstract", "unicode"=>"2764"}

Default configuration integrates with Rails, but you can change it with an initializer:
    
    # config/initializers/emoji.rb
    Emoji.asset_host = "emoji.cdn.com"
    Emoji.asset_path = '/assets/emoji'

String Helper Methods:

You can also 

    include 'emoji/string_ext'

and call methods directly on your string to return the same results:

    > 'I ❤ Emoji'.with_emoji_images
    => "I <img class=\"emoji\" src=\"http://localhost:3000/assets/emoji/heart.png\"> Emoji"

    > 'heart'.image_url
    > '❤'.image_url
    => "http://localhost:3000/assets/emoji.heart.png"
    
    > 'heart'.emoji_data
    > '❤'.emoji_data
    => {"moji"=>"❤", "name"=>"heart", "name-ja"=>"ハート", "category"=>"abstract", "unicode"=>"2764"}


## HTML Safety and Performance

This gem uses pure ruby code for compatibility with different Ruby virtual machines.  However, there can be significant performance gains to escaping incoming HTML strings using optimized, native code in the `escape_utils` gem.

The emoji gem will try to use `escape_utils` if it's available, but does not require it.  [Benchmarks show a 10x-100x improvement](https://gist.github.com/wpeterson/c851be471bd91868716c) in HTML escaping performance, based on the size of the string being processed.

To enable native HTML escaping, add this line to your application's Gemfile:

    gem 'escape_utils'


## Contributing

1. Fork it
2. Bundle Install (`bundle install`)
3. Run the Tests (`rake test`)
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
