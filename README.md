# RationalOptionPages

A PHP class for generating WordPress Option Pages.

## Installation

* Download or clone the repo
* Include `RationalOptionPages.php` in your file
* Instantiate the class with your array of pages

```php
require_once('RationalOptionPages.php');
$pages = array(
	'sample-page'	=> array(
		'page_title'	=> __( 'Sample Page', 'sample-domain' ),
	),
);
$option_page = new RationalOptionPages( $pages );
```

### Pages

Pages are added in a key/value syntax where the value is an array of parameters for the page itself.

#### Parameters

Based on [WordPress' `add_menu_page()` function](https://developer.wordpress.org/reference/functions/add_menu_page/).

* `page_title` - The text string for the title of the page __Required__.
* `menu_title` - The text string for the menu item itself. Defaults to `page_title`.
* `capability` - The permissions required to access this page. Defaults to `manage_options`.
* `menu_slug` - The "slug" of the menu item. Defaults to a "slugified" version of the `page_title`.
* `icon_url` - The URL of the menu icon. [WordPress' Dashicons are available](https://developer.wordpress.org/resource/dashicons). Defaults to `false`, renders "dashicons-admin-generic".
* `position` - The position where the menu item appears, from 1 to 99. Defaults to `null`, last.

If your page title is more than 2 words I recommend using the `menu_title` parameter to avoid wrapping. The default `capability` should work for most plugins and themes as only the admin would typically make higher level changes like these.

### Subpages

Subpages are differentiated by the `parent_slug` parameter. They can be added either at the top level of the pages array submitted to the class or under a `subpages` key within another, top-level page parameter array.

#### Parameters

Based on [WordPress' `add_submenu_page()` function](https://developer.wordpress.org/reference/functions/add_submenu_page/).

* `parent_slug` - The `menu_slug` of the parent page. [WordPress has several top-level menu items](https://codex.wordpress.org/Administration_Menus#Using_add_submenu_page). __Required__.
* `page_title` - The text string for the title of the page. __Required__.
* `menu_title` - The text string for the menu item itself. Defaults to `page_title`.
* `capability` - The permissions required to access this page. Defaults to `manage_options`.
* `menu_slug` - The "slug" of the menu item. Defaults to a "slugified" version of the `page_title`.

```php
require_once('RationalOptionPages.php');
$option_page = new RationalOptionPages( array(
	'sample-page'	=> array(
		'page_title'	=> __( 'Sample Page', 'sample-domain' ),
		// via the subpages key
		'subpages'		=> array(
			'sub-page-one'	=> array(
				'page_title'	=> __( 'Sub Page One', 'sample-domain' ),
			),
		),
	),
	// via the pages array itself
	'sub-page-two'	=> array(
		'parent_slug'	=> 'sample_page',
		'page_title'	=> __( 'Sub Page Two', 'sample-domain' ),
	),
	// sub page of the "Appearance" menu item
	'sub-theme'	=> array(
		'parent_slug'	=> 'themes.php',
		'page_title'	=> __( 'Sub Theme', 'sample-domain' ),
	),
) );
```
