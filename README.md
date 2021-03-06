# emoji_data.rb

[![Gem Version](http://img.shields.io/gem/v/emoji_data.svg?style=flat)](https://rubygems.org/gems/emoji_data)
[![Build Status](http://img.shields.io/travis/mroth/emoji_data.rb.svg?style=flat)](https://travis-ci.org/mroth/emoji_data.rb)
[![Dependency Status](http://img.shields.io/gemnasium/mroth/emoji_data.rb.svg?style=flat)](https://gemnasium.com/mroth/emoji_data.rb)
[![CodeClimate Status](http://img.shields.io/codeclimate/github/mroth/emoji_data.rb.svg?style=flat)](https://codeclimate.com/github/mroth/emoji_data.rb)
[![Coverage Status](http://img.shields.io/coveralls/mroth/emoji_data.rb.svg?style=flat)](https://coveralls.io/r/mroth/emoji_data.rb)


Provides classes and helpers for dealing with emoji character data as unicode.  Wraps a library of all known emoji characters and provides convenience methods.

Note, this is mostly useful for low-level operations.  If you can avoid having to deal with unicode character data extensively and just want to encode/decode stuff, [rumoji](https://github.com/mwunsch/rumoji) might be a better bet for you.  If however, you are doing anything complicated involving emoji encoding/decoding, or you are just obsessed with understanding the details, this library is your new best friend.

This library currently uses `iamcal/emoji-data` as it's dataset, and thus considers it to be the "source of truth" regarding certain things, such as how to represent doublebyte unified codepoint IDs as strings (seperated by a dash).

This is basically a helper library for my [emojitrack](https://github.com/mroth/emojitrack) and [emojistatic](https://github.com/mroth/emojistatic) projects, but may be useful for other people.

## Installation

Add this line to your application's Gemfile:

    gem 'emoji_data'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install emoji_data

Currently requires `RUBY_VERSION >= 1.9.2`.

## Library Usage

Pretty straightforward, read the source.  But here are some things you might care about:

### EmojiData

  The `EmojiData` module provides some convenience methods for dealing with the library of known emoji characters.  Check out the source to see what's up.

Some notable methods to call out:

 - `EmojiData.find_by_unified(id)` gives you a quick way to grab a specific EmojiChar.

		>> EmojiData.find_by_unified('1f680')
	 	=> #<EmojiData::EmojiChar:0x007fd455ab2ff8 @name="ROCKET", @unified="1F680", @docomo="", @au="E5C8", @softbank="E10D", @google="FE7ED", @image="1f680.png", @sheet_x=21, @sheet_y=28, @short_name="rocket", @short_names=["rocket"], @text=nil>

 - `EmojiData.find_by_name(name)` and `.find_by_short_name(name)` do pretty much what you'd expect:

		>> EmojiData.find_by_name('thumb')
		=> [#<EmojiData::EmojiChar:0x007f9db214a558 @name="THUMBS UP SIGN", @unified="1F44D", @docomo="E727", @au="E4F9", @softbank="E00E", @google="FEB97", @image="1f44d.png", @sheet_x=10, @sheet_y=17, @short_name="+1", @short_names=["+1", "thumbsup"], @text=nil>, #<EmojiData::EmojiChar:0x007f9db2149720 @name="THUMBS DOWN SIGN", @unified="1F44E", @docomo="E700", @au="EAD5", @softbank="E421", @google="FEBA0", @image="1f44e.png", @sheet_x=10, @sheet_y=18, @short_name="-1", @short_names=["-1", "thumbsdown"], @text=nil>]

 - `EmojiData.char_to_unified(char)` takes a string containing a unified unicode representation of an emoji character and gives you the unicode ID.

		>> EmojiData.char_to_unified('🚀')
		=> "1F680"

 - `EmojiData.all` will return an array of all known EmojiChars, so you can map or do whatever funky Enumerable stuff you want to do across the entire character set.

 		#gimmie the shortname of all doublebyte chars
 		>> EmojiData.all.select(&:doublebyte?).map(&:short_name)
		=> ["hash", "zero", "one", "two", "three", "four", "five", "six", "seven", "eight", "nine", "cn", "de", "es", "fr", "gb", "it", "jp", "kr", "ru", "us"]


### EmojiData::EmojiChar

  `EmojiData::EmojiChar` is a class representing a single emoji character.  All the variables from the `iamcal/emoji-data` dataset have dynamically generated getter methods.

There are some additional convenience methods, such as `#doublebyte?` etc. Most important addition is the `#char` method which will output a properly unicode encoded string containing the character.
