Scribe PHP
==========

## Install

```
composer require "tranch/scribe-php" 0.1.0
```

## Usage

```php
<?php
use Thrift\Protocol\TBinaryProtocol;
use Thrift\Transport\TFramedTransport;
use Thrift\Transport\TSocketPool;

$messages = array();

$msg1 = new LogEntry();
$msg1->category = 'foo';
$msg1->message = 'foo message';
$messages[] = $msg1;

$msg2 = new LogEntry();
$msg2->category = 'bar';
$msg2->message = 'bar message';
$messages[] = $msg2;

list($host, $post,) = array_slice($argv, 1);

$socket = new TSocketPool(array($host), array(intval($port)));
$transport = new TFramedTransport($socket);
$protocol = new TBinaryProtocol($transport);

$scribeClient = new scribeClient($protocol);

$transport->open();
$resultCode = $scribeClient->Log($messages);
$transport->close();

assert($resultCode == ResultCode::OK);
```
