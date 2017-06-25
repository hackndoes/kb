# Clear RAM used Cache
## Free pagecache, dentries and inodes in cache memory
```sh
sync; echo 3 > /proc/sys/vm/drop_caches
```

## Free dentries and inodes use following command
```sh
sync; echo 2 > /proc/sys/vm/drop_caches
```

## Free pagecache only use following command
```sh
sync; echo 1 > /proc/sys/vm/drop_caches
```

### Add cron job to run hourly
```sh
0 * * *  * sync; echo 3 > /proc/sys/vm/drop_caches
```