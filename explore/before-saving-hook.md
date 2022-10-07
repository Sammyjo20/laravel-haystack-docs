# Before Saving Hook

Sometimes you may wish to modify the Haystack model before it is saved. You can use the `beforeSave` method to modify the Haystack model instance.

```php
<?php

Haystack::build()
    ->addJob(new RecordPodcast)
    ->beforeSave(function (Haystack $haystack) {
         $haystack->options->customOption = true;
     })
    ->dispatch();
```
