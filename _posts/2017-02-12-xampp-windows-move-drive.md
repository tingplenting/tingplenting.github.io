---
layout: post
title: How to move your site data to another drive in windows 10 xampp
date: 2017-02-06 
---

### XAMPP v3.2.2

#### www

line 246 & 247 -> C:\xampp\apache\conf\httpd.conf

```sh
DocumentRoot "C:/xampp/htdocs"
<Directory "C:/xampp/htdocs">
```

```sh
DocumentRoot "D:/Sites"
<Directory "D:/Sites">
```

#### mysql

C:\xampp\mysql\bin\my.ini

```sh
C:/xampp/mysql/data
```

to 
```sh
D:/databases
```

