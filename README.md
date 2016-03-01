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

## Usage

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

* `parent_slug` - The `menu_slug` of the parent page. [WordPress has several top-level menu items](https://codex.wordpress.org/Administration_Menus#Using_add_submenu_page). __Required__ (unless added via the `subpages` key method).
* `page_title` - The text string for the title of the page. __Required__.
* `menu_title` - The text string for the menu item itself. Defaults to `page_title`.
* `capability` - The permissions required to access this page. Defaults to `manage_options`.
* `menu_slug` - The "slug" of the menu item. Defaults to a "slugified" version of the `page_title`.

#### Example

```php
require_once('RationalOptionPages.php');
$pages = array(
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
);
$option_page = new RationalOptionPages( $pages );
```

### Sections

Sections are added via a `sections` key in the page parameter arrays.

#### Parameters

Based on [WordPress' `add_settings_section()` function](https://developer.wordpress.org/reference/functions/add_settings_section/).

* `title` - The title of the section. __Required__.
* `id` - The ID of the section.
* `callback` - An optional parameter for generating custom, section content. __Requires `custom` parameter be set to `true`__.
* `custom` - A boolean option that indicates you want to use a custom callback. Defaults to `false`.
* `text` - An option parameter for adding HTML text under the section title.
* `include` - An option parameter that calls PHP's `include()` under the section title. __Use absolute path__.

#### Example

```php
require_once('RationalOptionPages.php');
$pages = array(
	'sample-page'	=> array(
		'page_title'	=> __( 'Sample Page', 'sample-domain' ),
		'sections'		=> array(
			'section-one'	=> array(
				'title'			=> __( 'Section One', 'sample-domain' ),
				'text'			=> '<p>' . __( 'Some HTML text to describe the section', 'sample-domain' ) . '</p>',
				'include'		=> plugin_dir_path( __FILE__ ) . '/your-include.php',
			),
			'section-two'	=> array(
				'title'			=> __( 'Section Two', 'sample-domain' ),
				'custom'		=> true,
				'callback'		=> 'custom_section_callback_function',
			),
		),
	),
);
$option_page = new RationalOptionPages( $pages );
```

## To Do

* Add `text` and `include` parameters to pages.
