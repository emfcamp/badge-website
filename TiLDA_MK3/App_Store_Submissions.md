## How to submit

Zip your folder up, sign up to <http://api.badge.emfcamp.org/>, create a
new app and publish it

## Common Problems

Please check these things before publishing your app:

- Your app will end up in "app/username~appname", so please make sure
  all you paths are using that. Example:
  `open("app/myname~myapp/some.json")`
- Please make sure the meta data at the top of the file is ok
- Please make sure to use `ugfx.init()` and `buttons.init()`, it makes
  it easier for me to review

Besides those, there are currently problems with apps that have too many
files (lots of images for example) or use too much memory (big arrays).
If it's possible in any way, please try to keep both down. We're working
on reducing memory usage.