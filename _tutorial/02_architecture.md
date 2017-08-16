---
title: Architecture
permalink: /tutorial/architecture.html
---
## Architecture

![Alt text]({{ site.baseurl}}/public/architecture.svg)

The above diagram shows the general Tomoko flow of control from external request up to your code registered as handler on a given patch path.

Patch lifecycle starts outside of your server application. An HTTP PATCH request is sent to you server. It can be done by a microservice, an Angular / React app or just plain Javascript XHR code.
The only requirement is that the request follows the format specified by RFC 6902. Basically it is a list of operations. Each operation consists of a path (/tickets/1234/title) and a value being any valid JSON element.

The request arrives at your server and is passed to your controller. There the request body is parsed to a Patch object. You can write your own parser but Tomoko provides its implementation based on Jackson.

After the request has been successfully parsed it is passed to a Patcher object. It contains handlers that are to perform the actual modifications to the model. Handler is just a piece of your code that is registered in a patcher on a given path.
Patcher is responsible for analyzing contents of a patch and finding handlers for each operation defined there. It also converts each operation's value property to a type expected by the corresponding handler.

If handlers have been found for all operations and type conversions went well then each such handler is called with parameters extracted from the corresponding operation.
