# Deleting Specific Haystacks

If a haystack was created by mistake, and you would like to delete it without directly accessing the database, you may use the `haystacks:forget` command. The `haystacks:forget` command accepts the ID of the haystack as its only argument:

```
php artisan haystacks:forget <id>
```
