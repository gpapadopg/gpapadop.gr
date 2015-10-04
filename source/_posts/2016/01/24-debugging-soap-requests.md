---
title: Debugging SOAP requests
tags:
    - PHP
    - SOAP

---

If for some reason you have to deal with debugging on SOAP requests
(that is, to see the actual request and response bodies), you can
use these methods:

- [SoapClient::__getLastRequest()](http://php.net/manual/en/soapclient.getlastrequest.php)
- [SoapClient::__getLastRequestHeaders()](http://php.net/manual/en/soapclient.getlastrequestheaders.php)
- [SoapClient::__getLastResponse()](http://php.net/manual/en/soapclient.getlastresponse.php)
- [SoapClient::__getLastResponseHeaders()](http://php.net/manual/en/soapclient.getlastresponseheaders.php)

and remember to set the trace option when initializing the SOAP object. For example, using the free [webservicex.net](http://www.webservicex.net):

    <?php
    // initialize soap client
    $soap = new SoapClient(
        "http://www.webservicex.net/country.asmx?WSDL", [
        "trace" => true,
    ]);

    // call a method on soap object
    $soap->__call("getCountries", [
        // options for method
    ]);

    // output raw request, something like
    // <?xml version="1.0" encoding="UTF-8"?>
    // <SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" 
    //                    xmlns:ns1="http://www.webserviceX.NET">
    //     <SOAP-ENV:Body>
    //         <ns1:GetCountries/>
    //     </SOAP-ENV:Body>
    // </SOAP-ENV:Envelope>
    echo $soap->__getLastRequest(); 

    // output raw response, something like
    // <?xml version="1.0" encoding="utf-8"?>
    // <soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"
    //                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    //                xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    //     <soap:Body>
    //         <GetCountriesResponse xmlns="http://www.webserviceX.NET">
    //             <GetCountriesResult>
    //                 <!-- ... -->
    //             </GetCountriesResult>
    //         </GetCountriesResponse>
    //     </soap:Body>
    // </soap:Envelope>
    echo $soap->__getLastResponse(); 
