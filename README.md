# Flowmailer PHP SDK

Flowmailers PHP 8.0 SDK for api.flowmailer.net REST API

## Getting started

### Installation

See [docs.php-http.org](https://docs.php-http.org/en/latest/httplug/users.html) for details on the HttpClient discovery.

A normal installation would be something equal to:
```bash
$ composer require symfony/http-client nyholm/psr7 flowmailer/flowmailer-php80-sdk
```

Choose your preferred [client implementations](https://packagist.org/providers/psr/http-client-implementation) on packagist.  

See how to force the usage of a specific implementation when many are available in your project on [github.com/php-http/discovery](https://github.com/php-http/discovery#usage-as-a-library-user).

### Basic usage

#### Submit a message
```php
<?php

require 'vendor/autoload.php';

use Flowmailer\API\Enum\MessageType;
use Flowmailer\API\Flowmailer;
use Flowmailer\API\Model\SubmitMessage;

// The credentials can be obtained in your Flowmailer account
$accountId    = '...';
$clientId     = '...';
$clientSecret = '...';

$flowmailer = Flowmailer::init($accountId, $clientId, $clientSecret);

$submitMessage = (new SubmitMessage())
    ->setMessageType(MessageType::EMAIL)
    ->setSubject('An e-mail message')
    ->setRecipientAddress('your-customer@email.org')
    ->setSenderAddress('info@your-company.com')
;
```

#### Loop over messages that were submitted earlier
```php
<?php

use Flowmailer\API\Collection\MessageCollection;

$flowmailer           = Flowmailer::init($accountId, $clientId, $clientSecret);
$pageSize             = 100;
$savedReferenceOrNull = null; // Get reference from database or cache (null will start from the beginning of the list)
$referenceRange       = new ReferenceRange($pageSize, $savedReferenceOrNull);

while ($referenceRange instanceof ReferenceRange) {
    /** @var MessageCollection $result */
    $result = $flowmailer->getMessages($referenceRange);

    // Do stuff with the result here

    // Store $referenceRange->getReference() value here in database or cache as input for a future run
    // Now we set the ReferenceRange for the next loop in memory
    $referenceRange = $result->getNextRange();
}
```

### Advanced usage

See [advanced usage] for examples on caching, logging and sending multiple messages at once.  
See [queue messages] for info on queueing messages for later consumption.

[advanced usage]: docs/advanced-usage.md "See advanced usage"
[queue messages]: docs/queue-messages.md "See queue messages"
