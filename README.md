Social Shares
=============

[![Gem Version](https://badge.fury.io/rb/social_shares.svg)](http://badge.fury.io/rb/social_shares)

Social shares is intended to easily check social sharings of an url.

Supported networks
------
International:
* [facebook](http://www.facebook.com/)
* [google plus](https://plus.google.com)
* [twitter](https://twitter.com/)
* [reddit](http://www.reddit.com/)
* [linkedin](https://www.linkedin.com/)
* [pinterest](http://www.pinterest.com/)
* [stumbleupon](http://www.stumbleupon.com/)
* [buffer](https://bufferapp.com/)

Russian:
* [vkontakte](http://vkontakte.ru/)
* [mail.ru](http://mail.ru/)
* [odnoklassniki](http://www.odnoklassniki.ru/)

Chinese:
* [weibo](http://www.weibo.com)

Basic usage
-----
```ruby
:000 > require 'social_shares'
 => true

:000 > url = 'http://www.apple.com/'
 => "http://www.apple.com/"

:000 > SocialShares.facebook url
 => 394927

:000 > SocialShares.google url
 => 28289

:000 > SocialShares.twitter url
 => 1164675
```
In case of exception:
```ruby
:000 > SocialShares.twitter url
 => nil

:000 > SocialShares.twitter! url
  => RestClient::RequestTimeout: Request Timeout
```

Advanced usage
-----
List of all supported networks:
```ruby
:000 > SocialShares.supported_networks
 => [:vkontakte, :facebook, :google, :twitter, :mail_ru, :odnoklassniki, :reddit, :linkedin, :pinterest, :stumbleupon, :buffer]
```

Fetch all shares by one method (#all, #all!):
```ruby
# in case of exception it will return nil
:000 > SocialShares.all url
 => {:vkontakte=>nil, :facebook=>399027, :google=>28346, :twitter=>1836, :mail_ru=>37, :odnoklassniki=>1, :reddit=>2361, :linkedin=>33, :pinterest=>21011, :stumbleupon=>43035, :weibo=>12760, :buffer=>1662}

# and this will raise it
:000 > SocialShares.all! url
 => RestClient::RequestTimeout: Request Timeout
```

Fetch shares of selected networks(#selected, #selected!):
```ruby
:000 > SocialShares.selected url, %w(facebook google linkedin)
 => {:facebook=>394927, :google=>28289, :linkedin=>nil}

# same here
:000 > SocialShares.selected! url, %w(facebook google linkedin)
 => RestClient::RequestTimeout: Request Timeout
```
Total sum of sharings in selected networks:
```ruby
:000 > SocialShares.total url, %w(facebook google)
 => 423216

# Second arg is optional, by default it takes all networks
:000 > SocialShares.total url
 => 1631102
```
Does any network have at least one link?
```ruby
:000 > SocialShares.has_any? url, %w(facebook google)
 => true
# Second arg is optional, by default it takes all networks
:000 > SocialShares.has_any? url
 => true
```
Note that #has_any? is faster than (#total > 0), because it stops on first network that has at least 1 sharing

Instalation
-----
Include the gem in your Gemfile:
```
gem 'social_shares'
```

Contributing
-----
* Create provider class in lib/social_shares/foo.rb with method #shares!, that return Integer value. #checked_url is accessed attribute.
```ruby
module SocialShares
  class Foo < Base
    def shares!
      response = RestClient.get(url)
      JSON.parse(response)["shares"] || 0
    end

  private

    def url
      "http://foo.com/?url=#{checked_url}"
    end
  end
end
```
* Add it to lib/social_shares.rb
```
require 'social_shares/foo'
SUPPORTED_NETWORKS = [:foo, :vkontakte, :facebook]
```
* Update README: add link to list, possible answer in #all method, etc.

Authors
----
* [Timur Kozmenko](https://twitter.com/Timrael)
