---
layout: post
title: "Rails Callback Debugging"
date: 2020-02-17 00:00:00-0600
categories: jekyll update
---
Throughout my career as a Rails developer there have been a number of times that using Rails [callbacks](https://guides.rubyonRails.org/active_record_callbacks.html) for debugging purposes has come in handy and saved me a ton of time and frustration, so I thought I'd take some time to share this approach. To put it simply, if you're seeing any odd behavior relating to a Rails object being created, updated, or destroyed, this may be a way for you to easily get to the bottom of what is going on. Some might argue this approach is a bit hacky, but when it comes to debugging I think anything goes as long as it gives you the right answers.

## When To Use Callbacks for Debugging
 While this technique could be used for any of the Rails callbacks, I have mostly used it in the following contexts.

* A Rails object is unexpectedly being created (use `after_create` for debugging)
* A Rails object is unexpectedly being updated (use `after_update` for debugging)
* A Rails object is unexpectedly being destroyed (use `after_destroy` for debugging)

## How To Use Callbacks for Debugging
The implementation of this is relatively simple; if one the aforementioned unexpected behaviors is occurring, put the related callback in the correct model with a `binding.pry` (see [pry](https://github.com/pry/pry)) and use `caller` (see [caller](https://ruby-doc.org/core-2.3.1/Kernel.html#method-i-caller)) to help identify the source of the unexpected behavior. Let’s look at a simple example.

#### Models
```
class Team < ApplicationRecord
  has_many :players

  def create_player(name:)
    players.create(name: name)
  end
end
```

```
class Player < ApplicationRecord
  belongs_to :team
end
```
#### Factory
```
FactoryBot.define do
  factory :team do
    trait :bucks do
      name { 'Bucks' }

      after(:build) do |team|
        team.create_player(name: 'Giannis Antetokounmpo')
      end
    end
  end
end

```
#### Test
```
describe Team do
  let!(:bucks) { create(:team, :bucks) }

  describe '#create_player' do
    let(:team) { create(:team) }

    it 'creates a player' do
      team.create_player(name: 'John')

      expect(Player.count).to eq(1)
    end
  end
end
```
This test is going to fail because the player count will be 2 and not 1. The reason for this is that `let!(:team) { create(:team, :bucks) }` (specifically the `:bucks` attribute) causes the `Team` factory to create the `Giannis Antetokounmpo` player for that team, so when `team.create_player(name: 'John')` is called there is already an existing player. See [factory_bot](https://github.com/thoughtbot/factory_bot) for more info. While this is relatively easy to spot in this simple example, in a real world situation the `let!(:team) { create(:team, :bucks) }` could be hundreds of lines above the failing test, and getting to the bottom of why this test is failing could take a long time. This is where using callbacks comes into play, so let's add that now.

```
class Player < ApplicationRecord
  belongs_to :team

  after_create :debug_test_failure // Callback added for debugging

  def debug_test_failure
    binding.pry
  end
end
```

When I run the test again with this `debug_test_failure` callback I will hit the `binding.pry` every time a player is created. Once this pry is hit I can call `caller` to see the execution stack, which will point me directly to code that is creating the player. For this test my `binding.pry` will be hit twice (once for `let!(:team){ create(:team, :bucks) }` and once for `team.create_player(name: 'John’)`), and it’s on the first hit that I will be pointed to the line of code that is the culprit.  From there I can fix the test accordingly and then remove the debugging callback I added to the player model.

It is worth noting that the simple example above, while helpful for capturing the technique of using callbacks for debugging, does not fully illustrate the full potential of this technique.  It’s when you’re dealing with code bases that have evolved over years and have many dusty corners that this can really save you a ton of time and frustration. So, hopefully the next time you are scratching your head over the source of some unexpected Rails object behavior, you can apply this approach.
