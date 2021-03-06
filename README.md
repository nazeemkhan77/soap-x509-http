# soap-x509-http
> An X.509 http client for [node-soap](https://github.com/vpulim/node-soap)

This module provides an HTTP client for accessing web services that require X.509 certificates.
It's based on [node-soap](https://github.com/vpulim/node-soap)'s default HTTP client and relies on [ws.js](https://github.com/yaronn/ws.js) for the signing process.


## Install

Install with [npm](http://github.com/isaacs/npm):

```
npm install soap
npm install soap-x509-http

```

## Usage

**new X509HttpClient(requestOptions, singingCredentials);** creates a new http client to be used on node-soap.

This example assumes that the web service requires a TLS client certificate. If this is not needed then the first parameter can be left empty.


```
var fs = require('fs');
var soap = require('soap');
var X509HttpClient = require('soap-x509-http');


var myX509Client = new X509HttpClient({
  pfx: fs.readFileSync('<your-client-certificate.pfx>'),
  passphrase: '<password>',
  ca: fs.readFileSync('<ca-certificate.pem>'),
  rejectUnauthorized: false
}, {
  key: fs.readFileSync('<your-client-key.pem>')
});


var wsdl = './WebService.wsdl';
var args = {name: 'value'};

soap.createClient(wsdl, {
  httpClient: myX509Client
},
function(err, client) {
  client.setEndpoint('<webservice-endpoint>');

  client.MyFunction(args, function(err, result) {
    console.log(result);
  });
});

```

## More info
For more references and examples on how to use **node-soap**, please check:

- [node-soap](https://github.com/vpulim/node-soap) - A SOAP client and server for node.js.
- [ws.js](https://github.com/yaronn/ws.js) - A WS-* client stack for node.js. Written in pure javascript!
