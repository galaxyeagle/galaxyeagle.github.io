# For self-reference

This is my personal Github pages blog. It was one of my first attempts at creating a website. Perhaps many beginners love Github pages. That's because it generates a static website from Markdown files. Github provides free hosting at a `https://username.github.io` URL. It is auto-built (with Jekyll) and auto-deployed whenever a commit is pushed to the master branch (Github Actions manages all CI/CD under the hood).

Further it is ideal for simple blogs. Unlike say a `Reactjs blog`, where you'd use `JSX` (tougher than Markdown) and install several dependencies in your `package.json` file & then `npm install` them, `Github pages` uses `Markdown` & just by including the `github-pages` gem in your `Gemfile` & running `bundle install`, all supporting gems (including the Jekyll gem itself) are automatically installed.

## Steps to create a Github pages blog

Refer to my article [here](https://galaxyeagle.github.io/pages/tech/webd/Creating-Static-Website.html) and [here](https://galaxyeagle.github.io/pages/tech/webd/Creating-Website-Advanced-1.html).

## When Migrating to a new PC

1. You can copy-paste your github-pages folder to your new machine. Alternatively, you can use `git clone` to clone the repository from Github to your new machine.

2. Install git, ruby, bundler and the jekyll gem (or simply the github-pages gem) in your new machine.

3. Delete the `Gemfile.lock` file and run `bundle install` again.

4. Run `bundle exec jekyll serve` to start the local server.
