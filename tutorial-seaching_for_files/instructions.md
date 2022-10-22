# Searching For Files

## locate

locate requires a database of files and directories to be created and updated regularly to be effective.

### Creating a Database for locate

```
sudo updatedb
```

![Creating database for locate](./screenshots/locate_updatedb.png)

### Locating Files

```
locate passwd | tail
locate passwd | head
```

![locating files with passwd in them](./screenshots/locate_passwd.png)

### Locating Files with case-insensitive

```
locate pacific | tail
locate -i pacific | tail
```

![locating files with case insensitivity](./screenshots/locate_pacific_caseinsensitive.png)

### Only Locate file and not directories

```
locate -i pacific | tail
locate -ib pacific | tail
```

![locate only files](./screenshots/locate_base_name.png)

### Locate, Count Match, but don't display matchs

```
locate -c passwd
```

![Count matches with locate](./screenshots/locate_count.png)

## find

### Find Files By Name and List Them

```
sudo find /etc -name passwd -type f -ls
sudo find /etc -name *shadow* -type f -ls
```

![Finding the passwd and shadow files](./screenshots/find_by_name_and_list.png)

### Find Files by Access time in days

```
sudo find /etc -atime -1 -type f -ls | head
sudo find /etc -atime +1 -type f -ls | head
```

![Find files by access time](./screenshots/find_file_access_time.png)

### Find Files by Modify time in days

```
sudo find /etc -mtime -1 -type f -ls
sudo find /etc -mtime +1 -type f -ls | tail
```

![Find modified files](./screenshots/find_modify_time.png)


### File File By Permissions

```
sudo find /etc -perm 444 -type f -ls
sudo find /etc -perm -444 -type f -ls | tail
```

![Find files in /etc by permissions](./screenshots/find_permissions_etc.png)

```
sudo find / -perm -4000 -type f -ls | head
```

![Find set-uid files](./screenshots/find_set_uid.png)

### Find Files by User Owner

```
sudo find / -user kittykat -type f -ls | tail
```

![Find files owned by kittykat](./screenshots/find_user_files.png)

### Find Files by Group Owner

```
sudo find / -group kittykat -type f -ls | tail
```

![Find files owned by group kittykat](./screenshots/find_group_files.png)




