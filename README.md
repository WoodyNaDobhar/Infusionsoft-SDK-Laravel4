PHP iSDK for Laravel 4
==================
This is a port of [Infusionsoft's PHP SDK](https://github.com/infusionsoft/PHP-iSDK) to Laravel 4 inspired by a [similar project](https://github.com/tshafer/Infusionsoft-for-Laravel-4/tree/master/src/Chrono/Infusionsoft) created by [tshafer](https://github.com/tshafer).

Motivation
==================
Infusionsoft's API is business critical and being a control freak I wanted to take ownership over merging in future updates. I'll also be writing a db based logging system to track execution time on a per API call basis.

Installation Instructions
==================
Install the [package](https://packagist.org/packages/woodynadobhar/infusionsoft) using composer and add the following entry to the array of service providers found in app/config/app.php:

`'WoodyNaDobhar\Infusionsoft\InfusionsoftServiceProvider',`

Also, add the facade alias to aliases array found in the same file:

`'Infusionsoft'	  => 'WoodyNaDobhar\Infusionsoft\Facades\Infusionsoft',`

You'll also need to publish the config file to your app/config/ directory by running the following from command line in your project root:

`php artisan config:publish woodynadobhar/infusionsoft`

This will copy the config file to app/config/packages/woodynadobhar/infusionsoft/ where you must set up your Infusionsoft application name and API key.

Usage
==================
There are two ways of using this package. The first is to instantiate the iSDK object then run its methods: 

Add the following to your app/routes.php and visit http://www.yourdomain.com/infusionsoft-test

```php
Route::get('/infusionsoft-test', function()
{
	$sdk = Infusionsoft::sdk();
	$contactId = 12345;
	$result = $sdk->loadCon($contactId, array('FirstName','LastName','Email'));
	
	dd($result);
});
```

The second way is to instantiate the iSDK object then use Infusionsoft::execWithLog() to run the desired method:

```php
Route::get('/infusionsoft-test', function()
{	
	$sdk = Infusionsoft::sdk();
	$contactId = 12345;
	$args = array( $contactId, array('FirstName','LastName','Email') );
	$result = Infusionsoft::execWithLog( $sdk, 'loadCon', $args );
	dd($result);
});
```

Using this second method is beneficial if you plan to use the API transaction time logging feature (coming soon) without needing to modify your code in the future. As of now Infusionsoft::execWithLog() just runs the requested method without any logging. 

