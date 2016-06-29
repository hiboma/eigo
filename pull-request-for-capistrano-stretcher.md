Hi! Here is a benchmark compared `gzip -9` with `gzip` (set :complression_level empty = default compression level).

#### gzip -9

```
# build directory contains 70770 files (1.1GB)

$ time tar -cf - --exclude tmp --exclude spec ./ | pv -s $( du -sb ./ | awk '{print $1}' ) | gzip -9 > /tmp/build-benchmark.tar.gz
 935MiB 0:01:39 [9.38MiB/s] [=====================================================================================================] 103%

real	1m39.807s
user	1m41.231s
sys	0m1.955s

$ ls -hal /tmp/build-benchmark.tar.gz
-rw-rw-r--. 1 app app 545M Jun 29 17:11 /tmp/build-benchmark.tar.gz
```

#### gzip (default)

```
$ time tar -cf - --exclude tmp --exclude spec ./ | pv -s $( du -sb ./ | awk '{print $1}' ) | gzip > /tmp/build-benchmark.tar.gz
 935MiB 0:00:32 [28.3MiB/s] [=====================================================================================================] 103%

real    0m28.866s
user    0m29.663s
sys 0m2.205s

$ ls -hal /tmp/build-benchmark.tar.gz
-rw-rw-r--. 1 app app 547M Jun 29 17:13 /tmp/build-benchmark.tar.gz
```

gzip -9 は gzip より tar.gz のサイズを 2MB を多く減らしました。でも、gzip -9 は gzip より多くの時間を費やします (60秒以上)

`gzip -9` reduced about 2MB more of tar.gz than `gzip`, but it took much more real/user time (~ 60sec).

私の考えでは、 gzip はだいたいのシチュエーションでは十分 スピード/サイズ の効率がよいと思います

I think `gzip` (default compression level) is effecient enough in speed and size in most situations.

どうしても tar.gz のサイズが重要なら、 :gzip_compress_level を `-9` に変更しておいたらよいと思います

If the size of tar.gz is the highest priority, you can set `:gzip_compress_level` to `-9` ( ... or any values).
