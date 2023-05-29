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
```bash
pestr malware.exe | more
```