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

## Basic Google Maps Geocoding API Request ##

HTTPRequest is also great for making calls to 3rd-party APIs. Here's a Geocoding request to Google Maps:

	$http = new HTTPRequest('maps.googleapis.com', '/maps/api/geocode/json');
	$params = array(
	  'address' => 'The Moon',
	  'sensor' => false
	);
	$http -> setQueryParams($params);
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

## For fans of chaining ##

Many of the HTTPRequest class' methods return the instance itself, enabling chaining (if you please).

	$http = new HTTPRequest('maps.googleapis.com', '/maps/api/geocode/json');
	$params = array('address'=>'The Moon','sensor'=>false);
	echo $http -> setQueryParams($params) -> execute()  -> getResponseText();
	$http -> close();

## HTTPS/SSL Support ##

HTTPRequest also supports HTTPS/SSL for both cURL and fsockopen. For example, you can make calls to the Trello API in a snap:

	$http = new HTTPRequest('api.trello.com', '/1/boards/4d5ea62fd76aa1136000000c', 443);
	$params = array(
		'key' => '[YOUR_DEVELOPER_KEY]'
	);
	$http -> setQueryParams($params);
	$http -> execute();
	echo $http -> getResponseText();
	$http -> close();
	
We get back some info about the Trello development board:

	{"id":"4d5ea62fd76aa1136000000c","name":"Trello Development","desc":"Trello board used by the Trello team to track work on Trello.  How meta!\n\nThe development of the Trello API is being tracked at https://trello.com/api\n\nThe development of Trello Mobile applications is being tracked at https://trello.com/mobile","closed":false,"idOrganization":"4e1452614e4b8698470000e0","url":"https://trello.com/board/trello-development/4d5ea62fd76aa1136000000c","prefs":{"voting":"public","permissionLevel":"public","invitations":"members","comments":"public"}}