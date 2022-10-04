---
description: ⚡️ Supercharged job chains for Laravel
---

# Introduction

Laravel Haystack allows you to have a job chain stored in the database. It comes with powerful features like delaying jobs for as long as you like, applying middleware to every job, sharing data and models between jobs and even chunking jobs.  Laravel Haystack supports every queue connection/worker out of the box. (Database, Redis/Horizon, SQS).

```php
$haystack = Haystack::build()
   ->addJob(new RecordPodcast)
   ->addJob(new ProcessPodcast)
   ->addJob(new PublishPodcast)
   ->then(function () {
      // Haystack completed
   })
   ->catch(function () {
      // Haystack failed
   })
   ->finally(function () {
      // Always run either on success or fail.
   })
   ->withMiddleware([
      // Middleware for every job
   ])
   ->withDelay(60)
   ->withModel($user)
   ->dispatch();
```

#### But doesn't Laravel already have job chains?

That's right! Laravel does have job chains but they have some disadvantages that you might want to think about.

* They consume quite a lot of memory/data since the chain is stored inside the job. This is especially true if you are storing thousands of jobs.
* They are volatile, meaning if you lose one job in the chain - you lose the whole chain.
* They do not provide the `then`, `catch`, `finally` callable methods that batched jobs do.
* Long delays with memory-based or SQS queue is not possible as you could lose the jobs due to expiry or if the server shuts down.
* You can't share data between jobs as there is no "state" across the chain

Laravel Haystack aims to solve this by storing the job chain in the database and queuing one job at a time. When the job is completed, Laravel Haystack listens out for the "job completed" event and queues the next job in the chain from the database.

#### Laravel Haystack Features

* Low memory consumption as one job is processed at a time and the chain is stored in the database
* You can delay/release jobs for as long as you want since it will use the scheduler to restart a chain. Even if your queue driver is SQS!
* It provides callback methods like `then`, `catch` and `finally`.
* Global middleware that can be applied to every single job in the chain
* You can store models and data that are shared with every job in the chain.
* You can prepare a Haystack and dispatch it at a later time

#### Use Cases

* If you need to make hundreds or thousands of API calls in a row, can be combined with Spatie's Job Rate Limiter to keep track of delays and pause jobs when a rate limit is hit.
* If you need to queue thousands of jobs in a chain at a time.
* If you need to batch import rows of data - each row can be a haystack job (bale) and processed one at a time. While keeping important job information stored in the database.
* If you need "release" times longer than 15 minutes if you are using Amazon SQS
