# Chunking Jobs

Sometimes it's useful to split up the processing of something into multiple jobs, like processing a large file or scraping data from a paginated API. Laravel haystack uses the Laravel Chunkable Job package and allows you to split a job up into chunks. Just create a job, remove the `handle` method and extend the `ChunkableHaystackJob` class. It's important that you extend this class and not `ChunkableJob` as it will use Haystack's methods to keep processing on the same Haystack.

You do not need to add the `StackableJob` interface or `Stackable` trait since the `ChunkableHaystackJob` will already add this for you.

To read more about the chunkable jobs documentation [click here](https://github.com/sammyjo20/laravel-chunkable-jobs).

```php
<?php

use Sammyjo20\ChunkableJobs\Chunk;
use Sammyjo20\LaravelHaystack\ChunkableHaystackJob;

class GetPageOfPokemon extends ChunkableHaystackJob implements ShouldQueue
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels;

    public function defineChunk(): ?Chunk
    {
        $response = Http::asJson()->get('https://pokeapi.co/api/v2/pokemon');

    	$count = $response->json('count'); // 1154

    	return new Chunk(totalItems: $count, chunkSize: 1, startingPosition: 1);
    }

    protected function handleChunk(Chunk $chunk): void
    {
        $response = Http::asJson()->get(sprintf('https://pokeapi.co/api/v2/pokemon?limit=%s&offset=%s', $chunk->limit, $chunk->offset));

    	$data = $response->json();

    	// Store data of response
    }
}
```
