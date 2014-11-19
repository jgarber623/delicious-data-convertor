# Delicious Data Convertor

A simple Ruby-based tool for converting an exported `delicious.html` file from [Delicious](https://delicious.com/) into JSON.


## Requirements

- No aversion to the command line
- Ruby (2.1.4)
- Bundler (1.7.4 or newer)
- A [Delicious](https://delicious.com/) account


## Installation

1. Install Ruby 2.1.4. If you're using Mac OS X, you can use tools like [chruby](https://github.com/postmodern/chruby), [rbenv](https://github.com/sstephenson/rbenv), or [RVM](https://rvm.io) to install multiple versions of Ruby, including the one required for this tool.
2. Install Bundler (`gem install bundler`).
3. Install gems (`bundle install`).


## Usage

1. Clone this repository and `cd` into the `delicious-data-convertor` directory.
2. Visit the ["Manage" section](https://delicious.com/settings/manage) of your Delicious account and choose "Export Bookmarks." Be sure to include tags and notes in the export (the default).
3. Save the resulting `delicious.html` file to this project's `data` directory.
4. Back over in Terminal, run `rake convert` from the root of the project.
5. Marvel at the resulting `bookmarks.json` file in the `data` folder!


## Caveats

I slapped this together quickly in an hour or so. It's really only tested against an export of [my Delicious bookmarks](https://delicious.com/jgarber). It may or may not work for you.

### HTML elements in titles and descriptions

I encountered an issue with bookmarks that had strings like `<article>`, `<url>`, etc. in titles and descriptions. Basically, instances where there were HTML elements (or strings that look like HTML elements) caused Nokogiri to go haywire. I couldn't find a bulletproof solution to this, so I manually changed `<` to `&lt;` and `>` to `&gt;` in titles and descriptions.

For example, this:

	A sample description referencing an <html> element.

became this:

	A sample description referencing an &lt;html&gt; element.

It's not perfect, but it's the best I could figure out in a timely fashion.


## License

Per [CC0](http://creativecommons.org/publicdomain/zero/1.0/), to the extent possible under law, I have waived all copyright and related or neighboring rights to this work. Go forth and make things.