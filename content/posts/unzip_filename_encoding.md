---
title: "Fix zip file names using unzip"
date: 2024-12-02T22:47:21+01:00
description: "Fix zip file names using unzip"
tags: ["zip", "linux", "encoding"]
draft: false
---

Double-check the exact name of the encoding, as to not misspell it: https://www.iana.org/assignments/character-sets/character-sets.xhtml

The run

```
$ unzip -O <encoding> <filename> -d <target_dir>
```
or
```
$ unzip -I <encoding> <filename> -d <target_dir>
```
choosing between -O or -I according to instructions here:
```
$ unzip -h
UnZip 6.00 of 20 April 2009, by Debian. Original by Info-ZIP.
    ...
    -O CHARSET  specify a character encoding for DOS, Windows and OS/2 archives
    -I CHARSET  specify a character encoding for UNIX and other archives
    ...
```
which means that simply try -O and it should work, because not a lot of people would create a .zip file in Unix...

> Source: https://superuser.com/questions/872596/how-to-decompress-a-zip-file-with-specified-file-directory-name-character-encodi
