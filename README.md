BernardBundle
=============

bernardphp.com Symfony Bundle

Add to your kernel:

```php
new Bernard\BernardBundle\BernardBundle(),
```

Add to your config:

```yml
bernard:
    driver: dbal
    serializer: simple
    dbal: default
#    redis:
#        host:
#        port:
#    ironmq:
#        token:
#        project:
#    sqs:
#        key:
#        secret:
#        region:
```

By default no configuration is required and the bundle will assume the doctrine driver.

Before using run the following command to set schema for Doctrine driver:

```
php app/console bernard:dbal-schema --force
CREATE TABLE bernard_queues (name VARCHAR(255) NOT NULL, ...
CREATE TABLE bernard_messages (id INT UNSIGNED AUTO_INCREMENT NOT NULL, ...
```

##Usage

This bundle comes with two message handlers or so called receivers in Bernard.
They are the `EchoMessageHandler` and the `CommandMessageHandler`. Let's use
them as examples on what are the things that you can do with Bernard.

To begin produce a message intended to be consumed by `EchoMessageHandler`
service let's type the following on the prompt:
```
php app/console bernard:produce EchoMessageHandler "{\"message\": \"Message sample\"}"
```

The producer will create a queue called `echo-message-handler` and pump the
message in that queue awaiting to be consumed.

In order to consume the messages on a specific queue, in our case `echo-message-handler`
we can now issue the following command:
```
php app/console bernard:consume echo-message-handler
```
This will consume the message by going to the queue called `echo-message-handler`
and taking off a job and calling the method `echoMessageHandler` from the
service class `EchoMessageHandler`. Notice that because the method of the
class `EchoMessageHandler` takes as argument a `DefaultMessage` message, the
JSON message passed through the command line will be turned into a decoded
array and finally into properties existing on our message. So they can be
accessed simply as normal properties `$message->property`.

Let's take a look at another example, in this case we will use the `CommandMessageHandler`.

```
php app/console bernard:produce CommandMessageHandler "{\"command\": \"list\"}"
```

The producer will create a queue called `command-message-handler` and pump the
message in that queue awaiting to be consumed.

Let's consume the queue and run the command.
```
```
To see the bundle in action with a sample message take a look at: https://github.com/stanlemon/bernard-bundle-app

##License

All code is licensed MIT. See LICENSE file on root directory.
