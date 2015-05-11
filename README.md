icebergist.com
==============

# Local setup

1. Clone this repository.
1. Run `bundle install`
1. Install MacDown markdown editor - `brew cask install macdown`
1. Start local server `bundle exec jekyll --server --auto`


## Writing a new post

1. Create a new git branch `post/title_of_my_new_post'
1. Run `bundle exec rake new_post\['title_of_my_new_post'\]`
1. Open newly generated file in MacDown.
1. Edit meta information in header.
1. Write the post.
1. Take a look at it on your local server (you'll have to switch published flag to true).
1. Once you are satisfied with the post push your changes to Github and make a pull request.


# Deploying changes

To deploy latest changes run:

```
bundle exec rake generate
bundle exec rake deploy
```
 
-------------


# Icebergist - ist suffix

The suffix -ist is used to denote a person who either practices something or a person who is “user of/expert in” something or a person who holds certain principles, doctrines, etc.

http://en.wiktionary.org/wiki/-ist