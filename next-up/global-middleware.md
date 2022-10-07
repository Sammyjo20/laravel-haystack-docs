# Global Middleware

You can also provide middleware that will be applied to every job in the haystack. This is perfect if you don’t want to manually add the middleware to every job, or if the middleware can’t belong to the job on its own. This is extremely useful if you want to apply an API rate limiter to your requests. Just use the `withMiddleware` method. It will accept either an array, a closure that returns an array or an invokable class that returns an array.

#### Array

```php
$haystack = Haystack::build()
   ->onQueue('podcasts')
   ->addJob(new RecordPodcast) 
   ->addJob(new ProcessPodcast)
   ->withMiddleware([
       (new RateLimited)->allows(30)->everyMinute(),
       new OtherMiddleware,
   ])
   ->dispatch();
```

#### Closure

You must return an array in the closure for it to work.

```php
$haystack = Haystack::build()
   ->onQueue('podcasts')
   ->addJob(new RecordPodcast) 
   ->addJob(new ProcessPodcast)
   ->withMiddleware(function () {
        return [
           (new RateLimited)->allows(30)->everyMinute(),
           new OtherMiddleware,
        ];
   })
   ->dispatch();
```

#### Invokable class

You must return an array inside your invokable class for it to work.

```php
$haystack = Haystack::build()
   ->onQueue('podcasts')
   ->addJob(new RecordPodcast) 
   ->addJob(new ProcessPodcast)
   ->withMiddleware(new PodcastMiddleware)
   ->dispatch();

// Invokable class...

class PodcastMiddleware {
    public function __invoke()
    {
		return [
	        (new RateLimited)->allows(30)->everyMinute(),
		    new OtherMiddleware,
		];
    }
}
```
