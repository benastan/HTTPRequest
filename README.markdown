HTTPRequest.php
===============

## Easy HTTP Requests with PHP ##

HTTPRequest simplifies cURL and fsockopen requests around Object-Oriented concepts. It serves as a superb foundation for PHP-based API integrations and allows a range of customization of headers, query parameters and automatically degrades to fsockopen when cURL is unavailable.  

## A Most Basic Request ##

Request www.google.com:

	$http = new HTTPRequest('www.google.com');
	$http -> execute();
	echo $http -> getResponseText();
	$http -> close();

Echoes:
    
	<!doctype html><html itemscope itemtype="http://schema.org/WebPage"><head><meta http-equiv="content-type" content="text/html; charset=UTF-8"><meta name="description" content="Search the world&#39;s information, including webpages, images, videos and more. Google has many special features to help you find exactly what you&#39;re looking for."><meta name="robots" content="noodp"><meta itemprop="image" content="/images/google_favicon_128.png"><title>Google</title>

Nice!

## Basic Trello API Request ##

HTTPRequest is also great for making calls to 3rd-party APIs. Here's a Geocoding request to Google Maps:

	$http = new HTTPRequest('maps.googleapis.com', '/maps/api/geocode/json');
	$http -> setQueryParams(array(
	  'address' => 'The Moon',
	  'sensor' => false
	));
	$http -> execute();
	echo $http -> getResponseText();
	$http -> close();

And we get back some pretty JSON:

	{
		"results" : [
		{
		 "address_components" : [
		    {
		       "long_name" : "Moon",
		       "short_name" : "Moon",
		       "types" : [ "route" ]
	            },
	...

In this case, the constructor took a second parameter (the uri) in addition to first property (which was the host name). The constructor can also take the port number, a boolean directing whether or not to use cURL if possible and the timeout in seconds.

	public function __construct($host = null, $uri = '/', $port = 80, $useCurl = null, $timeout = 10)

## If you hate readability ##

If you hate readability, you can always write it like so:

	$params = array('address'=>'The Moon','sensor'=>false);
	$http = new HTTPRequest('maps.googleapis.com', '/maps/api/geocode/json');
	echo $http -> setQueryParams($params) -> execute()  -> getResponseText();
	$http -> close();
	