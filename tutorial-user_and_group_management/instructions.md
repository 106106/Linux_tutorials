# User And Group Management Commands

## adduser command

### Adding a normal user with just the defaults 

```
sudo adduser user01
*Enter password for sudo priviledges*
*Enter a password for user01*
*Re-type the password for user01*
ENTER
ENTER
ENTER
ENTER
ENTER
Y
```

![Output of adduser with defaults](./screenshots/adduser_user01.png)

Verify the results of `sudo adduser user01`

```
ls /home
grep user01 /etc/passwd
id user01
```

![Checking user01 was created](./screenshots/verify_adduser_user01.png)

Verify that user01 doesn't have sudo priviledges

```
su - user01
*Enter user01's password*
sudo vim /etc/shadow
*Enter user01's password*
```

![Verifying user01 doesn't have sudo priviledges](./screenshots/user01_not_sudo.png)

Logout or exit from user01

```
exit
```

### Adding a normal user with comments

```
sudo adduser user02
*Enter password for sudo priviledges*
*Enter a password for user02*
*Re-enter the password for user02*
user02
101
1-234-567-8910
1-234-567-8910
Comments
y
```

![Output of adding user02](./screenshots/adduser_user02.png)

Verify the results of `sudo adduser user02`

```
ls /home
grep user01 /etc/passwd
id user01
```

![Checking user02 was created](./screenshots/verify_adduser_user02.png)


### Adding a normal user with a non-default shell and home directory

```
sudo adduser --shell /bin/sh --home /home/user03_home user03
```

![Output of adduser with options](./screenshots/adduser_user03)

Verify the results of adding user03

```
ls /home
grep user03 /etc/passwd
id user03
su - user03
*Enter user03 password*
echo $SHELL
exit
```

![Output of verifying user03 details](./screenshots/verify_adduser_user03.png)

### Adding a system user

System users or service accounts are typically created when a new service is install to provide the necessary security context for the service. We can create a system user/account with

```
sudo adduser --system system_user01
```

![Output of adding a system user](./screenshots/adduser_system_user01.png)

Inspect system\_user01 properties

```
ls /home
grep system_user01 /etc/passwd
id system_user01
sudo grep system_user01 /etc/shadow
```

![Output of inspecting system\_user01](./screenshots/verify_adduser_system_user01.png)

Notice the second field is a * . This is not a valid output of crypt(3) thus the system\_user01 will not be able to use a unix style password to log in, but may be able to login via another method. 

## addgroup command

### Adding A Group With No Options

add a normal group with the following

```
addgroup group01
```

![Output of addgroup group01](./screenshots/addgroup_group01.png)

Verify group01 was created

```
grep group01 /etc/group
```
![Displaying group01 info](./screenshots/verify_group01.png)

### Adding a Group With A Specific GID

```
addgroup --gid 2222 group02
```

![Output of addgroup with custume GID](./screenshots/addgroup_group02.png)

Verify group02 was created

```
grep group02 /etc/group
```

![Displaying group02 info](./screenshots/verify_group02.png)


### Adding an existing user to a group

```
adduser user01 group01
grep group01 /etc/group
id user01
```

![Adding user01 to group01](./screenshots/adduser_user01_to_group01.png)

## deluser command

### Delete A User

```
ls /home
grep user02 /etc/passwd
id user02
deluser user02
ls /home
grep user02 /etc/passwd
grep user02 /etc/group
```

![Deleting user02](./screenshots/deluser_user02.png)

Note: the users home directory and mail spool (if created) did not get deleted.
Let's remove user02's home directory manually

```
rm -r /home/user02
```

### Delete A User and Their Home Directory

```
ls /home
grep system_user01
id system_user01
deluser --remove-home system_user01
ls /home
grep system_user01 /etc/passwd
```

![Deleting a user and their home directory](./screenshots/deluser_user_home.png)

### Delete a User and All Their Files

What if the user has a file not in their home directory that we also want to remove.
We can remove all the files own by the user as follows. First we'll create some file for demo purposes.

```
cd ..
ls
sudo mkdir user03_files
sudo chown user03:user03 user03_files/
ls -l
```

![Creating a folder owned by user03 not in their home directory](./screenshots/create_user03_folder.png)

Lets create some more user03 files

```
su - user03
whoami
cd ../user03_files
touch file1.txt
touch file2.txt
ls -l
touch /tmp/another_file.txt
ls -l /tmp/another_file.txt
exit
```

![Created more files owned by user03](./screenshots/user03_create-files.png)


Let's finally delete user03

```
sudo deluser --remove-all-files user03
ls /home
ls /tmp
```

## delgroup command

### Delete An empty Group

```
tail -n 2 /etc/group
sudo delgroup group02
tail -n 2 /etc/group
```

![Output of deleting an empty group](./screenshots/delgroup_empty_group.png)


### Delete a non-empty Group

```
tail -n 2 /etc/group
sudo delgroup group01
tail -n 2 /etc/group
```

![Output of deleting a non-empty group](./screenshots/delgroup_nonempty.png)


## useradd command

### useradd with no options

```
sudo useradd user02
ls /home
grep user02 /etc/passwd
sudo grep user02 /etc/shadow
```

![Output of useradd with no options](./screenshots/useradd_user02.png)

Notice the ! mark for user02's password and that the home directory for user02 is /home/user02but the directory was not actually created.

### useradd create home dir

```
sudo useradd -m user03
ls /home
grep user03 /etc/passwd
sudo grep user03 /etc/shadow
```

![Output of useradd with -m option](./screenshots/useradd_user03.png)

The home directory was created but no password.

### Useradd with a bunch of options

Create user04, their home directory, assign them bash as their shell, give them uid 1111, and add a comment in the /etc/passwd file.

```
useradd -m -s /bin/bash -u 1111 -c "koi pond" user04
ls /home
grep user04 /etc/passwd
sudo grep user04 /etc/shadow
id user04
```

![Output of useradd with lots of options](./screenshots/useradd_user04.png)

## groupadd command

### groupadd with no comments

```
groupadd group01
tail -n 5 /etc/group
```

![Output of groupadd with no options](./screenshots/groupadd_group01.png)

### groupadd with custume GID

```
groupadd -g 2022 group02
tail -n 5 /etc/group
```

![Output of groupadd with custume GID](./screenshots/groupadd_group02.png)


## usermod command

### Change a user's name

```
tail -n 5 /etc/passwd
ls /home
sudo usermod -l new_user01 user01
ls /home
tail -n 5 /etc/passwd
```

![Result of changing a user's name](./screenshot/usermod_user01_name.png)

### Change a user's home directory

Note: Just renaming the user's home directory is not sufficient since their default environment variables will also need to be changed.

```
grep new_user01 /etc/passwd
ls /home
usermod -m -d new_user01 new_user01
grep new_user01 /etc/passwd
ls /home
```

![Result of moving/changing a user's home directory](./screenshots/usermod_user01_home.png)


### Change a user's shell

```
grep user02 /etc/passwd
sudo usermod -s /bin/bash user02
grep user02 /etc/passwd
sudo su - user02
echo $SHELL
exit
```

![Result of changing a user's shell](./screenshots/usermod_user02_shell.png)

### Add or change a user comment

```
grep user02 /etc/passwd
sudo usermod -c "new comment" user02
grep user02 /etc/passwd
```

![Result of changing a user's comment field](./screenshots/usermod_user02_comment.png)

### Add a user to a group

First we are not going to use the -a option to demo an issue that can arise, then use -a option.

```
tail -n 2 /etc/group
id user02
sudo usermod -G group02 user02
tail -n 2 /etc/group
id user02
sudo usermod -G group01 user02
tail -n 2 /etc/group
id user02
sudo usermod -aG group02 user02
tail -n 2 /etc/group
id user02
```

![Output of adding a user to multiple groups with and without -a](./screenshots/usermod_user02groups.png)

Note: we can use -G without the -a option in a creative way to remove a user from a group. You can also do the following.

```
tail -n 2 /etc/group
id user02
sudo deluser user02 group01
tail -n 2 /etc/group
id user02
```

![Removing user02 from group01](./screenshots/deluser_user02_group.png)

## groupmod command

### Change a group name

```
tail -n 2 /etc/group
id user02
sudo groupmod -n new_group02 group02
tail -n 2 /etc/group
id user02
```

![Changing a group name](./screenshots/groupmod_group02_name.png)

### Change GID of user primary group

```
grep "user02" /etc/passwd
grep "user02" /etc/group
sudo groupmod -g 1222 user02
grep "user02" /etc/passwd
grep user02" /etc/group
```

![Changing gid of user02 group](./screenshots/groupmod_user02_gid.png)

### Change GID of standard group

```
tail -n 2 /etc/group
touch testfile.txt
sudo chown :group01 testfile.txt
ls -l testfile.txt
sudo groupmod -g 1010 group01
tail -n 2 /etc/group
ls -l testfile.txt
```

![Changing gid of standard group](./screenshots/groupmod_group01_gid.png)

Notice the testfile group ownership did not get changed.

## userdel command

### Delete a user without removing home folder

```
ls /home
grep "new_user01" /etc/passwd
sudo userdel new_user01
ls /home
grep "new_user01" /etc/passwd
sudo rm -r /home/new_user01
```

![Deleting new_user01](./screenshots/userdel_new_user01.png)

### Delete a user and their home directory

```
ls /home
grep "user04" /etc/passwd
sudo userdel -r user04
ls /home
grep "user04" /etc/passwd
```

![Deleting user04 and their home directory](./screenshots/userdel_user04_home.png)

## groupdel command

### Delete a empty group

```
tail -n 2 /etc/group
sudo groupdel group01
tail -n 2 /etc/group
```

![Deleting empty group](./screenshot/groupdel_group01_empty.png)

### Delete a non-empty group

```
tail -n 2 /etc/group
id user02
sudo groupdel new_group02
tail -n 2 /etc/group
id user02
```

![Deleting non-empty group](./screenshots/groupdel_group02_nonempty.png)


# References

1. man pages
