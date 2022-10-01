# Installation

You can install the package with Composer:

<pre class="language-bash"><code class="lang-bash"><strong>composer require sammyjo20/laravel-haystack</strong></code></pre>

> Requires Laravel 8+ and PHP 8.1

Then publish and run the migrations with:

```bash
php artisan vendor:publish --tag="haystack-migrations"
php artisan migrate
```

You can publish the config file with:

```bash
php artisan vendor:publish --tag="haystack-config"
```
