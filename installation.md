- [Introduction](#introduction)
    - [Requirements](#requirements)
    - [Installing](#installing)
    - [Resolving dependences](#resolving-dependences)


<a name="introduction"></a>
## [Introduction](#introduction)

[alert type="info"]The Assely framework perfectly integrates with Bedrock from Roots.io, our documentation has a whole dedicated chapter about that. If you are using Bedrock development stack — highly recommended — [I encourage you to read it too](/docs/bedrock).[/alert]

<a name="requirements"></a>
### [Requirements](#requirements)

The Assely framework uses modern development tools, because of that his "must have" requirements are higher than standard WordPress installation. Your server must fulfill this requirements:

- PHP >= 5.6
- MySQL >= 5.6
- The mod_rewrite Apache module

... and the most important, at least **WordPress 4.4**.

<a name="installing"></a>
### [Installing](#installing)

Assely behaves as normal WordPress theme and should be installed inside themes directory. By default it is `wp-content/themes`, but if you are using some sort of development stack (Bedrock, WordPlate), this may be somewhere else.

#### Via Assely Installer

The Assely Installer is a recommended way to create new Assely applications. It has to be installed only once as Composer global package. Open your terminal and type:

```bash
composer global require "assely/installer"
```
[alert type="warning"]Be sure to add the `$HOME/.composer/vendor/bin` directory in your system $PATH, so the `assely` command can be executed globally. (`export PATH="$PATH:$HOME/.composer/vendor/bin"`)[/alert]

Afterward, you should have a global `assely` command. Now, you can create a new Assely application by simply typing:

```bash
assely new project-name
```

Where `project-name` is a folder name of your new application. The latest release of files will be automatically downloaded into this directory.

#### Via Composer as project

You can also use composer `create-project` command.

```bash
composer create-project --prefer-dist assely/assely project-name
```

#### Via Git clone

Clone the latest release from GitHub.

```bash
git clone git://github.com/assely/assely.git project-name
```

<a name="resolving-dependeces"></a>
### [Resolving dependencies](#resolving-dependeces)

[alert type="danger"]**Skip this paragraph if you are using a development stack**. Do not manually install any of Assely plugins. Instead, require Assely packages inside stack's main `composer.json` file. Read more in [bedrock docs](/docs/bedrock/#installation).[/alert]

Before go on, we need to pull in framework. It is simple as sounds, go to the `wp-content/plugins` directory and run `assely fetch:framework`. This command will download the latest release of the framework to the current directory and resolve its composer dependencies for you.

```bash
cd wp-content/plugins

assely fetch:framework
```

Then go to the "Plugins" dashboard and activate the plugin. Familiar with a console? You can also use WP-CLI `plugin activate` command.

```bash
wp plugin activate assely-framework
```

Last step. We need to install application composer dependencies and generate an autoloading file. Get into application root directory (where `composer.json` file is located) and run `composer install -o` command.

```bash
cd project-name

composer install -o
```

That's it! Activate your new theme and visit the homepage. If everything went well, you should see a welcome splash. Good job!
