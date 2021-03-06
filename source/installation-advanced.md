Advanced Installation Options
=============================

Whilst Bolt is designed to be simple for anyone to install, its core functionality is also modular and easy to configure for anyone who is comfortable making a few modifications to their bootstrap code.

By default Bolt ships with its own bootstrap file which is responsible for setting up and running a Bolt application. Most of the advanced configuration options are best attained by taking over this responsibility in your own application. In the root of your project you will see an `index.php` file that should look like this:

<pre class="brush: php">
require_once __DIR__ . '/app/bootstrap.php';
$app->run();
</pre>

Ideally you would replace the path to bootstrap.php with a link to your own bootstrap file, for example:

<pre class="brush: php">
require_once __DIR__ . '/custom-bootstrap.php';
$app->run();
</pre>

### The basics of configuring a Bolt application

The job of your custom bootstrap file is to provide a bootstrapped `$app` object that the `index.php` file will then run. The simplest possible bootstrap file will look like this, assuming that your bootstrap file is in the same directory as your index.php file:

<pre class="brush: php">
// custom-bootstrap.php
$configuration = new Bolt\Configuration\Standard(__DIR__);
$app = new Bolt\Application(array('resources'=>$configuration));
</pre>


### Installing Bolt as a composer package.

As in the previous example, Bolt provides out of the box support for running as an installed Composer package, you will however need to make a couple of additions to your local `composer.json` file as well. Bootstrapping your app looks very similar but we will use the `Composer` configuration class instead.

<pre class="brush: php">
// custom-bootstrap.php
$configuration = new Bolt\Configuration\Composer(__DIR__);
$app = new Bolt\Application(array('resources'=>$configuration));
</pre>

##### Composer.json modifications

You also need to add the below to your `composer.json` file:

<pre class="brush: plain">
    "scripts": {
        "post-install-cmd": [
            "Bolt\\Composer\\ScriptHandler::installAssets"
        ],
        "post-update-cmd": [
            "Bolt\\Composer\\ScriptHandler::installAssets"
        ]
    },
    
    "extra":{
        "bolt-web-dir": "./"
    }
</pre>

This will take care of copying the required assets from the `vendor` directory into your local project.

### Customising your configuration

Your configuration that is passed into the `Bolt\Application` constructor is referred to throughout the execution of a Bolt app to decide what local resources to use, for instance whenever a Bolt component wants to write to the cache, it will ask your configuration what path to use. Here's a selection of some of the things you can alter before an app is initialised.

<pre class="brush: php">
// custom-bootstrap.php
$configuration = new Bolt\Configuration\Standard(__DIR__);

// Some customisations:
$configuration->setPath('cache',    'my/custom/cache');
$configuration->setPath('config',   'my/custom/config');
$configuration->setPath('database', 'my/custom/database');
$configuration->setPath('files',    'my/custom/filestore');
$configuration->setPath('web',      'my/public');

$app = new Bolt\Application(array('resources'=>$configuration));
</pre>





