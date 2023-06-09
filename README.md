# Table of Contents
- [Table of Contents](#table-of-contents)
  - [Find Files in REMnux](#find-files-in-remnux)
  - [Decode HEX](#decode-hex)
  - [OSINT and Sandboxes](#osint-and-sandboxes)
  - [Mutex](#mutex)
  - [Static Properties Analysis](#static-properties-analysis)
    - [Execute With Full Privileges](#execute-with-full-privileges)
    - [Static Properties](#static-properties)
    - [Looking at Strings](#looking-at-strings)
      - [pestr](#pestr)
      - [BinText](#bintext)
      - [strings2](#strings2)
      - [Interesting Strings](#interesting-strings)
    - [pestudio](#pestudio)
    - [peframe](#peframe)
    - [PE Header Details](#pe-header-details)
      - [More Static Property Analysis Tools](#more-static-property-analysis-tools)
  - [Behavioral Analysis](#behavioral-analysis)
    - [Essentials](#essentials)
    - [Basic Process](#basic-process)
    - [Network Emulation](#network-emulation)
      - [DNS](#dns)
      - [HTTP](#http)
  - [Code Analysis Essentials](#code-analysis-essentials)
    - [x64dbg](#x64dbg)
    - [x64dbg Example](#x64dbg-example)
    - [API Monitor](#api-monitor)
    - [Decode Hex](#decode-hex-1)
    - [Use XOR to Decode](#use-xor-to-decode)


## Find Files in REMnux  
```sudo find / -type f -name pescanner.py```

## Decode HEX
```xxd -r -p encoded.hex```

## OSINT and Sandboxes
- [Malware Report Template](https://zeltser.com/malware-analysis-report/)
- [Malware Repositories](https://zeltser.com/malware-sample-sources/)
- [Automated Sandboxes](https://zeltser.com/automated-malware-analysis/)
- [Multi Engine AV Scanners](https://zeltser.com/multi-engine-av-scanning-sites/)
- [Website Investigation](https://zeltser.com/lookup-malicious-websites/)
- [Other](https://github.com/hslatman/awesome-threat-intelligence)  

## Mutex
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

### PE Header Details
- Programs have a specially formatted header that contains the details the operating system needs to
load the file into memory and set up the runtime environment for the process so it can run properly
 - Packing typically involves obfuscating, encrypting, or otherwise encoding the
original executable to create a new file that embeds the original program as data
- When the packed program runs, it unpacks itself into the memory of the infected host

    - [Detect It Easy](https://github.com/horsicq/Detect-It-Easy)  
    - [Exeinfo PE](https://github.com/ExeinfoASL/ASL)

#### More Static Property Analysis Tools
- [signsrch](http://aluigi.altervista.org/mytoolz.htm) : Locates code used for crypto, compression, and more
- [pescan](http://pev.sf.net) and [portex](https://katjahahn.github.io/PortEx) : Examine key aspects of Windows executable files and identify some anomalies
- [MASTIFF](https://git.korelogic.com/mastiff.git) : Extracts many details from various types of malware;
good for bulk review of many samples (launch using the mas.py command)
- [exiftool](http://www.sno.phy.queensu.ca/~phil/exiftool) : Displays metadata embedded in files of various types
- [trid](http://mark0.net/soft-trid-e.html) : Identifies the type of file you're starting to examine
- [Viper](https://github.com/viper-framework/viper) : Manages the malware collection and extracts various
static properties about the files

## Behavioral Analysis
### Essentials
- [Process Hacker](https://processhacker.sourceforge.io/): Replaces built-in Task Manager, similar to Microsoft's Process Explorer tool
- [Process Monitor](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon): Records interactions of processes with the registry, file system, network, and other processes
- [Regshot](https://sourceforge.net/projects/regshot/): Highlights changes to the file system and the registry
- [ProcDOT](https://www.procdot.com/): Visualizes Process Monitor logs for easier analysis
- [Wireshark](https://www.wireshark.org/): Sniffs the network and captures packets

### Basic Process
1. Launch Process Hacker
2. Launch Process Monitor
   1. Pause Capture
   2. Clear Log
3. Launch Regshot
   1. Take 1st Shot
4. Launch WireShark
5. Activate Capture in Process Monitor
6. Launch Malware
   1. Wait 30 Seconds
7. Terminate Malware in Process Hacker
8. Pause Capture in Process Monitor
9. Pause Capture in WireShark
10. Take 2nd Shot in Regshot
    1.  Compare Registry Snapshots
11. Filter Process Monitor by Malware Process Name
    1.  Save All Events from Process Monitor as CSV
12. Launch ProcDot
    1.  Upload Process Monitor CSV to ProdDot
    2.  Select Process and Click Refresh
  
### Network Emulation
- A common practice when performing behavioral analysis is to give the specimen the network resources
that it seeks

#### DNS
[fakedns](https://code.activestate.com/recipes/491264-mini-fake-dns-server/)  
- On REMnux
- Automatically responds to DNS queries with the IP address of the host on which it runs

#### HTTP
- On REMnux
- ```httpd start```


## Code Analysis Essentials

- Disassembling involves translating binary machine-level instructions to human-readable assembly language code.
- Static code-level analysis involves using a disassembler to examine the program's code without actually executing it.
- Dynamic code-level analysis involves examining the code at the assembly level while running the program.

### x64dbg
[Link](https://github.com/x64dbg/x64dbg)
- Debugger or Windows Executables
- Open Source
- Handles 32 and 64-bit code
- Supports plugins and scripts

### x64dbg Example
1. Load Malware in x64dbg
   1. Disassembler is visible on the **CPU** tab
      1. One instruction per line
      2. 2nd column shows hex values that represent relative addresses (offsets) of instructions
         1. ```00007FF713F53F07```
      3. 3rd column shows groupings of instructions in hex
         1. ```B9 09000000  ```
      4. Next column version of those instructions in human-readable assembly
         1. ```mov ecx,9 ```
      5. Right most columns show comments
         1. ```9:'\t'```
2. Set Breakpoint
   1. Direct the debugger to pause the executing of the debugged process by setting a breakpoint that defines the circumstances under which x64dbg will pause the specimen and give you control
   2. Review executable's imports to spot API calls
      1. Example ```ReadFile```
      2. Set Breakpoint on the code that might read the file to find the general vicinity of code
      3. SetBPX ReadFile
3. Debug > Run
   1. Pauses at ReadFile in kernel32.dll
      1. Pauses AFTER code makes the ReadFile call
   2. Title of x64dbg window shows current location
   3. RIP designates the current instruction
      1. current means the instruction will be executed next by the debugged program
4. Examine Parameters pass to ReadFile
   1. 64-bit code often uses the register ```rcx``` for supplying the first parameter to a function
   2. Glance at the middle region of the right side of the **CPU** tab
5. Determine where the handle is pointing
   1. Go to handle tab
   2. Right Click and Click Refresh
   3. Look for the name of the desired handle
6. Allow ReadFile to Finish Running
   1. Examine the code implemented by the developer (user code) to see what the specimen does with the contents once they've been read
   2. Debug > Run to User Code
      1. Run until it reaches user code
   3. Call Stack
      1. Nested Functions in Reverse Chronological Order
      2. After **From** the function will return to **To**
      3. ```call``` operand is the typical way in which functions are invoked
7. Run instructions until the instruction after CryptDecrypt call
   1. Click on instruction
   2. Debug > Run Until Selection

### API Monitor
[Link](http://www.rohitab.com/apimonitor)
- Quick way to spy on the parameters the program passes to external functions as well as to examine the return values of API calls

### Decode Hex
- Boolean XOR
  - Method to obfuscate strings
  - Change each character in the original string by XOR it with a 1-byte key value
- ROR (Rotate Right) or ROL (Rotate Left)
- ROT 
  - Rotate Alphabet characters by a certain number of positions within the alphabet
  
### Use XOR to Decode
- ```scite encoded.hex```
- ```xxd -r -p encoded.hex > encoded.raw```
- ```translate.py encoded.raw decoded.txt 'byte ^ 0x5b'```
- ```cat decoded.txt```

