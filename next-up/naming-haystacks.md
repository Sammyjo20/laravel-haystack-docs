# Naming Haystacks

You may wish to give your haystack a custom name. This is especially useful for debugging, as you could check your database and see what haystacks are currently being processed. To give the haystack a name, use the `withName` method when building the haystack.

```php
<?php

$haystack = Haystack::build()
   ->withName('Process API Data')
   ->addJob(new RetrieveDataFromApi)
   ->addJob(new ProcessDataFromApi)
   ->addJob(new StoreDataFromApi)
```
