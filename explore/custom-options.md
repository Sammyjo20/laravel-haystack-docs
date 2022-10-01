# Custom Options

Every Haystack has a `HaystackOptions` class stored against it. This class is serialized and stored in the database and contains various configuration variables used when processing jobs. You may wish to specify a new custom option by using the `setOption` method when building Haystacks.

```php
<?php

Haystack::build()
    ->addJob(new RecordPodcast)
    ->setOption('someCustomOption', 'option-value')
    ->dispatch();
```

{% hint style="info" %}
Since the options are serialized into one column, it's recommended that you only store small amounts of data inside this class, and make sure what you are storing can be serialized.
{% endhint %}

Inside your jobs, you can use the `getHaystackOptions` or `getHaystackOption` method to retrieve the options.

```php
<?php

$this->getHaystackOptions(); // HaystackOptions.php

$this->getHaystackOption('someCustomOption'); // "option-value"
```
