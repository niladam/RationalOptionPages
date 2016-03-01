# RationalOptionPages

A PHP class for generating WordPress Option Pages.

## Installation

* Download or clone the repo
* Include `RationalOptionPages.php` in your file
* Instantiate the class with your array of pages

```php
	include_once('RationalOptionPages.php');
	$option_page = new RationalOptionPages( array(
		'sample-page'	=> array(
			'page_title'	=> __( 'Sample Page', 'sample-domain' ),
		),
	) );
```