# Basic File Manipulation

## file command 

The file command runs tests/checks on a file to try to determine what type of file and what type of data it holds

### Basic Usage

#### Setup

```
touch test1
echo "This is an ascii text file" > test2
echo -e "#include <stdio.h>\n\nint main(){}\n" > test3
mkdir test4
ln test1 test5
ln -s test1 test6
mkfifo test7
```

![Creating different types of files](./screenshots/file_command_prep.png)

#### File Command Basic Usage

```
ls
file test1
file test*
file /dev/dm-0
shred test2
file test2
```

![Displaying file types using file command](./screenshots/display_file_types.png)

## stat 

### Display File and Directory Statistics

```
stat test1
stat test2
stat test3
stat test4
```

![Display statistics for test files 1-4](./screenshots/stat_test1234.png)

```
stat test5
stat test6
stat test7
```

![Display statistics for test files 5-7](./screenshots/stat_test567.png)

## cp

### Basic copy command

```
ls
cat test3
cp test3 new_test3
ls
cat new_test3
```

![Basics of syntax of the cp command](./screenshots/basic_cp_command.png)

### Copy with prompting before clobber and no clobber

```
ls
cp -i test1 test3
n
cat test3
cp -n test1 test3
cat test3
ls
cp -b test1 test3
ls
cat test3
cat test3~
```

![cp with different options to protect from overwriting destination files](./screenshots/cp_no_overwrite_options.png)

## mv

### basic mv command to rename a file/directory

```
ls
mv test4 test_dir
ls 
mv new_test3 test3_copy
ls
```

![using move to rename a file](./screenshots/basic_mv_rename.png)

### basic mv command to relocate a file


```
ls
mv test3_copy test_dir/
mv test3~ test_dir/
ls
ls test_dir
```

![Relocating files using mv](./screenshots/relocate_files_mv.png)

### mv command without clobbering

```
ls
mv -i test1 test2
n
ls
mv -n test1 test2
ls
mv -b test1 test2
ls
```

![using mv with clobber prevention](./screenshots/mv_command_no_clobber.png)

Notice the symbolic link of test6 to test1 is now broken.
use `ls -l` to see this more clearly.

### move and rename

```
ls
mv test2~ test_dir/backup_test2
ls
ls test_dir
```

![moving and renaming a file at the same time](./screenshots/move_and_rename.png)


## touch command

### Create and new empty file

```
ls 
touch test1
file test1
stat test1
cat test1
```

![Create an empty file with touch](./screenshot/create_with_touch.png)

notice that the symbolic link from test6 to test1 in fixed `ls -l`

### Update the timestamps

```
ls
stat test1
touch test1
stat test1
```

![Updating a time stamp on a file](./screenshot/basic_update_timestamps.png)

## Display file Contents

### Setup

```
ls
for i in {1..1000}; do echo "this is line number $i" >> test1; done
for i in {1..5}; do echo "file1 line $i" >> file1.txt; done
for i in {1..5}; do echo "file2 line $i" >> file2.txt; done
ls
```

![Adding some text to files for display purposes](./screenshots/create_and_fill_files.png)

### cat command

```
ls
cat file1.txt
cat file1.txt file2.txt
cat file1.txt file2.txt > file3.txt
cat file3.txt
ls
```

![Using the cat command to display files](./screenshots/cat_files_to_term.png)

### head command

Display the first lines of a file

```
ls
head test1
head -n 15 test1
```

![Displaying the first part of a file](./screenshots/head_test1.png)

### tail command

Display the last lines of a file

```
ls
tail test1
tail -n 15 test1
```

![Display the last part of a file](./screenshots/tail_test1.png)


Follow a file (good for logs)

```
tail -f /var/log/syslog
^C
```

### less is more

#### more command

```
more test1
RETURN
RETURN
SPACE
B-KEY
B-KEY
Q-KEY
```

#### less command

```
less
RETURN
RETURN
SPACE
SPACE
J-KEY
J-KEY
K-KEY
K-KEY
B-KEY
B-KEY
Q-KEY
```

## rm command

### Prep

```
mkdir dir1
mkdir dir2
```

### Removing files and directories

```
ls
rm file1.txt
rm -i file2.txt
y
ls
rm -d dir1
rmdir dir2
ls
rm -I file3.txt
rm -I test?
y
ls
rm -d test_dir/
rmdir test_dir/
rm -r test_dir/
ls
```

![Removing files and directories](./screenshots/rm_command_summary.png)

# References

1) man pages



