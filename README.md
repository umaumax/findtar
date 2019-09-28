# findtar

* `find` + `tar`
* You can tar files filtered by find command.
  * use case
    * cache build outputs for `make` command

## how to use
```
# dry-run
$ findtar dst.tar.bz2 -- $src_dir -name '*.txt'
$ findtar $dst_dir -- $src_dir -name '*.txt'

# run command
$ findtar -f dst.tar.bz2 -- $src_dir -name '*.txt'
$ mkdir $dst_dir
$ findtar -f $dst_dir -- $src_dir -name '*.txt'
```

```
src
├── fate.txt
├── hayate.txt
└── nanoha.txt

0 directories, 3 files

dst
└── src
    ├── fate.txt
    ├── hayate.txt
    └── nanoha.txt

1 directory, 3 files
```

## when to use
* cache build outputs for `make` command
```
# tar
findtar repo.tar.gz -- . -type f \( -name "*.a" -o -name "*.so" -o -name "*.o" \)
findtar repo.tar.gz -- . -type f \( -name "*.a" -o -name "*.so" -o -name "*.o" \)

# untar
tar xvf repo_arm.tar.gz -C .
tar xvf repo_x86_64.tar.gz -C .
```

## FYI
* [findの結果をtarでアーカイブしたい \- 浦安市在住＋デジカメ]( https://fei-yen.jp/maya/wordpress/blog/2013/01/15/find%E3%81%AE%E7%B5%90%E6%9E%9C%E3%82%92tar%E3%81%A7%E3%82%A2%E3%83%BC%E3%82%AB%E3%82%A4%E3%83%96%E3%81%97%E3%81%9F%E3%81%84/ )
