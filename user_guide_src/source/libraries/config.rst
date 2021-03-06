############
Config Class
############

The Config class provides a means to retrieve configuration preferences.
These preferences can come from the default config file
(application/config/config.php) or from your own custom config files.

.. note:: This class is initialized automatically by the system so there
	is no need to do it manually.

.. contents:: Page Contents

Anatomy of a Config File
========================

By default, CodeIgniter has one primary config file, located at
application/config/config.php. If you open the file using your text
editor you'll see that config items are stored in an array called
$config.

You can add your own config items to this file, or if you prefer to keep
your configuration items separate (assuming you even need config items),
simply create your own file and save it in config folder.

.. note:: If you do create your own config files use the same format as
	the primary one, storing your items in an array called $config.
	CodeIgniter will intelligently manage these files so there will be no
	conflict even though the array has the same name (assuming an array
	index is not named the same as another).

Loading a Config File
=====================

.. note::
	CodeIgniter automatically loads the primary config file
	(application/config/config.php), so you will only need to load a config
	file if you have created your own.

There are two ways to load a config file:

Manual Loading
**************

To load one of your custom config files you will use the following
function within the :doc:`controller </general/controllers>` that
needs it::

	$this->config->load('filename');

Where filename is the name of your config file, without the .php file
extension.

If you need to load multiple config files normally they will be
merged into one master config array. Name collisions can occur,
however, if you have identically named array indexes in different
config files. To avoid collisions you can set the second parameter to
TRUE and each config file will be stored in an array index
corresponding to the name of the config file. Example::

	// Stored in an array with this prototype: $this->config['blog_settings'] = $config
	$this->config->load('blog_settings', TRUE);

Please see the section entitled Fetching Config Items below to learn
how to retrieve config items set this way.

The third parameter allows you to suppress errors in the event that a
config file does not exist::

	$this->config->load('blog_settings', FALSE, TRUE);

Auto-loading
************

If you find that you need a particular config file globally, you can
have it loaded automatically by the system. To do this, open the
**autoload.php** file, located at application/config/autoload.php,
and add your config file as indicated in the file.


Fetching Config Items
=====================

To retrieve an item from your config file, use the following function::

	$this->config->item('item name');

Where item name is the $config array index you want to retrieve. For
example, to fetch your language choice you'll do this::

	$lang = $this->config->item('language');

The function returns FALSE (boolean) if the item you are trying to fetch
does not exist.

If you are using the second parameter of the $this->config->load
function in order to assign your config items to a specific index you
can retrieve it by specifying the index name in the second parameter of
the $this->config->item() function. Example::

	// Loads a config file named blog_settings.php and assigns it to an index named "blog_settings"
	$this->config->load('blog_settings', TRUE);

	// Retrieve a config item named site_name contained within the blog_settings array
	$site_name = $this->config->item('site_name', 'blog_settings');

	// An alternate way to specify the same item:
	$blog_config = $this->config->item('blog_settings');
	$site_name = $blog_config['site_name'];

Setting a Config Item
=====================

If you would like to dynamically set a config item or change an existing
one, you can do so using::

	$this->config->set_item('item_name', 'item_value');

Where item_name is the $config array index you want to change, and
item_value is its value.

.. _config-environments:

Environments
============

You may load different configuration files depending on the current
environment. The ENVIRONMENT constant is defined in index.php, and is
described in detail in the :doc:`Handling
Environments </general/environments>` section.

To create an environment-specific configuration file, create or copy a
configuration file in application/config/{ENVIRONMENT}/{FILENAME}.php

For example, to create a production-only config.php, you would:

#. Create the directory application/config/production/
#. Copy your existing config.php into the above directory
#. Edit application/config/production/config.php so it contains your
   production settings

When you set the ENVIRONMENT constant to 'production', the settings for
your new production-only config.php will be loaded.

You can place the following configuration files in environment-specific
folders:

-  Default CodeIgniter configuration files
-  Your own custom configuration files

.. note::
	CodeIgniter always tries to load the configuration files for
	the current environment first. If the file does not exist, the global
	config file (i.e., the one in application/config/) is loaded. This means
	you are not obligated to place **all** of your configuration files in an
	environment folder − only the files that change per environment.

Helper Functions
================

The config class has the following helper functions:

$this->config->site_url();
***************************

This function retrieves the URL to your site, along with the "index"
value you've specified in the config file.

$this->config->base_url();
***************************

This function retrieves the URL to your site, plus an optional path such
as to a stylesheet or image.

The two functions above are normally accessed via the corresponding
functions in the :doc:`URL Helper </helpers/url_helper>`.

$this->config->system_url();
*****************************

This function retrieves the URL to your system folder.
