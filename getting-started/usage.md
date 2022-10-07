# Usage

Let's build our first Haystack. Start preparing your jobs by adding the **StackableJob** interface and **Stackable** trait. These will make them compatible with Haystack, however, you can still dispatch the jobs on their own with the trait and interface added.

```php
<?php
 
namespace App\Jobs;
 
use Sammyjo20\LaravelHaystack\Contracts\StackableJob;
use Sammyjo20\LaravelHaystack\Concerns\Stackable;
 
class ProcessPodcast implements ShouldQueue, StackableJob
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels, Stackable
```

Now import the Haystack model and then call the `build` static function on it. This will provide you with an instance of the **HaystackBuilder** class which can be used to build your haystack. You can then use the `addJob` method to add jobs to the Haystack.

```php
<?php

use Sammyjo20\LaravelHaystack\Models\Haystack;

Haystack::build()
   ->addJob(new RecordPodcast)
   ->addJob(new PublishPodcast)
   ->addJob(new TweetAboutPodcast);
```

{% hint style="info" %}
You may also use the `addJobs` method which can accept an array or collection of jobs or the`addJobsWhen` method to conditionally add a job to the haystack.
{% endhint %}

#### Dispatching Haystacks

Finally, use the `dispatch` method to dispatch the haystack onto the queue connection that you have specified in your application's config. [See here for dispatching on custom connections](../next-up/connection-queue-and-delay.md).

<pre class="language-php"><code class="lang-php">&#x3C;?php

use Sammyjo20\LaravelHaystack\Models\Haystack;

$haystack = Haystack::build()
   ->addJob(new RecordPodcast)
   ->addJob(new PublishPodcast)
   ->addJob(new TweetAboutPodcast)
<strong>   ->dispatch();</strong></code></pre>

#### Creating Haystacks For Later

Sometimes you may wish to perform other logic or wait before dispatching the Haystack. To accomplish this, use the `create` method instead which will create the Haystack model in the database but will not dispatch it. When you are ready to dispatch it, retrieve the model and invoke the `start` method.

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

{% hint style="info" %}
When a haystack is cancelled, we will not run the `then` or `catch` closures, but we will execute the `finally` closure.
{% endhint %}
