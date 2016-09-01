## dd
### Write
You can use dd to create a large file as quickly as possible to see how long it takes. It’s a very basic test and not very customisable however it will give you a sense of the performance of the file system. You must make sure this file is larger than the amount of RAM you have on your system to avoid the whole file being cached in memory.

It’s usually installed out-of-the-box with most Linux file systems which makes it an ideal tool in locked-down environments or environments where it’s tricky to get packages installed onto. Use the below command substituting [PATH] with the filesystem path to test, [BLOCK_SIZE] with the block size and [LOOPS] for the amount of blocks to write.


```
time sh -c "dd if=/dev/zero of=[PATH] bs=[BLOCK_SIZE]k count=[LOOPS] && sync"
```
A break down of the command is as follows:

time – times the overall process from start to finish
of= this is the path which you would like to test. The path must be read/ writable.
bs= is the block size to use. If you have a specific load which you are testing for, make this value mirror the write size which you would expect.
sync – forces the process to write the entire file to disk before completing. Note, that dd will return before completing but the time command will not, therefore the time output will include the sync to disk.
The below example uses a 4K block size and loops 2000000 times. The resulting write size will be around 7.6GB.

```
time sh -c "dd if=/dev/zero of=/mnt/mount1/test.tmp bs=4k count=2000000 && sync"
2000000+0 records in
2000000+0 records out
8192000000 bytes transferred in 159.062003 secs (51501929 bytes/sec)

real 2m41.618s
user 0m0.630s
sys 0m14.998s
```

Now, let’s do the math. dd tells us how many bytes were written, and the time command tells us how long it took – use the real output at the bottom of the output. Use the formula BYTES / SECONDS. For these larger tests, convert bytes to KB or MB to make more sensible numbers.

(8192000000 / 1024 / 1024) / ((2 * 60) + 41.618)
Bytes converted to MB / (2 minutes + 41.618 seconds)

This gives us an average of 48.34 megabytes per second over the duration of the test.

Read
We can also use dd to test the read speed of a disk by reading the file we created and timing the process. Before we do that, we need to flush the file cache by writing another file which is about the size of the RAM installed on the test system. If we don’t do this, the file we just created will be partially in RAM and therefore the read test will not be completely read from disk.

Create a file using dd which is about the same size as the RAM installed on the system. The below assumes 2GB of RAM is installed. You can check how much RAM is installed with free.

```
dd if=/dev/zero of=/mnt/mount1/clearcache.tmp bs=4k count=524288
```
Now for the read test of our original file.

```
time sh -c "dd if=/mnt/mount1/test.tmp of=/dev/null bs=4k"
```
And process the time result the same was as when writing.
