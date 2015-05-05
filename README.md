What this buildpack does
========================

 * Run `npm install`
 * Install latest version of `gulp` if a gulpfile is present and was not already installed by previous pass
 * Install latest version of `bower` if a bowerfile is present and was not already installed by previous pass
 * Install latest version of `grunt` if a gruntfile is present and was not already installed by previous pass
 * Install `bundle`
 * Run `bower install`
 * Run `bundle install`
 * Run `bundle exec compass compile`
 * Run `gulp heroku:$NODE_ENV` (by default, it expands to `heroku:production`)
 * Run `grunt heroku:$NODE_ENV`


How to access private repos
---------------------------
Currently we support private repos for bower, for GitHub, by leveraging the "shorthand syntax". Follow this guide:

 * In your bower.json, install private repos using the syntax "organization/repo".
 * Create or modify your `.bowerrc` file, adding this line: `"shorthand-resolver": "git@github.com:{{owner}}/{{package}}.git"`. This tells bower to normally access GitHub using SSH, so that you have access to your private repos.
 * Create a personal access token from your [GitHub account security page](https://github.com/settings/applications). Unfortunately, GitHub doesn't allow to limit the token to access to a specific repo (nor a specific organization). If you feel uncomfortable with this, you need to create a new dummy GitHub user, grant it access to the repo(s) that you need to access from Heroku, and then create a personal access token for this user.
 * Save the token in the Heroku enviornment as `GITHUB_AUTH_TOKEN`.

The buildpack will then automatically instruct bower to access your repos through the token instead of using SSH (for which it wouldn't have access).

Notes
-----
 * Everything is cached after first download: npm, bower, gems
 * If you're upgrading from another grunt/gulp/bower/nodejs buildpack/fork, the process might fail because of a different use of the build cache. You're advised to purge the build cache of your project, by running: `heroku plugins:install https://github.com/heroku/heroku-repo.git; heroku repo:purge_cache -a appname`

