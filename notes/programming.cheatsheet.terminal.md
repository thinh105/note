---
id: 7d6nt7hno4ahp0htvvjx6af
title: Terminal
desc: ''
updated: 1663409164560
created: 1663409164560
isDir: false
latitude: 21.0313
longitude: 105.8516
altitude: 0
title_imported: Terminal
updated_imported: '2021-01-23T03:15:49.000Z'
created_imported: '2020-02-14T11:03:42.000Z'
---

[TOC]



## Unzip file

```bash
unzip <file-name.zip> -d <folder>
```

## Move file
```bash
mv <file-name> <new-folder>
```

## Rename folder

```bash
mv <old-name-folder> <new-name-folder>
```

## list dict and size

```bash
du -sh *
```
short version of:
```bash
du --summarize --human-readable *
```

Explanation:
du: Disk Usage

-s: Display a summary for each specified file. (Equivalent to -d 0)

-h: "Human-readable" output. Use unit suffixes: Byte, Kibibyte (KiB), Mebibyte (MiB), Gibibyte (GiB), Tebibyte (TiB) and Pebibyte (PiB). (BASE2)



## copy file
```bash
cp /home/myProjects/Node/"Jonas Course - Natour"/{.prettierrc,.gitignore,.eslintrc.json} /home/myProjects/Node/learn-mongoose    
```

## delete file

```bash
rm filename
rm filename1 filename2 filename3
rm *.pdf
```
## delete folder
```bash
# To remove an empty directory, use either rmdir or rm -d
# followed by the directory name
rm -d dirname
rmdir dirname

# To remove non-empty directories and all the files within them,
# use the rm command with the-r (recursive) option:
rm -r dirname
```

## list all file

```bash
ls -a
```

## show only hidden file

```bash
ls -ld .?* 
```
Will only list hidden files .

Explain :

```bash
 -l     use a long listing format

 -d, --directory
              list  directory entries instead of contents, and do not derefer‐
              ence symbolic links

.?* will only state hidden files 
```