# ChankoAb

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'chanko_ab'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install chanko_ab

## Usage
```ruby
# Define logging code and identifier
# e.g.
ChankoAb.set_logging do |name|
  Rails.logger.debug(name)
end

ChankoAb.set_default_identifier do
  cookies[:identifier]
end
```

```ruby
# And then, prepare ab test chanko unit.
module MySplitTest
  include Chanko::Unit
  include ChankoAb

  split_test.add(:default, {})
  split_test.add(:pattern1, { partial: 'partial1'} )

  split_test.log_tenmplate('show' ,'my_split_test.[name]')

  split_test.define(:new_text, scope: :view) do
    ab.log('show')

    case ab.name
    when 'default'
      run_default
    else
      render ab.fetch(:partial)
    end
  end
end
```

### Switch using identifier
```ruby
module MySplitTest
  ...
  split_test.identifier { cookies[:unique_id] }
  ...
end
```

### Use HexIdentifier if identifier is composed by hex.
```ruby
module HexMySplitTest
  ...
  split_test.logic ChankoAb::Logic::HexIdentifier
  ...
end
```

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/eudoxa/chanko_ab.
