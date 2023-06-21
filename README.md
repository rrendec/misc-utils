misc-utils
==========

Various shell utilities. See detailed description for each utility below.

# dupfind

dupfind is a very simple Bash script that finds duplicate files.

The script does not compare the file contents byte for byte. Instead, it relies
on the `md5sum` utility and assumes the contents is identical if the md5 sum
matches.  This improves performance significantly when there are multiple files
with the same size.

The algorithm uses three stages and eliminates non-duplicates at each stage:
1. A list of all files and their corresponding size is generated. Files with
   different size are definitely different, so they are eliminated.
2. The md5 sum of the first 256 KiB of each file is calculated. Files with
   different sum are eliminated. This is an optimization in case there are many
   files with the same size.
3. The full md5 sum of each file is calculated. Files with different sum are
   eliminated.

The repository includes a sample file tree with duplicates in the `test`
directory. Example:
```
[rrendec@bat dupfind]$ ./dupfind test/
71799ff1c822dcbaeadca8e2597a2fd9 74 test/sample D  copy 1
71799ff1c822dcbaeadca8e2597a2fd9 74 test/foo/sample-D-copy2
--
fa62f78b99b3c9c940ea730d5e4dd5e3 37 test/sample-A-copy1
fa62f78b99b3c9c940ea730d5e4dd5e3 37 test/foo/sample A copy 2
fa62f78b99b3c9c940ea730d5e4dd5e3 37 test/foo/bar/sample-A-copy3
```

# rpmorphans

rpmorphans finds the files that are not owned by any RPM package.
