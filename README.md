## Guile.docset

This is a [Dash][1] docset for the Guile Scheme interpreter.

### Building

The documentation index generator is written in Ruby. I'd recommend
ruby 2.0 or greater

    $ gem install bundler
    $ bundle install
    $ rake generate

If you want to blow away the index and start over again:

    $ rake clean

[1]: http://kapeli.com/dash
