# GhostingMich
evasion technique to defeat and divert detection and prevention of security products (AV/EDR/XDR)

![271839856-3b4607a1-1cc6-41f1-926f-892ae880e7a5](https://github.com/michredteam/GhostingMich/assets/168865716/b8be987e-cd68-4d9e-98d3-8293f45b8d5c)

Security teams defending Windows environments often rely on anti-malware products as their first line of defense against malicious executables. Microsoft offers security vendors the ability to register callbacks that will be triggered upon the creation of processes on the system. Driver developers can utilize APIs like PsSetCreateProcessNotifyRoutineEx to receive these events.

Despite its name, PsSetCreateProcessNotifyRoutineEx callbacks are not actually triggered upon the creation of processes, but rather upon the creation of the first threads within those processes. This creates a gap between when a process is created and when security products are notified of its creation. It also provides malware authors with an opportunity to tamper with the underlying executable before security products can scan it. Recent examples of such tampering attacks include Process Doppelgänging and Process Herpaderping, which exploit this behavior to evade security products.

To launch a new process, a series of steps must occur. In modern versions of Windows, these steps are typically performed in the kernel by NtCreateUserProcess. However, the individual component APIs (such as NtCreateProcessEx) are still exposed and functional for backward compatibility purposes. The steps involved are as follows:

Open a handle to the executable to be launched.
Example: hFile = CreateFile(“C:\Windows\System32\svchost.exe”)
Create an "image" section for the file.
A section maps a file, or a portion of a file, into memory. An image section is a special type of section that corresponds to Portable Executable (PE) files and can only be created from PE (EXE, DLL, etc.) files.
Example: hSection = NtCreateSection(hFile, SEC_IMAGE)
Create a process using the image section.
Example: hProcess = NtCreateProcessEx(hSection)
Assign process arguments and environment variables.
Example: CreateEnvironmentBlock/NtWriteVirtualMemory
Create a thread to execute within the process.
Example: NtCreateThreadEx

[![ghosting.png](https://i.postimg.cc/sgy64Rfd/ghosting.png)](https://postimg.cc/YhnztZmb)

[![malware.gif](https://i.postimg.cc/63GJTZyt/malware.gif)](https://postimg.cc/sQy0NvYN)



