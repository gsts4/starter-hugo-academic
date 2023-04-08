---
title: VSCode Launch JSON
linktitle: launch.json
toc: false
type: book
date: "2018-09-09T00:00:00Z"
weight: 3
tags:
  - VSCode
  - launch.json
  - Debugging RTKlib
---

The following is my current launch.json when using RTKlib with VSCode

```
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "rnx2rtkp",
            "type": "cppdbg",
            "request": "launch",
            "cwd": "${workspaceFolder}/app/consapp/rnx2rtkp/gcc/",
            "program": "${workspaceFolder}/app/consapp/rnx2rtkp/gcc/rnx2rtkp",
            "args": [
                "-x", "4",
                "-y", "2",
                "-k", "${workspaceFolder}/app/consapp/rnx2rtkp/gcc/proc.conf",
                "-o", "${workspaceFolder}/data/logfile/validations/output_file.out",
                "${workspaceFolder}/data/logfile/validations/rover_logfile.rnx",
                "${workspaceFolder}/data/logfile/validations/base_logfile.rnx",
                "${workspaceFolder}/data/logfile/validations/eph_logfile.nav"
            ],
            "stopAtEntry": true,
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "rnx2rtkp compile",
            "miDebuggerPath": "/usr/bin/gdb"
        },
        {
            "name": "pos2kml",
            "type": "cppdbg",
            "request": "launch",
            "cwd": "${workspaceFolder}/app/consapp/pos2kml/gcc/",
            "program": "${workspaceFolder}/app/consapp/pos2kml/gcc/pos2kml",
            // "args": ["-o", "../rtk_pb.conf"],
            "stopAtEntry": true,
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "pos2kml compile",
            "miDebuggerPath": "/usr/bin/gdb"
        },
        {
            "name": "rtkrcv",
            "type": "cppdbg",
            "request": "launch",
            "cwd": "${workspaceFolder}/app/consapp/rtkrcv/gcc/",
            "program": "${workspaceFolder}/app/consapp/rtkrcv/gcc/rtkrcv",
            "args": [
                "-s",
                "-p","3000",
                "-m","4000",
                "-o", "${workspaceFolder}/app/consapp/rtkrcv/conf/rtk.conf",
                "-r","3"],
            "stopAtEntry": true,
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "rtkrcv compile",
            "miDebuggerPath": "/usr/bin/gdb"
        },
        {
            "name": "str2str",
            "type": "cppdbg",
            "request": "launch",
            "cwd": "${workspaceFolder}/app/consapp/str2str/gcc/",
            "program": "${workspaceFolder}/app/consapp/str2str/gcc/str2str",
            // "args": ["-o", "../rtk_pb.conf"],
            "stopAtEntry": true,
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "str2str compile",
            "miDebuggerPath": "/usr/bin/gdb"
        },
        {
            "name": "convbin",
            "type": "cppdbg",
            "request": "launch",
            "cwd": "${workspaceFolder}/app/consapp/convbin/gcc/",
            "program": "${workspaceFolder}/app/consapp/convbin/gcc/convbin",
            // "args": ["-o", "../rtk_pb.conf"],
            "stopAtEntry": true,
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "convbin compile",
            "miDebuggerPath": "/usr/bin/gdb"
        },
    ]
}

```