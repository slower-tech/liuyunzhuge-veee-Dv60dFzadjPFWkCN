
在 Linux 系统中，内存管理通常由系统自动处理，但在某些情况下，手动释放内存可能是必要的。例如，当业务应用比较繁忙时会频繁存取文件，物理内存会被缓存大量占用，有时会出现内存不足的情况发生，甚至会导致系统性能下降。此时可主动在业务闲时手动释放内存。


## 一、首先查看当前内存使用情况


使用 free \-m 命令查看，输出结果包括总内存、已使用内存、空闲内存、共享内存、缓冲区和缓存等信息。


## 二、然后执行如下步骤手动释放内存


### ■ 查看当前 drop\_caches 的值


cat /proc/sys/vm/drop\_caches
可能会提示权限不足，默认值为 0，表示不释放缓存


### ■ 运行 sync 命令


sync
该命令将所有未写的系统缓冲区写到磁盘中，确保文件系统的完整性


### ■ 手动释放内存


echo 1 \> /proc/sys/vm/drop\_caches
drop\_caches 是 0\-3 之间的数字，代表不同的含义：
0：不释放（系统默认值）
1：释放页缓存
2：释放 dentries 和 inodes
3：释放所有缓存


### ■ 还原配置


echo 0 \> /proc/sys/vm/drop\_caches
释放完内存后，将 drop\_caches 的值改回 0，让系统重新自动分配内存


## 三、注意事项


### 缓存机制


Linux 的缓存机制非常先进，通常不需要手动释放内存。缓存包括 dentry（用于加速文件路径名到 inode 的转换）、Buffer Cache（针对磁盘块的读写）和 Page Cache（针对文件 inode 的读写）


### 性能影响


频繁手动释放内存可能会影响系统性能，建议在内存不足、应用获取不到可用内存或发生 OOM 错误时，首先分析应用方面的原因


 本博客参考[FlowerCloud机场订阅官网](https://hanlianfangzhi.com)。转载请注明出处！
