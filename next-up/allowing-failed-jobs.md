# Allowing Failed Jobs

The default behaviour of Haystack is to cancel the Haystack if one of the jobs in the chain fails. If you would like to continue processing the rest of the Haystack even if there was a failure, use the `allowFailures` option when building a Haystack.

```php
<?php

$haystack = Haystack::build()
   ->addJob(new RecordPodcast) 
   ->addJob(new ProcessPodcast)
   ->allowFailures()
   ->dispatch();
```

{% hint style="info" %}
When jobs fail they will be added to your `failed_jobs` database table.
{% endhint %}
