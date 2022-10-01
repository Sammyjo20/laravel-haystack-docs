# Callback Events

Laravel Haystack provides you with three really simple callback events that will be serialized and used at certain points in time.

#### Then

The “then” event is triggered when the haystack has completed successfully.

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

The “finally “event is always triggered at the end of a haystack, if it was successful or if it failed. This is very useful if you want to have loading states in your application.

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

Each of these methods support invokable classes.

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
