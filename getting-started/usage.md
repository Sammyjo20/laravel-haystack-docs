# Usage

To prepare your jobs for Laravel Haystack, you must add the **StackableJob** interface and **Stackable** trait to your jobs. This provides the properties and methods Haystack requires.

```php
<?php
 
namespace App\Jobs;
 
use Sammyjo20\LaravelHaystack\Contracts\StackableJob;
use Sammyjo20\LaravelHaystack\Concerns\Stackable;
 
class ProcessPodcast implements ShouldQueue, StackableJob
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels, Stackable
```

Now let’s create our first Haystack. To get started, import the Haystack model and then call the “build” static function on it. This will provide you with the **HaystackBuilder** class which can be used to define your haystack. You can then use the “addJob” method to add jobs to the Haystack. Make sure all the jobs have the **StackableJob** interface and **Stackable** trait.

```php
<?php

use Sammyjo20\LaravelHaystack\Models\Haystack;

Haystack::build()
   ->addJob(new RecordPodcast)
   ->addJob(new PublishPodcast)
   ->addJob(new TweetAboutPodcast);
```

{% hint style="info" %}
You may also use the `addJobs` method which can accept an array or collection of jobs
{% endhint %}

#### Dispatching Haystacks

Once you have created your Haystack, you can dispatch it using the `dispatch` method.

<pre class="language-php"><code class="lang-php">&#x3C;?php

use Sammyjo20\LaravelHaystack\Models\Haystack;

$haystack = Haystack::build()
   ->addJob(new RecordPodcast)
   ->addJob(new PublishPodcast)
   ->addJob(new TweetAboutPodcast)
<strong>   ->dispatch();</strong></code></pre>

#### Creating Haystacks For Later

Use the create method to create the model and store it somewhere for later processing. You can start the Haystack at any time.

<pre class="language-php"><code class="lang-php">&#x3C;?php

use Sammyjo20\LaravelHaystack\Models\Haystack;

$haystack = Haystack::build()
   ->addJob(new RecordPodcast)
   ->addJob(new PublishPodcast)
   ->addJob(new TweetAboutPodcast)
   ->create();

// Do other things...

<strong>$haystack->start(); // Initiate haystack</strong></code></pre>

#### Cancelling Haystacks

If you need to cancel the Haystack during processing, you can do this by using the `cancel` method on the Haystack model. If a job is being processed when you cancel it, it will process the next job and then stop before the job is executed.

<pre class="language-php"><code class="lang-php">use Sammyjo20\LaravelHaystack\Models\Haystack;

$haystack = Haystack::build()
   ->addJob(new RecordPodcast)
   ->addJob(new PublishPodcast)
   ->addJob(new TweetAboutPodcast)
   ->create();

// Store Haystack

$haystack->start();

// Do other things...

<strong>$haystack->cancel();</strong></code></pre>

> When a haystack is cancelled, we will not run the `then` or `catch` closures, but we will execute the `finally` closure.
