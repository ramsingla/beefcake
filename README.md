# Beefcake (A sane Google Protocol Buffers library for Ruby)
## It's all about being Buf; ProtoBuf.

# Install

    $ gem install beefcake

# Example

    require 'beefcake'

    class Variety
      include Beefcake::Message

      # Required
      required :x, :int32, 1
      required :y, :int32, 2

      # Optional
      optional :tag, :string, 3

      # Repeated
      repeated :ary,  :fixed64, 4
      repeated :pary, :fixed64, 5, :packed => true

      # Enums - Simply use a Module (NOTE: defaults are optional)
      module Foonum
        A = 1
        B = 2
      end

      # As per the spec, defaults are only set at the end
      # of decoding a message, not on object creation.
      optional :foo, Foonum, 6, :default => Foonum::B
    end

    x = Variety.new :x => 1, :y => 2
    # or
    x = Variety.new
    x.x = 1
    x.y = 2

## Encoding

Any object responding to `<<` can accept encoding

    s = ""
    x.encode(s)
    p [:s, s]
    # or (because encode returns the string/stream)
    p [:s, x.encode]
    # or
    open("x.dat") do |f|
      x.encode(f)
    end

    # decode
    encoded = x.encode
    decoded = Variety.decode(encoded)
    p [:x, decoded]

    # decode merge
    Variety.decoded(more_data, decoded)

# Why?

  Ruby deserves and needs first-class ProtoBuf support.
  Other libs didn't feel very "Ruby" to me and were hard to parse.

# Caveats

  Currently Beefcake doesn't parse `.proto` files.  This is OK for now.
  In the simple case, you can create message types by hand; This is Ruby
  after all.

  Example (for the above):

    message Variety {
      required int32 x = 1
      required int32 y = 2
      optional string tag = 3

      ...
    }

  In the near future, a generator would be nice.  I welcome anyone willing
  to work on it to submit patches.  The other ruby libs lacked things I would
  like, such as an optional namespace param rather than installing on (main).

  This library was built with EventMachine in mind.  Not just blocking-IO.

# Dev

Source:

    $ git clone git://github.com/bmizerany/beefcake

## Testing:

    $ gem install turn
    $ cd /path/to/beefcake
    $ turn

## VMs:

Currently Beefcake is tested and working on:

* Ruby 1.8.7
* Ruby 1.9.2
* JRuby 1.5.6

There is a bug in Rubinius preventing proper ZigZag encoding.
The team is aware and I'm sure they're working on a fix.

## Future

Nice to have:

* `.proto` -> Ruby generator
* Groups (would be nice for accessing older protos)

# Further Reading

http://code.google.com/apis/protocolbuffers/docs/encoding.html

# Thank You

Aman Gupta (tmm1) for help with cross VM support and performance enhancements.
