# Connection, Queue & Delay

You can configure a global delay, connection and queue that will apply to all jobs in the haystack. You can also provide a per-job configuration if you choose to too.

#### Delay

You can use the `withDelay` method to apply a global delay to every job.

```php
$haystack = Haystack::build()
   ->withDelay(60)
   ->addJob(new RecordPodcast) 
   ->addJob(new ProcessPodcast)
   ->dispatch();
```

#### Connection

You can use the `onConnection` method to use a given connection for every job.

```php
$haystack = Haystack::build()
   ->onConnection('redis')
   ->addJob(new RecordPodcast) 
   ->addJob(new ProcessPodcast)
   ->dispatch();
```

#### Queue

You can use the `onQueue` method to use a given queue for every job.

```php
$haystack = Haystack::build()
   ->onQueue('podcasts')
   ->addJob(new RecordPodcast) 
   ->addJob(new ProcessPodcast)
   ->dispatch();
```

#### Custom Delay, Connection, Queue Per Job

You can also choose to use a different delay, connection or queue for every job!

```php
$haystack = Haystack::build()
   ->onQueue('podcasts')
   ->addJob(new RecordPodcast, delay: 60, queue: 'low', connection: 'redis') 
   ->addJob(new ProcessPodcast, delay: 120, queue: 'high', connection: 'sqs')
   ->dispatch();
```

> If you have already configured the job with delay, connection or queue, it will use that configuration.
