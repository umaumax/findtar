# findtar

* `find` + `tar`

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

## FYI
* [findの結果をtarでアーカイブしたい \- 浦安市在住＋デジカメ]( https://fei-yen.jp/maya/wordpress/blog/2013/01/15/find%E3%81%AE%E7%B5%90%E6%9E%9C%E3%82%92tar%E3%81%A7%E3%82%A2%E3%83%BC%E3%82%AB%E3%82%A4%E3%83%96%E3%81%97%E3%81%9F%E3%81%84/ )
