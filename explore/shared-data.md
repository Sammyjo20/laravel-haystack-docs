# Shared Data

aravel Haystack has the ability for your jobs to store and retrieve state/data between jobs. This is really useful if you need to store data in the first job and then in the second job, process the data and in the final job, email the processed data. You are able to create a process pipeline since each job is processed in sequential order. This is really exciting because with traditional chained jobs, you cannot share data between jobs.

```php
<?php

$haystack = Haystack::build()
   ->addJob(new RetrieveDataFromApi)
   ->addJob(new ProcessDataFromApi)
   ->addJob(new StoreDataFromApi)
   ->dispatch();
```

#### Storing data inside of jobs

Inside your job, you can use the `setHaystackData()` method to store some data. This method accepts a key, value and optional [Eloquent cast](https://laravel.com/docs/eloquent-mutators).

```php
<?php
 
namespace App\Jobs;
 
use Sammyjo20\LaravelHaystack\Contracts\StackableJob;
use Sammyjo20\LaravelHaystack\Concerns\Stackable;
 
class RetrieveDataFromApi implements ShouldQueue, StackableJob
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels, Stackable

    public function handle()
    {
        // Your application code...
        
        $this->setHaystackData('username', 'Sammyjo20');
    }
```

#### Casting data

The `setHaystackData` method supports any data type. It supports fully casting your data into any of [Eloquent's existing casts](https://laravel.com/docs/eloquent-mutators), or even your custom casts. Just provide a third argument to specify the cast.

```php
<?php
 
namespace App\Jobs;
 
use Sammyjo20\LaravelHaystack\Contracts\StackableJob;
use Sammyjo20\LaravelHaystack\Concerns\Stackable;
 
class RetrieveDataFromApi implements ShouldQueue, StackableJob
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels, Stackable

    public function handle()
    {
        // Your application code...
        
        // Array Data, provide third argument to specify cast.
        
        $this->setHaystackData('data', ['username' => 'Sammyjo20'], 'array');
        
        // Carbon dates...
        
        $this->setHaystackData('currentDate', now(), 'immutable_datetime');
        
        // Supports custom casts
        
        $this->setHaystackData('customData', $object, CustomCast::class);
    }
```

#### Retrieving data inside of jobs

From one job you can set the data, but that data will be available to every job in the haystack there after. Just use the `getHaystackData` method to get data by key or use the `allHaystackData` to get a collection containing the haystack data.

```php
<?php
 
namespace App\Jobs;
 
use Sammyjo20\LaravelHaystack\Contracts\StackableJob;
use Sammyjo20\LaravelHaystack\Concerns\Stackable;
 
class RetrieveDataFromApi implements ShouldQueue, StackableJob
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels, Stackable

    public function handle()
    {
        // Get data by key
        
        $username = $this->getHaystackData('username'); // Sammyjo20
        
        // Get all data
        
        $allData = $this->allHaystackData(); // Collection: ['username' => 'Sammyjo20']
    }
```

#### Getting The Data After Processing

Laravel Haystack will conveniently pass a collection into your then/catch/finally closures with all the of the data that you stored inside the Haystack. You can then use this data however you wish.

```php
<?php

$haystack = Haystack::build()
   ->addJob(new RetrieveDataFromApi)
   ->addJob(new ProcessDataFromApi)
   ->addJob(new StoreDataFromApi)
   ->then(function ($data) {
        // $data: ['username' => 'Sammyjo20', 'other-key' => 'value', 'collection' => new Collection],
   })
   ->catch(function ($data) {
        // $data: ['username' => 'Sammyjo20', 'other-key' => 'value', 'collection' => new Collection],
   })
   ->finally(function ($data) {
        // $data: ['username' => 'Sammyjo20', 'other-key' => 'value', 'collection' => new Collection],
   })
   ->dispatch();
```

If you would like to disable this functionality, you can provide the `dontReturnData` method to the Haystack builder. If this method is provided, Haystack won't run the query that retrieves all the data at the end of a Haystack.

```php
<?php

$haystack = Haystack::build()
   ->addJob(new RetrieveDataFromApi)
   ->addJob(new ProcessDataFromApi)
   ->addJob(new StoreDataFromApi)
   ->dontReturnData()
   ->then(function ($data) {
        // $data: null
   })
   ->dispatch();
```

If you would like to disable this feature entirely, you can set the `return_all_haystack_data_when_finished` config variable to false.

### Setting data when creating haystacks

Sometimes it's useful to set some data for your jobs to consume without having to pass every piece of data down into each job. You can use the `withData` method while you are building your Haystack to add data before the Haystack starts. It also accepts a key, value and optional Eloquent cast.

```php
<?php

$haystack = Haystack::build()
   ->addJob(new RetrieveDataFromApi)
   ->addJob(new ProcessDataFromApi)
   ->addJob(new StoreDataFromApi)
   ->withData('username', 'Sammyjo20')
   ->dispatch();
```
