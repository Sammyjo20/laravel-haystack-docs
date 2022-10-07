# Global Middleware

You can also provide middleware that will be applied to every job in the haystack. It will accept either an array, a closure that returns an array or an invokable class that returns an array.&#x20;

This is useful if you don’t want to manually add the middleware to every job, or if the middleware cannot belong to the job on its own. One example of this being useful is if you want to apply an API rate limiter to your requests.&#x20;

To add middleware to every job, use the `addMiddleware` method when building the haystack.

If you are unfamiliar with job middleware, [click here](https://laravel.com/docs/queues#job-middleware).

#### Array

```php
$haystack = Haystack::build()
   ->onQueue('podcasts')
   ->addJob(new RecordPodcast) 
   ->addJob(new ProcessPodcast)
   ->addMiddleware([
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
   ->addMiddleware(function () {
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
   ->addMiddleware(new PodcastMiddleware)
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

#### Chainable Middleware

You can also chain multiple middlewares together.

```php
$haystack = Haystack::build()
   ->onQueue('podcasts')
   ->addJob(new RecordPodcast) 
   ->addJob(new ProcessPodcast)
   ->addMiddleware([
       (new RateLimited)->allows(30)->everyMinute(),
       new OtherMiddleware,
   ])
   ->addMiddleware([
       new AnotherMiddleware,
   ])
   ->dispatch();
```
