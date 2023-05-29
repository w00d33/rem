# Malware Analysis Fundamentals
## Introduction to Malware Analysis
- [Malware Report Template](https://zeltser.com/malware-analysis-report/)
- [Malware Repositories](https://zeltser.com/malware-sample-sources/)
- [Automated Sandboxes](https://zeltser.com/automated-malware-analysis/)
- [Multi Engine AV Scanners](https://zeltser.com/multi-engine-av-scanning-sites/)
- [Website Investigation](https://zeltser.com/lookup-malicious-websites/)
- [Other](https://github.com/hslatman/awesome-threat-intelligence)  

### Mutex
- A flag that programs use to serialize access to a resource
- Sometimes used by malware to avoid reinfecting the host (infection markers)
- Sometimes embedded in a program as a string
- Usually requires behavioral or code analysis

## Static Properties Analysis
### Execute With Full Privileges
- Right-click the shortcut.
- Select Properties.
- Click Advanced.
- Enable Run as administrator.
- Click OK
### Static Properties
- File and section hashes
- Packer identification
- Embedded resources
- Imports and exports
- Crypto references
- Digital certificates
- "Interesting" strings

### Looking at Strings
#### pestr
[Link](https://sourceforge.net/projects/pev/)  
```pestr malware.exe | more```
- Look for:
    - File Names
    - Registry Keys
    - URL Patterns

### Other PEV Tools
- pesec: check security features in PE files, extract certificates, and more
- readpe: parse PE headers, sections, imports and exports
- pescan: detect TLS callback functions, DOS stub modification, suspicious sections and more
- pedis: disassembly a PE file section or function with support for Intel and AT&T syntax
- pehash: calculate PE file hashes including imphash and ssdeep
- pepack: detect whether an executable is packed or not
- pestr: search for hardcoded Unicode and ASCII strings simultaneously in PE files
- peres: show and extract PE file resources

#### BinText
[Link](https://www.majorgeeks.com/files/details/bintext.html#:~:text=BinText%20is%20a%20small%2C%20fast,Resource%20strings%20in%20a%20file.)
- Another way to examine ASCII and Unicode-encoded strings on a file is to use the GUI tool BinText

#### strings2
[Link](https://www.split-code.com/strings2.html)  

Example Usage

Dump all strings from `malware.exe` to stdout:

* ```strings2 malware.exe```

Dump all strings from all `.exe` files in the `files` folder to the file `strings.txt`:
* ```strings2 ./files/*.exe > strings.txt```

Dump strings from a specific process id, including logging the module name and memory addresses of each match:
* ```strings2 -f -s -pid 0x1a3 > process_strings.txt```

Extract strings from `malware.exe` to a json file:
* ```strings2 malware.exe -json > strings.json```