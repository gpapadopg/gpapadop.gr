---
title: Push notifications to iOS application via PHP
tags:
    - PHP
    - iOS
---

This is a skeleton on how to send push notifications to iOS devices from PHP
with the help of [ApnsPHP](https://github.com/immobiliare/ApnsPHP) library:

```
// Get all device ids uninstalled the application
$feedback = new ApnsPHP_Feedback(
    ApnsPHP_Abstract::ENVIRONMENT_SANDBOX,
    'path/to/push-ck-sandbox.pem'
);
$feedback->setProviderCertificatePassphrase('MY-PASSPHRASE');
$feedback->connect();
$deviceTokens = $feedback->receive();
$feedback->disconnect();
foreach ($deviceTokens as $deviceId) {
    // Remove $deviceId from our database
}

// Now construct a list of active device ids
$devices = [];

// Get them from our database of other data provider
// ...

// Instantiate a new ApnsPHP_Push object
$push = new ApnsPHP_Push(
    ApnsPHP_Abstract::ENVIRONMENT_SANDBOX,
    'path/to/push-ck-sandbox.pem'
);

// Set the Provider Certificate passphrase
$push->setProviderCertificatePassphrase('MY-PASSPHRASE');

// Set the Root Certificate Autority to verify the Apple remote peer
$push->setRootCertificationAuthority(storage_path('certificates/entrust_2048_ca.cer'));

// Connect to the Apple Push Notification Service
$push->connect();

foreach ($devices as $deviceId) {
    // Instantiate a new Message with a single recipient
    $message = new ApnsPHP_Message($deviceId);

    // Set a custom identifier
    // To get back this identifier use the getCustomIdentifier() method
    // over a ApnsPHP_Message object retrieved with the getErrors() message.
    $message->setCustomIdentifier("Message-Badge-3");

    // Set badge icon to "3"
    $message->setBadge(1);

    // Set a simple welcome text
    $message->setText('simple welcome text');

    // Play the default sound
    $message->setSound();

    // Set a custom property
    $message->setCustomProperty('acme2', array('bang', 'whiz'));

    // Set another custom property
    $message->setCustomProperty('acme3', array('bing', 'bong'));

    // Set the expiry value to 30 seconds
    $message->setExpiry(30);

    // Add the message to the message queue
    $push->add($message);
}

// Send all messages in the message queue
$push->send();

// Disconnect from the Apple Push Notification Service
$push->disconnect();

// Examine the error message container
$errors = $push->getErrors();
```

Also, at the library's github page, there are instructions on [how to generate the push certificate](https://github.com/immobiliare/ApnsPHP/blob/master/Doc/CertificateCreation.md) and a [demo iOS application](https://github.com/immobiliare/ApnsPHP/tree/master/Objective-C%20Demo).
