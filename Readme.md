Patch for Magento 1.9 on PHP 5.6 SMTP Connection with TLS with Self-signed Certificate
===================================================================================

Description
-----------
As PHP 5.6 changed the ssl behaviour, it verifys the hostname and the certification CN name by default which is different from PHP 5.5. Some SMTP service will not work after upgrade the PHP version to 5.6. I create this patch to make this SSL behaviour same as the PHP 5.5

I just add the follow code to lib/Zend/Mail/Protocol/Abstract.php

```
   $streamContext = stream_context_create([
       'ssl' => [
       'verify_peer' => false,
       'verify_peer_name' => false
       ]
   ]);
   $this->_socket = @stream_socket_client($remote, $errorNum, $errorStr, self::TIMEOUT_CONNECTION,STREAM_CLIENT_CONNECT, $streamContext);

```

Usage
-----

```
cd [your magento installed folder]
patch p1 < patch-magento-mail-php5.6.patch
```
