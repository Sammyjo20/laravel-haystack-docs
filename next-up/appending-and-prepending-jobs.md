# Appending & Prepending Jobs

You can append to the haystack inside a job. The appended job will go at the end of the chain. Just use the `appendToHaystack` method. If you would like to append a job to the haystack to be processed immediately, use the `prependToHaystack` method.

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
        // Append a job to the end of the haystack

        $this->appendToHaystack(new DifferentJob);
        
        // Append a job and put it right at the top of the haystack
        
        $this->prependToHaystack(new NextJob);
    }
```

> The `appendToHaystack` and `prependToHaystack` methods also accept an array or Collection of jobs.
