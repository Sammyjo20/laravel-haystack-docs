# Long Delays & Pauses

Haystack fully supports the existing `release` and `delay` methods in Laravel, however occasionally you may want to pause the haystack for an extended period of time, or release a job until the next day when an API rate limit is lifted. This can also be used as a longer delay if you are using Amazon SQS which only has a delay of 15 minutes.

Laravel Haystack can provide this by storing the resume date in the database and using the Scheduler to dispatch the haystack when it is ready. When Laravel Haystack is paused, no job is left in your queue.

#### Setting Up

Before you configure long releases or pauses, you must make sure your scheduler is running and the following line is added to your Console/Kernel.php file. If you can, provide the `onOneServer` method as well which will prevent any accidental duplicate resumes.

```php
<?php

// Add to Console/Kernel.php

$schedule->command('haystacks:resume')->everyMinute();

// One server if you are running the scheduler on multiple servers

$schedule->command('haystacks:resume')->everyMinute()->onOneServer();
```

#### Long Releasing

If you would like to release the current job back onto the queue, just use the `longRelease` method inside your job’s handle method. You can provide an integer for seconds or a Carbon datetime instance.

```php
<?php
 
namespace App\Jobs;
 
use Sammyjo20\LaravelHaystack\Contracts\StackableJob;
use Sammyjo20\LaravelHaystack\Concerns\Stackable;
 
class ProcessPodcast implements ShouldQueue, StackableJob
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels, Stackable

    public function handle()
    {
        // Your application code...

        $this->longRelease(300); // Release for 5 minutes

        // Or use Carbon

        $this->longRelease(now()->addDays(2)); // Release for 2 days.
    }
```

#### Pausing the next job

If you want to process the current job but pause the Haystack for the next job, use the `pauseHaystack` method. If you have disabled automatic processing, you can provide a delay to the `nextJob` method.

```php
<?php
 
namespace App\Jobs;
 
use Sammyjo20\LaravelHaystack\Contracts\StackableJob;
use Sammyjo20\LaravelHaystack\Concerns\Stackable;
 
class ProcessPodcast implements ShouldQueue, StackableJo
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels, Stackable

    public function handle()
    {
        // Your application code...

        $this->pauseHaystack(now()->addHours(4)); // Pause the haystack for 4 hours.
    }
```
