# Create a new Backpack Theme

-----

This tutorial will create and package a Backpack theme, so that you can use it in multiple Laravel projects. And if you open-source it, others can do the same.


<a name="create-working-code"></a>
## Part A. Build the Theme in Your Project


<a name="create-the-package"></a>
## Part B. Create The Package


<a name="generate-package-folder"></a>
### Step 1. Generate the package folder

Let's install this excellent package that will make everything a lot faster:
```sh
composer require jeroen-g/laravel-packager --dev
```

Let's create our package. Instead of using their skeleton, we're going to use the Backpack [addon-skeleton](https://github.com/Laravel-Backpack/addon-skeleton):

```sh
php artisan packager:new --i --skeleton="https://github.com/Laravel-Backpack/addon-skeleton/archive/master.zip"
```

It will then ask you some basic information about the package. Keep in mind:
- the ```vendor-name``` should probably be your GitHub handle (or organisation), in kebab-case (ex: `company-name`); it will be used for folder names, but also for GitHub and Packagist links;
- the ```package-name``` should be in `kebab-case` too (ex: ```moderate-operation```);
- the `skeleton`, if you haven't copied the entire command above, should probably be the one we provide: `https://github.com/Laravel-Backpack/addon-skeleton/archive/master.zip`, which has everything you need to quickly create a Backpack add-on, including an innovative `AddonServiceProvider` that "_just works_";
- the ```website``` should be a valid URL, so include the protocol too: ```http://example.com```;
- the ```description``` should be pretty short; you can change it later in `composer.json`;
- the ```license``` is just the license name, if it's a common one (ex: ```MIT```, ```GPLv2```); our skeleton assumes you want `MIT` but you can easily change it;

OK great. The command has:
- created a ```/packages/vendor-name/package-name``` folder in your root directory;
- modified your project's ```composer.json``` file to load the files in this new folder;

This new folder should hold all your package files. You're off to a great start, because you're using our package skeleton (aka template), so it's already a _working package_. And it's already got a good file structure.

Let's take a look at the generated files inside ```/packages/vendor-name/package-name```:

![Blank Backpack addon as generated using the addon skeleton](https://user-images.githubusercontent.com/1032474/101022657-5992b080-357a-11eb-8f85-8e0718b66fb2.png)


You'll notice that it looks _exactly_ like a Laravel project, with a few exceptions:
- PHP classes live in `src` instead of `app`;
- inside that `src` folder you also have an `AddonServiceProvider`; so let's take a moment to explain why it's there, and what it does:
    - normally a package needs a ServiceProvider to tell Laravel "load the views from here", "load the migrations from here", "load configs from here", things like that; because a Composer package can also be a general PHP package (non-Laravel), normally you have to code a ServiceProvider for your package, that tells Laravel how to use your package - you have to write all that wiring logic;
    - but thanks to `AddonServiceProvicer`, you don't have to do any of that; it's all done _automatically_ if the files are in the right directories, just like Laravel does itself, in your project's folders;
    - the only thing you should worry about is placing your route files in `routes`, your migrations in `database/migrations` etc. and the `AddonServiceProvider` will understand and tell Laravel to load them; easy-peasy;

Excited by how easy it'll be to make it work? Excellent, let's do it.

> If you want to test that your package is being loaded, you can do a ```dd('got here')``` inside your package's ```AddonServiceProvider::boot``` method. If you refresh the page, you should see that ```dd()``` statement executed.

<a name="initialize-git-repo-and-make-first-commit"></a>
### Step 1. Initialize a git repo and make your first package commit

Let's save what we have so far - the generated files:
```bash
# go to the package folder (of course - use your names here)
cd packages/vendor-name/package-name

# create a new git repo
git init

# (optional, but recommended)
# by default the skeleton includes folders for most of the stuff you need
# so that it's easier to just drag&drop files there; but you really
# shouldn't have folders that you don't use in your package;
# so an optional but recommended step here would be to
# delete all .gitkeep files, so that you leave the
# empty folders empty; that way, you can still
# drag&drop stuff into them, but Git will
# ignore the empty folders
find . -name ".gitkeep" -print # shows all .gitkeep files, to double-check
find . -name ".gitkeep" -exec rm -rf {} \; # deletes all .gitkeep files

# commit the initially generated files
git add .
git commit -am "generated package code using laravel-packager and the backpack addon-skeleton"
```

Excellent. Now we have _two_ git repos, that we can use as a progress indicator:
- the _project_ repo, where the files are uncommitted;
- the _package_ repo, where we'll be moving the functionality;

If you've used a git client you can even place them side-by-side, and see the progress as you move files from the project (left) to the package (right). But you don't _have to_ do that, it's just a nice visual indicator if it's your first package:

![Two git repos - the project and the new package](https://user-images.githubusercontent.com/1032474/101024271-85af3100-357c-11eb-9a51-605b003a0a53.png)

<a name="define-dependencies"></a>
### Step 2. Define any extra dependencies

Your ```/packages/vendor-name/package-name/composer.json``` file already requires the latest version of Backpack (thanks to the addon skeleton). If your package needs any third-party packages apart from Backpack and Laravel, make sure to add them to the `require` section. Normally this just means cutting&pasting the line from your project's `composer.json` to your package's `composer.json`.


<a name="move-the-files-needed-for-the-theme"></a>
### Step 3. Move the functionality from your project to your package

Time to move files from your _project_ to your _package_. You can use whatever you want for that - drag&drop, the command line, your IDE or editor, whatever you want.

![Moving files from your project to your package](https://user-images.githubusercontent.com/1032474/101025619-821ca980-357e-11eb-82fb-0d0e57ad6e3f.gif)


As you do that, your `git status` or git client should show fewer and fewer files in your _project_, and more and more files in your _package_.

<a name="test-that-it-works"></a>
### Step 4. Test that the package works

That's pretty much it. You've created your package! 🥳 All the files your package need are inside your package, and the only remaining changes in your project (as reflected by `git status`) should be the minimal changes that users need to do to install your package.

Go ahead and test it in the browser. Make sure the functionality that was working inside your _project_ is still working now that it's inside a _package_. You might have forgotten something - we all do sometimes.

<a name="delete-files-you-dont-need"></a>
### Step 5. Delete the package files you don't need

Now that you know your package is working, go through the package folder and delete whatever your package isn't actually using: empty directories, empty files, placeholder files. Clean it up a little bit.


<a name="customize-markdown-files"></a>
### Step 6. Customize Markdown Files

Inside your package folder, go through all markdown files and make them your own. At the very least, open the `README.md` file and spend a little time on it, give it some love:
- write a clear description;
- add a screenshot if appropriate;
- write clear installation instructions (step-by-step) for how to use your package;
- answer any questions users might have about what the package does;
- link to whatever dependencies or resources your add-on uses, so that they check out their docs instead of bugging you about it;

If you plan to make this package public, take the `README.md` seriously, because it's a HUGE factor in how popular your package can become. If you include clear documentation and screenshots, more people will use your package - guaranteed.


<a name="put-the-package-online"></a>
## Part C. Put The Package Online


First, [create a new GitHub Repository](https://github.com/new) for it. Remember to use the same name you defined in your package's ```composer.json```. If in doubt, double-check.

Second, add that new GitHub Repo as a remote, and push your code to your new GitHub repo.

```bash
# save your working files to Git
git add .
git commit -am "working code for v1.0.0"

# add the remote to Github
git remote add origin git@github.com:yourusername/yourrepository.git
git branch -M main
git push -u origin main
git tag -a 1.0.0 -m 'First version'
git push --tags
```

The tags are the way you will version your package, so it's important you do it.


<a name="make-the-package-public"></a>
## Part D. Make the package public (on Packagist)

In order for people to be able to install your package using Composer, your package needs to be registered with [Packagist.org](https://packagist.org/), Composer's free package registry.

On [Packagist.org](https://packagist.org/), submit a new package. Enter your package's GitHub URL and click Check. If any errors occur, follow the onscreen instructions. When you're done, you're taken to your package's Packagist page.

**Congrats, you have a working package online**, you can now install it using Composer.

Note: On the package page, you might get a notice like this: _This package is not auto-updated. Please set up the [GitHub Service Hook](https://packagist.org/profile/) for Packagist so that it gets updated whenever you push!_ Let's take care of that. Click that link, get your API token and go to your package's GitHub page, in Settings / Webhooks & Services / Add a new service. Search for Packagist. Enter your username and the token and hit Submit. Your error in Packagist should disappear in 5–10 minutes.


<a name="install-the-repo-from-github"></a>
## Part E. Install the repo from GitHub

If you look close to your project's `composer.json` file, you'll notice your project is loading the package from `packages/vendor-name/package-name`. Which is fine, it's worked fine until now. But it's now time to install the package like your users will, and have it in `vendor/vendor-name/package-name`. That way:
- you test that your users are able to install the package;
- you don't commit anything extra inside your project;
- you can _easily_ make changes to your package, from whatever project you're using it in;

To do that, go ahead and do this to uninstall your package from your project:
```bash
cd ../../.. # so that you're inside your project, not package

# discard the changes in your composer files
# and delete the files from packages/vendor-name/package-name
php artisan packager:remove vendor-name package-name
```

And now install it exactly the same as your users will:
- if it's closed-source, add the GitHub repo to your `composer.json` then move on to the next step; do NOT do this if your package is open-source:

```json
    "repositories": {
        "vendor-name/package-name": {
            "type": "vcs",
            "url": "https://github.com/vendor-name/package-name"
        }
    }
```

- both for closed-source and open-source (on Packagist), install it using Composer's `--prefer-source` flag, so that it pulls the actual GitHub repo:

```bash
composer require vendor-name/package-name --prefer-source
```

That's it. It should be working fine now, but from the `vendor/vendor-name/package-name` directory. You can `cd vendor/vendor-name/package-name` and you'll see that you can `git checkout master`, make changes, tag releases, push to GitHub, everything.


<a name="feedback-and-promotion"></a>
### Feedback and Promotion

Congratulations on your new Backpack theme! To get feedback, ask people to try it by opening a Discussion in the [Backpack Community Forum](https://github.com/Laravel-Backpack/community-forum/discussions). After you've gotten some feedback, and a few users have installed your package and everything seems fine, time to promote it big time:
- post it to [laravel-news.com/links](https://laravel-news.com/links)
- show it off in the [Laracasts forum](https://laracasts.com/discuss)
- ask people to try it in [laravel.io](https://laravel.io/forum)

Have patience. It takes time to build up a user base, especially if it's your first open-source package. But treat every user as a friend, and you'll soon get there!
