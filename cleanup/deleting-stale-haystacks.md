# Deleting Stale Haystacks

Laravel Haystack will attempt to clean up every job on successful/failed haystacks, however there may be a situation, especially if you have not enabled the automatic processing. If you have disabled the option to automatically delete haystacks when they are finished they may build up quickly.

You can prevent this by running the following prune command every day in your scheduler:

```php
<?php

// Add to Console/Kernel.php

use Sammyjo20\LaravelHaystack\Models\Haystack;

$schedule->command('model:prune', [
    '--model' => [Haystack::class],
])->daily();
```
