# Linux Ubuntu Commands

Created: September 12, 2024 1:48 PM
Labels & Language: Linux Ubuntu, Shell

## Description

## Linux Ubuntu Commands

### chmod Method

```bash
# chmod Method in Ubuntu

sudo chmod -R 600 ××× （只有所有者有读和写的权限）
sudo chmod 644 ××× （所有者有读和写的权限，组用户只有读的权限）
sudo chmod 700 ××× （只有所有者有读和写以及执行的权限）
sudo chmod 666 ××× （每个人都有读和写的权限）
sudo chmod 777 ××× （每个人都有读和写以及执行的权限）
```

### Extract files

The **`extract`** command is not a specific command in Linux Ubuntu. However, if you are referring to extracting files from an archive, you can use different commands depending on the type of archive:

1. **`.tar`** archive: Use the **`tar`** command with the **`x`** flag followed by the name of the archive file. For example: **`tar -xvf file.tar`** will extract the contents of **`file.tar`** to the current directory.
2. **`.tar.gz`** or **`.tgz`** archive: Use the **`tar`** command with the **`xz`** flags followed by the name of the archive file. For example: **`tar -xvzf file.tar.gz`** will extract the contents of **`file.tar.gz`** to the current directory.
3. **`.zip`** archive: Use the **`unzip`** command followed by the name of the archive file. For example: **`unzip file.zip`** will extract the contents of **`file.zip`** to the current directory.
4. **`.rar`** archive: Use the **`unrar`** command followed by the name of the archive file. For example: **`unrar x file.rar`** will extract the contents of **`file.rar`** to the current directory.

Note that these commands may require additional options or flags depending on the specific archive file. You can refer to the manual pages (**`man`**) for each command for more information.