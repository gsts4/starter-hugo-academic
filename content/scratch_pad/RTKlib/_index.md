---
title: RTKlib
linktitle: RTKlib 💻
toc: false
type: book
date: "2018-09-09T00:00:00Z"
weight: 1
tags:
  - RTKRCV
  - RNX2RTKP
  - CONVBIN
  - STR2STR
---

Recommended repo that's maintained by RTKlib Explorer

```
git clone git@github.com:rtklibexplorer/RTKLIB.git
```

Compile IERS files

```
cd /lib/iers/gcc
make
```

RTKLIB includes a few different command line user interfaces which include :
1. RTKRCV - Real‐time Positioning
1. RNX2RTKP - Post‐Processing Analysis
1. CONVBIN - RINEX Converter of receiver raw data
1. [STR2STR](../str2str/) - Stream Server
