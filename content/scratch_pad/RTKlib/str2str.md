---
title: str2str
linktitle: str2str
toc: false
type: book
date: "2018-09-09T00:00:00Z"
weight: 1
tags:
  - STR2STR
  - Command line interface
---

Input data from a stream and divide and output them to multiple streams. The input stream can be serial,
TCP client, TCP server, NTRIP client, or file. The output stream can be serial, TCP client, TCP server, NTRIP
server, or file. str2str is a resident type application.

`./str2str -in ntrip://USERNAME:PASSWORD@SERVER:PORT/MOUNTPOINT -out MOUNTPOINT.bin`