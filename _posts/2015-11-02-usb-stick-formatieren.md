---
layout: dark-post
title: USB-Stick formatieren
modified:
categories: howto
description: How to format a USB-Stick
tags: [USB-Stick]
image:
  feature:
  credit:
  creditlink:
comments: false
share: false
---

# Windows

#### Diskpart

Console starten (Ausführen -> cmd)

- DISKPART
- LIST DISK
- SELECT DISK **x** (x in die passende Nummer aendern)
- CLEAN
- CREATE PARTITION PRIMARY
- FORMAT FS=FAT32 QUICK
- ASSIGN

# Linux

#### fdisk

- fdisk /dev/sd**x** (x in die passende Nummer aendern)
- d (fuer delete)
- n (fuer neue Partition erstellen)

