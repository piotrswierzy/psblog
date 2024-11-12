# First Post

Hello World! As a developer, that’s pretty much the only right way to kick off a blog. Though to be fair, my current code output might not totally reflect the title.

But hey, as people say, you can’t judge a book by its cover—or a developer by the lines of code they write. And anyway, I'm writing this post using [mdBook](https://rust-lang.github.io/mdBook/), which is pretty cool, right? Initially, I wanted to use Payload for the blog, but it’s tough to host on Vercel. Plus, $35 a month for hosting on Payload directly was a bit much, and all the customization options got overwhelming. So, for now, I’m sticking with mdBook; we’ll see how it goes.

If you want to start your own blog with this setup, you can follow this [guide](https://rust-lang.github.io/mdBook/guide/creating.html). Then, just set up your GitHub workflow to deploy to GitHub Pages like this:

```yaml
name: Deploy
on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: write  # To push a branch 
      pull-requests: write  # To create a PR from that branch
    steps:
    - uses: actions/checkout@v4
      with:
        fetch-depth: 0
    - name: Install latest mdbook
      run: |
        tag=$(curl 'https://api.github.com/repos/rust-lang/mdbook/releases/latest' | jq -r '.tag_name')
        url="https://github.com/rust-lang/mdbook/releases/download/${tag}/mdbook-${tag}-x86_64-unknown-linux-gnu.tar.gz"
        mkdir mdbook
        curl -sSL $url | tar -xz --directory=./mdbook
        echo `pwd`/mdbook >> $GITHUB_PATH
    - name: Deploy GitHub Pages
      run: |
        # This assumes your book is in the root of your repository.
        # Just add a `cd` here if you need to change to another directory.
        mdbook build
        git worktree add gh-pages
        git config user.name "Deploy from CI"
        git config user.email ""
        cd gh-pages
        # Delete the ref to avoid keeping history.
        git update-ref -d refs/heads/gh-pages
        rm -rf *
        mv ../book/* .
        git add .
        git commit -m "Deploy $GITHUB_SHA to gh-pages"
        git push --force --set-upstream origin gh-pages

```
And there you go! You’ve got your very own amazing blog, just like this one.

## What’s This Blog Going to Be About?
Good question. Honestly, I’m not sure yet! I’ll write about whatever I want, and hopefully, it’ll be interesting.

## So, Who Am I?
I still consider myself a developer—but not a coder. I write pretty slowly :p Sadly my output got a hit two years ago when I co-founded [BlockyDevs](https://www.blockydevs.com/), a team of engineers who love solving problems and helping our partners succeed.

I’ve also started studying again and I am currently working on my Master’s in Economics. Which, by the way, is why I’m starting this blog today: I have an Advanced Microeconomics exam next week, and what better way to procrastinate than writing a blog?

## What’s This Blog Going to Be About? Second try.
I think I’ll use this blog as a place to share my thoughts, ideas, and opinions—maybe about economics, tech, blockchain, my company, life, or whatever else I find interesting. Maybe some of you will find it interesting too, and if not, I’ll still have a nice journal of my thoughts… if I keep this up for more than one post.

## Why Am I Doing This?
Firstly, I want to improve my English, and writing will help with that. I also plan to use AI to help with my grammar, so we’ll see how that affects my learning.

Plus, who knows? Maybe this will help me connect with others. Or maybe it’ll just be a fun way to put my thoughts out there. Either way, I’m curious to see where this goes.

## Used Tools
To write and set up this blog, I used Cursor AI. For editing and polishing my writing, I used ChatGPT with this prompt:
```
"I'm writing my first blog post. Help me correct my English and make it sound more natural, but keep a light tone."
```
