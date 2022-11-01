# [openssl3-nov2022](https://mta.openssl.org/pipermail/openssl-announce/2022-October/000238.html)

![](https://img.shields.io/badge/last--updated-December%202021%20-green) ![](https://img.shields.io/badge/src-public-orange)

[Rolling 2 day view of updates from this repo](https://github.com/CiscoCXSecurity/openssl3-nov2022/compare/master@%7B2day%7D...master)

## Public trackers

* https://gsd.id/GSD-2022-1006615
* https://github.com/pblumo/openssl-vuln-nov-2022/blob/main/list.csv
* https://github.com/NCSC-NL/OpenSSL-2022
* https://docs.google.com/spreadsheets/d/1EMGeffxhiRceyz6XW6O6cH5tcrBaoLLX06M9OdNsENY/edit#gid=1075768917

## Kick banning attacks at the perimeter

* TBC

## Paths to check

### UNIX

* ```/opt```
* ```/usr/local```
* ```/home```

### OS X

(see also UNIX)

* ```/Applications```
* ```/Library```
* ```/Users/*/Applications```
* ```/Users/*/Library```

### Windows

* ```c:\Program Files```
* ```c:\Program Files (x86)```
* ```c:\Documents and Settings```
* ```c:\Users```

## Dirty checks

* ```find /path/to/check -type f -iname "lib*ssl*.so*" -o -iname "lib*crypt*.so*" -o -iname "lib*ssl*.a*" -o -iname "lib*crypt*.a*" 2>/dev/null | while read filename; do echo $filename,`strings $filename | grep "OpenSSL 3" | wc -l`; done```
* ```osqueryi "SELECT distinct processes.name, process_open_sockets.local_port, process_memory_map.path as ssllib from process_memory_map join process_open_sockets USING (pid) join processes USING (pid) WHERE (process_memory_map.path LIKE '%lib%ssl%' OR process_memory_map.path LIKE '%lib%crypt%') AND process_memory_map.permissions LIKE '%x%' AND process_open_sockets.local_port <> 0;"```
* ```Get-ChildItem -Recurse -File -ErrorAction SilentlyContinue -Path "C:\" -Filter "lib*ssl*"```
* Correlation with other mission critical packages e.g. OpenSSH (https://twitter.com/j0hn__f/status/1587067842515673090): OpenSSH >= 8.9 is a relatively good indicator that the OS also ships with OpenSSL 3
* Hunt for "OpenSSL/3.*" in SIEM, WAF logs etc

## Yara rules

Running the rules:

* ```yara -r yara/... /path/to/check```

Example here:

* TBC

### Personal

* TBC

## Source code to check

* https://codesearch.debian.net/search?q=libssl3&literal=1&perpkg=1
