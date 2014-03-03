# Laravel HTML Tidy Filter

[Laravel-html-tidy](https://github.com/Stolz/laravel-html-tidy) is a [Laravel filter](http://laravel.com/docs/routing#route-filters) that parses Laravel's Response objects in order to detect and fix markup problems as well as to improve the layout and indent style of the resulting markup.

## tl;dr
When editing HTML it's easy to make mistakes. Did you ever forget to close a `<div>` tag that made a mess of all your layout and then you went crazy trying to figure out what/where the problem was?. Wouldn't it be nice if there was a simple way to detect and fix these mistakes automatically and at the same time tidy up sloppy editing into nicely layed out markup? Well that is what [W3C HTML Tidy](http://www.w3.org/People/Raggett/tidy/) utility is for!. HTML Tidy is available as a [PHP extension](http://www.php.net/manual/en/book.tidy.php) and this package makes using it with Laravel a breeze.


## Requirements

- PHP compiled with HTML Tidy extension enabled.
- Laravel

## Installation

To get the latest version of the filter simply require it in your composer.json file by running:

	composer require "stolz/laravel-html-tidy:dev-master"

Once the package is installed you need to register the service provider with the application. Open up `app/config/app.php` and find the `providers` key.

	'providers' => array(
		'Stolz\Filter\HtmlTidyServiceProvider',

Now add the filter to the bottom of your `app/filters.php` file:

	Route::filter('html-tidy', 'Stolz\Filter\HtmlTidy');

Here the filter will be named "html-tidy". Feel free to use any other name but remember to use that name when attaching the filter to your routes.


## Usage

After following the instructions above the filter is installed and ready to be used as an **"after"** filter. Follow [standard procedure](http://laravel.com/docs/routing#route-filters) to attach the filter to any route(s) you want.

Example `routes.php`

	// Attach to a route with closure
	Route::get('some/url', array(
		'after' => 'html-tidy',
		function() {
			return View::make('hello');
		}
	));

	// Attach to controller route
	Route::get('another/url', array('after' => 'html-tidy', 'uses' => 'Controller@method'));

	// Attach to a collection of routes
	Route::group(array('after' => 'html-tidy'), function() {
		Route::get('foo', 'Controller@method1');
		Route::get('bar', 'Controller@method2');
	});

You can also attach it to all of your routes without having to define a route group by editing `filters.php` and adding the following to the `App::after()` function:

	App::after(function($request, $response)
	{
		return App::make('stolz.filter.tidy')->filter(null, $request, $response);
	});


## Configuration

To configure the package, you can use the following command to copy the configuration file to `app/config/packages/stolz/laravel-html-tidy/config.php`.

	php artisan config:publish stolz/laravel-html-tidy

All available settings are included inside `config.php` and with the provided comments they should be self-explanatory.

## License

MIT License
(c) [Stolz](https://github.com/Stolz)
