# Tar and Compression

## Setup

### Create Files

```
touch testfile{1..9}.txt
mkdir testdir{1..3}
for i in {1..3}; do touch testdir$i/example"$i"file{1..9}.txt; done
ls -R
```

![Creating some test files and directories](./screenshots/create_test_files.png)

### Fill Some Files With Data

```
ls
for i in {1..10000}; do echo "repetitive data: blah blah blah" >> testfile1.txt; done
head -n 5 testfile1.txt
du -h testfile1.txt
dd if=/dev/urandom of=testfile2.txt bs=1M count=1024 status=progress
du -h testfile2.txt
dd if=/dev/zero of=testfile3.txt bs=1M count=1024 status=progress
du -h testfile2.txt
```

![Filling some test files with different types of data](./screenshots/fill_test_files.png)

## gzip

### Basic Usage Of gzip

#### Compression

```
ls -l testfile1.txt*
du -h testfile1.txt*
gzip testfile1.txt
ls -l testfile1.txt*
du -h testfile1.txt*
```

![Compressing testfile1.txt with no options](./screenshots/basic_gzip_compress_test1.png)

#### Decompress

```
ls
gzip -d testfile1.txt.gz
ls
gzip testfile1.txt
ls -l testfile1.txt*
gunzip testfile1.txt.gz
ls -l testfile1.txt*
ls
```

![Decompressing a gzip using -d and gunzip](./screenshots/basic_gzip_decompress_test1.png)

### Keeping original file with gzip

```
ls
gzip -k testfile1.txt
ls
gzip -d testfile.txt.gz
n
ls
gzip -d testfile.txt.gz
y
ls
```

![Keeping original file when gzipping a file](./screenshots/gzip_keep_original_test1.png)

### Verbose and more info on compression gzip

```
ls
du -h testfile{1,2,3}.txt
gzip -v testfile1.txt
gzip -v testfile2.txt testfile3.txt
gzip -l testfile{1,2,3}.txt.gz
du -h testfile{1,2,3}.txt.gz
ls
gunzip -v testfile{1,2,3}.txt.gz
ls
```

![List compressed files and compress using verbosity](./screenshots/gzip_verbose_list.png)

### Recursively compress files

```
ls testdir1
gzip -rv testdir
ls testdir1
gzip -drv testdir1
ls testdir1
```

![Recursively compress a directory of files](./screenshots/gzip_recurse_dir1.png)

### Fast vs Best Compression

```
ls
du -h testfile{1,2,3}.txt
gzip -v --best test{1,2,3}.txt
du -h testfile{1,2,3}.txt.gz
gunzip testfile{1,2,3}.txt.gz
gzip -v --fast testfile{1,2,3}.txt
du -h testfile{1,2,3}.txt.gz
gunzip testfile{1,2,3}.txt.gz
ls
```

![Comparing best vs fast compression with gzip](./screenshots/gzip_best_vs_fast.png)

## xz

xz is quite powerful with lots of nuanced options, we will just cover the basics

### No option xz compressions and decompression

```
ls
du -h testfile{1,3}.txt
xz testfile1.txt
xz -z testfile3.txt
du -h testfile{1,3}.txt.xz
ls
unxz testfile1.txt.xz
xz -d testfile3.txt.xz
ls
```

![Basics of compression and decompression using xz](./screenshots/basics_xz_test13.png)

### xz with basic options

```
ls
xz -kv testfile{1..3}.txt
du -h testfile{1..3}.txt.xz
ls
rm testfile{1..3}.txt
ls
unxz -k testfile{1..3}.txt.xz
ls
rm testfile{1..3}.txt.xz
``` 

![Keeping files with xz compression/decompression](./screenshots/keep_verbose_xz_test123.png)

## bzip2

### No options bzip2 compression and decompression

```
ls
du -h testfile{1,3}.txt
bzip2 testfile1.txt
bzip2 -z testfile3.txt
ls
du -h testfile{1,3}.txt.bz2
bunzip2 testfile1.txt.bz2
bzip2 -d testfile3.txt.bz2
ls
```

![Compression and decompression with bzip2](./screenshots/basic_bzip2_test13.png)

### bzip2 with basic options

```
ls
bzip2 -kv testfile{1..3}.txt
du -h testfile{1..3}.txt.bz2
ls
rm testfile{1..3}.txt
ls
bunzip2 -k testfile{1..3}.txt.bz2
ls
rm testfile{1..3}.txt.bz2
``` 

![Keeping files with bzip2 compression and decompression](./screenshots/keep_verbose_bzip2_test123.png)

## tar

Note: tar is a large program with lots of options and considerations. We are only going to cover the basics here.

### Create a basic tar archive

```
ls
ls testdir1
tar -cvf testdir1.tar testdir1
ls
```

![Creating a basic tar archive](./screenshots/create_basic_tar_archive.png)

### Compare a archive with changes to the original files

```
ls
tar -d -f testdir1.tar testdir1/
echo "test test" >> testdir1/example1file1.txt
tar -d -f testdir1.tar testdir1/
touch testdir1/newfile
tar -d -f testdir1.tar testdir1/
ls testdir1/
tar -d -f testdir1.tar testdir1/
rm testdir1/example1file2.txt
tar -d -f testdir1.tar testdir1/
```

![Comparing archive with files](./screenshots/diff_tar_testdir1.png)


### Extract archive 

```
ls 
rm -r testdir1
ls
tar -xvf testdir1.tar
ls
``` 

![Extracting a basic tar archive](./screenshots/extract_basic_tar_archive.png)

### List the contents of an archive

```
ls
tar -tf testdir1.tar
```

![displaying the contents of an archive](./screenshots/list_tar_archive.png)

### Append files to an archive

```
ls
mv testdir1.tar testarchive.tar
ls
tar -r -f testarchive.tar testdir2/ testfile1.txt
tar -tf testarchive.tar
```

![Appending files to an archive](./screenshots/append_files_tar.png)

### Concatenate archives

```
ls
tar -cf testdir3.tar testdir3/
ls
tar -Af testarchive.tar testdir3.tar
ls
tar -tf testarchive.tar | tail
tar -tf testarchive.tar | head -n 5
```

![Concatenating archives](./screenshots/concatenate_archives_tar.png)

### Decompression chamber

When opening a tar archive it is best practice to open it in a new directory just in case the archive happens to be a tar bomb (.i.e. what we just made, kinda).

```
ls
mkdir decompression_chamber
mv testarchive.tar decompression_chamber/
cd decompression_chamber
tar -xf testarchive.tar
ls
``` 

![Best practice for decompression](./screenshots/tar_bomb_chamber.png)

### tar with compression

```
ls
tar -czf testdir1.tar.gz testdir1
tar -cjf testdir2.tar.bz2 testdir2
tar -cJf testdir3.tar.xz testdir3
ls
tar -tf testdir1.tar.gz
```

![Making and compressing archives with tar](./screenshots/tar_with_compression.png)

### tar and decompression

```
ls
rm -r testdir?
ls
tar -xzf testdir1.tar.gz
tar -xjf testdir2.tar.bz2
tar -xJf testdir3.tar.xz
ls
rm -r testdir1
ls
gunzip testdir1.tar.gz
tar -xf testdir1.tar
ls
```

![Decompressing and extracting tar archives](./screenshots/tar_and decompression.png)


## zip command

```
ls
zip archive.zip testdir* testfile*
rm -r test*
unzip archive.zip
ls
```

![Zipping up and unzipping files](./screenshots/zip_and_unzip.png)


