HTTPRequest.php
===============

## Easy HTTP Requests with PHP ##

HTTPRequest simplifies cURL and fsockopen requests around Object-Oriented concepts. It serves as a superb foundation for PHP-based API integrations and allows a range of customization of headers, query parameters and automatically degrades to fsockopen when cURL is unavailable.  

## A Most Basic Request ##

    $http = new HTTPRequest('www.google.com');
		$http -> execute();
		echo $http -> getResponseText();
		$http -> close();
