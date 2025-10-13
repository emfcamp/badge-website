## Rules for apps in the official badge store

- App has to be "Rated G: General Audiences – all ages admitted"
- App should not be of malicious nature
- App complies with EMF's [Code of
  Conduct](https://www.emfcamp.org/code-of-conduct)
- No code/image hot-loading without good reasons (since it might be
  change after the review process)
- It's fine for people to improve on Apps other have written. Likewise,
  if you submit an App, be aware that others can and will make changes
  to it.

## Packaging up your badge app for submission to the store

Every app becomes a new folder in the root of the Mk4-App GitHub
repository. So if you create a "snake" app, you need to put your program
files in a folder called "snake" with at least a "main.py" in it.

App names need to be unique (the folder structure enforces that). App
folders can contain multiple files (python and non python), but at a
minimum "main.py" with the correct headers is required.

Plesae verify that your apps folder structure is correct by running
"./tilda_tools validate" (this works without a badge) before submitting
it to the badge app store as the validations is also run by Travis to
check pull requests.

### Headers

main.py headers:

    ___title___         = "<your_app_name>"
    ___license___      = "MIT"
    ___dependencies___ = ["wifi", "http", "ugfx_helper", "sleep"]
    ___categories___   = ["<see below>"]
    ___bootstrapped___ = True # Whether or not apps get downloaded on first install. Defaults to "False", mostly likely you won't have to use this at all.

Please use one of these categories:

- System
- Homescreens
- Games
- Sound
- EMF
- Villages
- Phone
- LEDs
- Sensors
- Demo
- Other

## Submitting your badge app to the official badge store

- Create a GitHub account [1](https://github.com/join)
- Download and install git on your local computer
  [2](https://git-scm.com/downloads)
- Set up git on your local computer with your username and email address
  [3](https://github.com/emfcamp/Mk4-Apps/blob/master/CONTRIBUTING.md#using-git-and-github-to-submit-your-badge-app)
- Fork the emfcamp/Mk4-Apps repo
  [4](https://github.com/emfcamp/Mk4-Apps/blob/master/CONTRIBUTING.md#using-git-and-github-to-submit-your-badge-app)
- Clone **your** GitHub repo to your computer
  [5](https://github.com/emfcamp/Mk4-Apps/blob/master/CONTRIBUTING.md#using-git-and-github-to-submit-your-badge-app)
- Add your app to your local repo
  [6](https://github.com/emfcamp/Mk4-Apps/blob/master/CONTRIBUTING.md#using-git-and-github-to-submit-your-badge-app)
- Create a Pull Request
  [7](https://github.com/emfcamp/Mk4-Apps/blob/master/CONTRIBUTING.md#using-git-and-github-to-submit-your-badge-app)

<!-- -->

- Over time, your GitHub repo will get out of sync with the changes that
  have been made to the emfcamp/Mk4-Apps repo so you will need to update
  your GitHub repo
  [8](https://github.com/emfcamp/Mk4-Apps/blob/master/CONTRIBUTING.md#updating-your-github-and-local-git-repo)

If you prefer a GUI interface, you could consider
<https://desktop.github.com/>