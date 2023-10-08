# Configuration

Laravel Haystack provides various configuration options, altering default package behaviour that you may find useful for your application. These can be changed in the `config/haystack.php` file.

## Return All Haystack Data When Finished

When this option is set to `true`, Laravel Haystack will query all the haystack data rows from the database and include them in the `then`/`finally`/`catch` callbacks as a Laravel collection upon completion of the job processing:

```php
// config/haystack.php

'return_all_haystack_data_when_finished' => true,
```

```php
use Illuminate\Support\Collection;

$haystack = Haystack::build()
   ->addJob(new RecordPodcast)
   ->then(function (Collection $data) {
       // ...
   })
   ->catch(function (Collection $data) {
       // ...
   })
   ->finally(function (Collection $data) {
       // ...
   })
   ->dispatch();
```

## Process Automatically

This configuration determines whether Laravel Haystack should automatically queue `Stackable` jobs after each job is processed. If set to `false`, you will need to manually call `$this->nextJob` inside your jobs:

```php
// config/haystack.php

'process_automatically' => false,
```

```php
<?php
 
namespace App\Jobs;
 
use Sammyjo20\LaravelHaystack\Contracts\StackableJob;
use Sammyjo20\LaravelHaystack\Concerns\Stackable;
 
class ProcessPodcast implements ShouldQueue, StackableJob
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels, Stackable;
    
    public function handle()
    {
        // ...
        
        // Call the next job in the haystack:
        $this->nextJob();
        
        // Or, with a delay in seconds:
        $this->nextJob($seconds = 15);
        
        // Or, with a delay using a Carbon instance:
        $this->nextJob(now()->addDay());
    }
}
```

## Keep Stale Haystacks for Days

Defines the duration (in days) for which "stale" haystacks are retained. Stale haystacks are those where the controlling job has failed without sending the failure signal to laravel-haystack.

```php
// config/haystack.php

'keep_stale_haystacks_for_days' => 3,
```

## Delete Finished Haystacks

Determines whether Laravel Haystack should automatically delete haystacks after they have finished processing. If set to `false`, ensure to use the scheduled command to clean up old finished haystacks.

```php
// config/haystack.php

'delete_finished_haystacks' => true,
```

## Keep Finished Haystacks for Days

Specifies the duration (in days) for which finished haystacks will be retained. This is only applicable if `delete_finished_haystacks` is set to `false`.

```php
// config/haystack.php

'keep_finished_haystacks_for_days' => 1,
```

## Database Connection

Specifies the database connection used to store haystack jobs. The default value is retrieved from the `HAYSTACK_DB_CONNECTION` environment variable, falling back to the default database connection specified in your Laravel configuration (`DB_CONNECTION`).

```php
// config/haystack.php

'db_connection' => env(
    'HAYSTACK_DB_CONNECTION',
    env('DB_CONNECTION', 'mysql')
),
```
