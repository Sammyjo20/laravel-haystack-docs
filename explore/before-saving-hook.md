# Before Saving Hook

Sometimes you may wish to modify the Haystack model before it is saved. You can use the `beforeSave` method to modify the Haystack model instance.

<pre class="language-php"><code class="lang-php"><strong>&#x3C;?php
</strong><strong>
</strong><strong>Haystack::build()
</strong>    ->addJob(new RecordPodcast)
    ->beforeSave(function (Haystack $haystack) {
         $haystack->options->customOption = true;
     })
    ->dispatch();</code></pre>
