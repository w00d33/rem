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

Other PEV Tools
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

#### Interesting Strings
- IOCs for Detection:
    - File system: malware.tmp
    - Network: %s?i=%s&c=%s&p=%s
- Persistence Mechanism:
    - Registry: Software\Microsoft\Windows\CurrentVersion\Run
- Communication Mechanism:
    - Network: HTTP/1.1
    - User-Agent: Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1; Trident/4.0)

###  pestudio
[Link](https://www.winitor.com/download)
- Automatically highlights
potentially malicious aspects of the Windows executable that the tool is examining
- Calculates various hash values

<img src="https://media.geeksforgeeks.org/wp-content/uploads/20220915224514/1.jpg"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" />

###  peframe
[Link](https://github.com/guelfoweb/peframe)
- Open source tool to perform static analysis on Portable Executable malware and generic suspicious files - Detect packer, xor, digital signature, mutex, anti debug, anti virtual machine, suspicious sections and functions, macros

###  Find Files in REMnux  
```sudo find / -type f -name pescanner.py```

### PE Header Details
[Detect It Easy](https://github.com/horsicq/Detect-It-Easy)  

<img src="https://raw.githubusercontent.com/horsicq/Detect-It-Easy/master/docs/1.png"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" />  

[Exeinfo PE](https://github.com/ExeinfoASL/ASL)


