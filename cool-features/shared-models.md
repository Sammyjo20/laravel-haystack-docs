# Shared Models

You can also provide models when creating Haystacks which will be accessible to every job. This is extremely useful as you don't have to pass the model into every job. Just use the `withModel` method when building your Haystack to store a model.&#x20;

```php
<?php

$user = Auth::user();

Haystack::build()
    ->addJob(new RecordPodcast)
    ->withModel($user)
    ->dispatch();
```

You can also provide an optional `$key` as the second argument.&#x20;

```php
<?php

$user = Auth::user();

Haystack::build()
    ->addJob(new RecordPodcast)
    ->withModel($user, 'admin')
    ->dispatch();
```

Then, inside your jobs you can call the `getHaystackModel` method. If you did not provide a key, you can just pass in the model's class name.

```php
$user = $this->getHaystackModel(User::class);

// Or if you specified a key...

$this->getHaystackModel('admin');
```

{% hint style="info" %}
Laravel Haystack will serialize your models including their loaded relationships. It will only retrieve the model and the relationships when you attempt to get the model.
{% endhint %}
