# Callback Events

There are four callback events that Haystack provides throughout its lifecycle. These are **then, catch, finally** and **paused**. You can use these methods to run application code like notifying a user.

{% hint style="warning" %}
Since these callback events are serialized and stored in the database you cannot use **$this** inside of the anonymous functions.
{% endhint %}

#### Then

The “then” event is triggered when the haystack has been completed successfully.

```php
$haystack = Haystack::build()
   ->addJob(new RecordPodcast)
   ->then(function () {
       // Do something... 
   })
   ->dispatch();
```

#### Catch

The “catch” event is triggered when the haystack has failed. You will still have the failed job to see what the error was, but this is useful if you need to perform any cleanup.

```php
$haystack = Haystack::build()
   ->addJob(new RecordPodcast)
   ->catch(function () {
       // Do something... 
   })
   ->dispatch();
```

#### Finally

The “finally “event is always triggered at the end of a haystack on both success and failure.&#x20;

```php
$haystack = Haystack::build()
   ->addJob(new RecordPodcast)
   ->finally(function () {
       // Do something... 
   })
   ->dispatch();
```

#### Paused

The "paused" event is triggered if the Haystack has been paused using the `pauseHaystack` method or a job has been released using the `longRelease` method. This is useful if you need to update the database to mark an import as paused, especially if the pause is for a long time.

```php
$haystack = Haystack::build()
   ->addJob(new RecordPodcast)
   ->paused(function () {
       // Do something... 
   })
   ->dispatch();
```

#### Invokable classes

Each of these methods supports invokable classes. If you use invokable classes you will have access to the **$this** context.&#x20;

```php
$haystack = Haystack::build()
   ->addJob(new RecordPodcast)
   ->then(new Then)
   ->catch(new Catch)
   ->finally(new Finally)
   ->paused(new Paused)
   ->dispatch();

// Example Invokable class

class Then {
   public function __invoke()
   {
       // Do something...
   }
}
```

#### Chained Methods

Each of the callback events can be chained for multiple events.

```php
$haystack = Haystack::build()
   ->addJob(new RecordPodcast)
   ->then(function () {
       // Do something first...
   })
   ->then(function () {
       // Then do something after! 
   })
   ->dispatch();
```
