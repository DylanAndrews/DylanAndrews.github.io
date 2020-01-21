---
layout: post
title: "Developer Necessities"
date: 2019-01-21  00:00:00-0600
categories: jekyll update
---
After giving it some thought, I’ve decided to write a post cataloging the various tools, tips, tricks, and commands that I could not live without. These have been gathered over the past three years, and I know that a list like this would have been invaluable to me when I was first getting started. Luckily I had a lot of great people around me to clue me in to this stuff, but for those who are not so lucky, I hope this post is helpful.

### Software
* [Better Snap Tool](https://itunes.apple.com/us/app/bettersnaptool/id417375580?mt=12) - The ultimate tool for organizing and managing windows on mac
* [OnePassword](https://1password.com/) - Best password manager I know of
* [Alfred](https://www.alfredapp.com/powerpack/) - Worth the price for the paste history alone, but also has some great customization features for becoming a mac power user.

### Editor Plugins
These plugins are specific to atom, my editor of choice, but if you don't use atom you should still be able to find something very similar.
* [vim-mode-plus](https://atom.io/packages/vim-mode-plus) - allows you to use vim commands in the editor
* [Bracket Colorizer](Bracket Colorizer) - makes it way easier to find where your brackets start and end

### Rails
1. ##### `_`
I can use `_` in the rails con set a variable to the return value of my last expression. For example, if I do `User.where(email: foo@bar.com)`, I can then do `user = _` and it will set `user` to the return value of `User.where(email: foo@bar.com)`.
2. ##### `reload!`
This will load the latest code into the rails console environment. This will prevent you from having to close and reopen your rails console every time you make a change to the code.

### Git
1. #### `git commit --amend --no-edit`
I have an alias for this and I use it more than I would like to admit. My typical use case is I make a commit and then realize I missed a piece of code that should have gone in with that commit. After realizing this I simply do `git add file-i-changed`, `git commit --amend --no-edit`, and then `git push --force` (be very careful with force push; I only do it on branches that I alone am working on).
2. #### `git log`
Allows you to see the commit history in your current branch; it comes in handy all the time.
3. #### `git cherry-pick`
I am constantly amazed by this command. It allows you to take a commit from one branch and put it in another. For example, if I am currently working on the `foo` branch and want to take a commit with a sha of `1234` from the `staging` branch. I would simply do `git cherry-pick 1234` and it will put that commit on top of the commits in my `foo` branch.

### Command Line
* [tmux](https://github.com/tmux/tmux) - Allows you to control multiple terminal environments in one screen. A must if you have a project that requires multiple builds to be running locally.
* [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh) - For managing your zsh config
