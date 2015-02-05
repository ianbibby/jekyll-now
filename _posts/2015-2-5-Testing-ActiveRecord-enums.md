---
layout: post
title: Testing ActiveRecord enums
---

ActiveRecord enums have been available since Rails 4.1 and help bring sanity to those model attributes that map a *magic number* to some human-friendly string.

You can read the [documentation](http://api.rubyonrails.org/v4.2.0/classes/ActiveRecord/Enum.html) for all the details.

Previously, and without any customized solutions, you'd headdesk yourself immediately after realizing you threw in a number without any clear meaning then quickly reach for the [Replace Magic Number with Symbolic Constant](http://www.refactoring.com/catalog/replaceMagicNumberWithSymbolicConstant.html) refactoring.

Never again!  Using the example from the docs, here is the new alternative with enums:

```ruby
class Conversation < ActiveRecord::Base
  enum status: [ :active, :archived ]
end

# conversation.update! status: 0
conversation.active!
conversation.active? # => true
conversation.status  # => "active"
```

Ok cool.  But wait, this is still kinda sucky and there's an even better way.

If you can't already tell, the status attribute value points to a particular position in the array.  Change the order of the array, and you'll be in trouble.

A better way is to use a Hash:

```ruby
class Conversation < ActiveRecord::Base
  enum status: { active: 0, archived: 1 }
end
```

The numbers here are ok - but if you were dealing with legacy stuff where the numbering isn't sequential it'd be easy to do something like `{ active: STATUS_ACTIVE, ... }`.

Back to the point of this post though, how do you go about creating these enums from a TDD approach?  I've been using minitest a lot lately, not the spec-style minitest but plain ole ActiveSupport::TestCase minitest and it's **awesome**!

Solution \#1:

```ruby
class FooTest < ActiveSupport::TestCase

  ...
  
  [ :active, :archived ].each do |status|
    define_method("test_valid_status_#{status.to_s}") do
      subject.status = Foo.statuses[status]
      assert subject.send "#{status.to_s}?".to_sym
    end
  end
  
  ...
```

Solution \#2, which takes advantage of the set operations on `Array`:

```ruby
  STATUS_VALUES = [ :active, :archived ]

  def test_missing_foo_statuses
    assert_empty STATUS_VALUES - Foo.statuses.keys.map(&:to_sym), "Foo is missing statuses"
  end

  def test_extraneous_foo_statuses
    assert_empty Foo.statuses.keys.map(&:to_sym) - STATUS_VALUES, "Foo has extraneous formats"
  end
```
I prefer solution 2 because of the added benefit of catching left over crud from any experimenting you may have done.
