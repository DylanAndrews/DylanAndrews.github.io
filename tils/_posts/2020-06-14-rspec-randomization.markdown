---
layout: post
title: "Rspec Randomization"
date: 2020-06-14 00:00:00-0600
categories: ruby til
---
Setting `Kernel.srand config.seed` in your Rspec config will cause calls to ruby methods such as `rand`, `shuffle`, and `sample` to return the same element every time for a given seed. More [here](https://relishapp.com/rspec/rspec-core/v/3-3/docs/command-line/randomization-can-be-reproduced-across-test-runs#specifying-a-seed-using-%60srand%60-provides-predictable-randomization).
