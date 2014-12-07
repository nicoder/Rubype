# Ruby with Type.

Matz has mentioned Ruby3.0 with static type at some confluences. But almost all rubyists(include me) are not sure how typed Ruby is.

But it's worth thinking more. This gem is kind of trial without so much side-effect.

```rb
require 'rubype'

# ex1
class MyClass
  def sum(x, y)
    x + y
  end
  typesig sum: [Numeric, Numeric => Numeric]

  def wrong_sum(x, y)
    'string'
  end
  typesig wrong_sum: [Numeric, Numeric => Numeric]
end

MyClass.new.sum(1, 2)
#=> 3

MyClass.new.sum(1, 'string')
#=> ArgumentError: Wrong type of argument, type of "str" should be Numeric

MyClass.new.wrong_sum(1, 2)
#=> TypeError: Expected wrong_sum to return Numeric but got "str" instead


# ex2
class People
  type People >= Any, def marry(people)
    # Your Ruby code as usual
  end
end
typesig marry: [People => Any]

People.new.marry(People.new)
#=> no error

People.new.marry('non people')
#=> ArgumentError: Wrong type of argument, type of "non people" should be People
```

## Feature
### Typed method can coexist with non-typed method

```ruby
# It's totally OK!!
class MyClass
  def sum(x, y)
    x + y
  end
  typesig sum: [Numeric, Numeric => Numeric]

  def sum_without_type(x, y)
    'string'
  end
end
```

### Duck typing

```ruby

class MyClass
  def foo(any_obj)
    1
  end
  typesig sum: [Any => Numeric]
end

# It's totally OK!!
MyClass.new.foo(1)
# It's totally OK!!
MyClass.new.foo('str')
```

## Installation

gem install rubype or add gem 'rubype' to your Gemfile.

This gem requires Ruby 2.0.0+.

### Contributing

Fork it ( https://github.com/[my-github-username]/rubype/fork )

Create your feature branch (`git checkout -b my-new-feature`)

    $ bundle install --path vendor/bundle

Commit your changes (`git commit -am 'Add some feature'`)

    $ bundle exec rake test

    > 5 runs, 39 assertions, 0 failures, 0 errors, 0 skips

Push to the branch (`git push origin my-new-feature`)

Create a new Pull Request

## Credits
[@chancancode](https://github.com/chancancode) first brought this to my attention. I've stolen some idea from him.
