---
title: MacOS.System.Packages
hidden: true
tags: [Client Artifact]
---

Parse packages installed on Macs


```yaml
name: MacOS.System.Packages
description: |
  Parse packages installed on Macs

sources:
  - precondition: |
      SELECT OS From info() where OS = 'darwin'
    query: |
        LET packages = SELECT parse_json(data=Stdout) AS Json
          FROM execve(argv=[
            "system_profiler", "-json", "SPApplicationsDataType"
          ], length=100000)

        SELECT *, _name AS Name
        FROM foreach(
           row=packages[0].Json.SPApplicationsDataType)

```