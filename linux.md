# List functions from .so files

```
$ nm -D /path/lib.so
```

# Dentry cache

```
# host
$ cat /proc/sys/fs/dentry-state

# cgroup
$ grep dentry /sys/fs/cgroup/memory/memory.kmem.slabinfo
