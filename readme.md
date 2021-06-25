# Base Theme for WordPress

This is a very basic theme/plugin combination for starting out a new build. Front-end styles and scripts use a Gulp build process with SCSS styles. This is not a theme framework or "all-in-one" theme builder – it is a set of templates and processes meant to cut down on project setup time.

## Requirements
If running the accompanying plugin, PHP v5.3.2 or higher is required.

## Theme Setup
#### 1. Initial Setup
This repository contains only the theme and theme plugin. Core WordPress files and plugins are ignored in version control. After cloning this repo locally, follow the standard WordPress setup for your local site:

* Download the core WordPress files into the directory
* Add the `wp-content/uploads` directory and set appropriate permissions
* Setup your local MySQL database for the site
* Setup your local development domain
* Rename `htaccess.sample.txt` to `.htaccess`
* Update `wp-config.php` with your local development credentials

```
define( 'DB_NAME', 'dbname' );
define( 'DB_USER', 'dbuser' );
define( 'DB_PASSWORD', 'dbpassword' );
define( 'DB_HOST', 'hostname' );
```

* Set `WP_DEBUG` to `true` for local development.

*** 


#### 2. Install Plugin Dependencies

##### WPDB Migrate Pro

* Generate a new set of Composer API Keys from your [Delicious Brains Settings](https://deliciousbrains.com/my-account/settings/)
* Rename `auth.sample.json` to `auth.json`
  * Add the username and password created above. 

##### Advanced Custom Fields Pro
* Add an environment file to the repo directory
  * This file provides credentials to download the [Advanced Custom Fields Pro](https://advancedcustomfields.com) premium plugin.
  * File name: `.env.php`
  * File contents: `ACF_PRO_KEY={KEY}`
  * If you do not have an ACF Pro Key, contact the site owner or technical lead.

##### Install Plugins
* Update plugin dependencies in `composer.json`
* Run composer in the repository directory to install plugins
  * `composer install`

 
***

### Theme Development
The theme uses a [gulp.js](https://gulpjs.com/) build process for styles and scripts.

* `cd` into the theme directory
* Run `npm install` to install node dependencies
* Run `gulp` to start the build process
* Update the `acf-json` directory with server-writeable permissions. Custom fields are synched through json files.

#### Styles
Styles are compiled from scss files located in `assets/scss`. There are 2 primary source files: 

* `style.scss` 
  * Defines the theme attributes
  * Outputs to `style.css` in theme directory
  * All public theme style
  * Also set as the class editor styles for tinymce WYSIWYG fields
* `gutenberg.scss`
  * Outputs to `gutenberg.css`
  * Includes most styles from the public theme, as well as tinymce and gutenberg fixes/tweaks.

#### Colors
Theme colors are defined in `config.json`. This file is read to output a `_colors.scss` file automatically that contains the color variables and css classes specifically for the WordPress editor.

In addition, these colors define the color choices available in the WordPress editor. These are available as a array constant named `THEME_COLORS`.

##### Style Conventions
* Separate scss files should be added for each modular addition. 
* Custom blocks should have a separate scss file prefixed with `_block-`
* All variables are defined in `_variables.scss`
* All mixins are defined in `_mixins.scss`
* WordPress-specific formatting should be defined in `_wp-formats.scss`
* Global typographic formatting should be defined in `_typography.scss`

#### Javascript
Scripts are compiled from source files in `assets/js/src`. 

Scripts are setup modularily, with a separate class/file for each component. If that component has style dependencies, the same naming convention is used (ex: `sitename.accordions.js` as the JS class, `_accordions.scss` as the styles.

##### To add a new JS class:

* Create the source file.
  * This source file should extend the primary plugin JS class
  * All functionality related to the component should be contained in the class
* Instantiate this new class in the factory class `sitename.factory.js`
* Update `gulpfile.js` to include this new file
  * The filename should be added to the `js_source` array
* Stop gulp if running and restart the build process


## Plugin Setup

The theme plugin takes the place of and organizes functionality that would normally be placed in `functions.php` and any php includes within the theme.

The purpose for this is to better organize functionality and provide a more consistent coding convention.

Some elements that remain in `functions.php`:

* The theme version (used throughout the theme)
* Image size definition
* ACF options page registration
* Nav menu registration

### Namespacing, Autoloading and Dependencies
The theme plugin makes use of [composer](https://getcomposer.org/) for namespacing, autoloading and dependencies. There is no need to run composer for development, unless additional PHP packages are required. The `vendor` directory is tracked under version control so that composer updates are not required in production.

#### Namespaces
* The primary namespace is `Base`
* Sub namespaces should be organized into directories by responsibility
* Some key namespaces are:
  * Blocks - Registers all of our custom blocks
  * Activation - Enqueues assets and handles redirects
  * Gutenberg - Enables Gutenberg support and adds custom colors to the color picker
  * Display - Applies filters and functions related to the front-end display of the website
  * WPData - Registers custom post types, taxonomies, and meta fields

### Initial Setup
1. Rename the `base` plugin directory
2. Rename the namespace declaration in `composer.json`
3. Update namespaces in classes under `app/`
4. Run `composer install`
5. Add post types/taxonomies as needed in their respective classes

## Repository Setup
1. Edit `.gitignore` to reflect the new theme and plugin folder names after completing the steps above.