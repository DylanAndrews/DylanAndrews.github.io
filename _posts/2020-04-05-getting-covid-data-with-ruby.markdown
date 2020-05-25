---
layout: post
title: "Getting COVID-19 Data With Ruby"
date: 2020-04-05 00:00:00-0600
categories: post ruby methods
---
I’m going to take a slightly different approach with this post and talk about a very cool and timely ruby [gem](https://github.com/siaw23/kovid) called kovid that I recently discovered and contributed to this weekend.


### The Gem
Ever since the pandemic began I've been wanting an easy, centralized way to get COVID-19 data, and this gem is exactly that. It's a CLI tool that you can use to get worldwide data, to compare data from various countries, and even compare data at the state level in the United States. See the [docs](https://github.com/siaw23/kovid) for more info.

### My Contribution

This week I got into a routine of running `kovid check USA` and `kovid state Tennessee` (see output below) every morning before work just to get a sense of how things are going, and after a couple of days I realized it would be great to have the ability to see a table of all the US states for comparison. The docs listed a feature that allows you to do `kovid states Florida Tennessee`, for example, to compare multiple states, but it wasn't working and there wasn’t an easy way to get data on all of the states at once without typing them all out. So, I figured I’d take some time to fork the repo this weekend and take a stab at adding this country wide data feature and also look into fixing the issue with state comparison.

![usa](/assets/kovid-post/usa.png)


![state](/assets/kovid-post/state.png)

After having a bit of struggle getting things set up for local development, I was good to go. I got the country wide feature added, and I fixed the bug with state comparison (see my pull-request [here](https://github.com/siaw23/kovid/pull/95) if interested). If you now run `kovid aus` you will see all US states listed in order of COVID-19 cases descending.

![state](/assets/kovid-post/all_states.png)

### Conclusion
This was my first time contributing to an open source project in my free time, and I must say I found it deeply satisfying. I got to work within a design pattern that is new to me, I got to collaborate with other rubyists across the world, I got the feature I wanted and can now use, and I made the gem a little bit better. I hope to contribute more features in the future, and I hope others find the gem useful during these tough times.
