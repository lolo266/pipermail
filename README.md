# pipermail

  node.js utilities for reading pipermail archives such as es-discuss

[![Build Status](https://travis-ci.org/esdiscuss/pipermail.png?branch=master)](https://travis-ci.org/esdiscuss/pipermail)
[![Dependency Status](https://gemnasium.com/esdiscuss/pipermail.png)](https://gemnasium.com/esdiscuss/pipermail)
[![NPM version](https://badge.fury.io/js/pipermail.png)](http://badge.fury.io/js/pipermail)

## Basic Usage

```javascript
var pipermail = require('pipermail');

var options = {};

//`pipermail` returns a stream of JSON objects.
//This can't be directly written to a file
var parsed = pipermail('https://mail.mozilla.org/pipermail/es-discuss/', options);

//convert the stream of json objects into a stream of JSON text seperated by new lines.
var stringified = parsed.pipe(pipermail.stringify());

//pipe to a file
stringified.pipe(require('fs').createWriteStream('res.txt'));

//compress to a file
stringified.pipe(require('zlib').createGzip())
  .pipe(require('fs').createWriteStream('res.txt.gz'));
```

The resulting `res.txt` would look something like:

```javascript
{"url":"https://mail.mozilla.org/pipermail/es-discuss/2006-June/003436.html","header":{"from":{"email":"baz@example.com","name":"Brendan Eich"},"date":"Sat, 3 Jun 2006 12:35:18 -0700","subject":"Welcome to the ECMAScript Edition 4 discussion list"},"body":"Thanks to Graydon Hoare for setting it up.\n\n/be"}
{"url":"https://mail.mozilla.org/pipermail/es-discuss/2006-June/003437.html","header":{"from":{"email":"bar@example.com","name":"Olav Junker Kjær"},"date":"Tue, 06 Jun 2006 15:40:48 +0200","subject":"ES4 translator"},"body":"Hello,\nI'm very pleased to s the new public specs for ES4"}
{"url":"https://mail.mozilla.org/pipermail/es-discuss/2006-June/003438.html","header":{"from":{"email":"foo@example.com","name":"Robert Sayre"},"date":"Wed, 7 Jun 2006 11:43:37 -0400","subject":"date literals"},"body":"I think the date literal should allow a trailing 'Z' to substitute for\n'+00:00'.\n\nRobert Sayre"}
```

I've shortened the bodies and renamed the e-mails but other than that it's the first few lines generated by the above code.

## Options

 - filterMonth: a function that gets the month's url as its argument and returns `true` or `false` to indicate whether the month should be included (or returns a promise if it's asynchronous)
 - filterMessage: a function that gets the message's url as its argument and returns `true` or `false` to indicate whether the message should be downloaded (or returns a promise if it's asynchronous)
 - months: the maximum number of months to download.  If set, only the most recent n months will be downloaded.
 - parallel: the maximum number of messages to download in parallel, defaults to `10`
 - parallelMonths: the maximum number of month index pages to download in parallel, defaults to `2`

## License

MIT
