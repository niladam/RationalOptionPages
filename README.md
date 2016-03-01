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

#### Parameters

Based on [WordPress' `add_menu_page()` function](https://developer.wordpress.org/reference/functions/add_menu_page/).

* `page_title` - The text string for the title of the page __Required__.
* `menu_title` - The text string for the menu item itself. Defaults to `page_title`.
* `capability` - The permissions required to access this page. Defaults to `manage_options`.
* `menu_slug` - The "slug" of the menu item. Defaults to a "slugified" version of the `page_title`.
* `icon_url` - The URL of the menu icon. [WordPress' Dashicons are available](https://developer.wordpress.org/resource/dashicons). Defaults to `false`, renders "dashicons-admin-generic".
* `position` - The position where the menu item appears, from 1 to 99. Defaults to `null`, last.