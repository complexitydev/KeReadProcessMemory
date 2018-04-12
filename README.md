# KeReadProcessMemory
A lot of people suggest that you implement anti-debugging and memory protection into your application. People even go to the length of using callbacks to deny handle access to the process. All of these techniques are worthless with a bit of kernel code and reversing knowledge.

For example, an anti-virus may attempt to use ObRegisterCallbacks in order to monitor everytime a new process is created, a handle, etc. None of that matters when in kernel. However, the main drawback to this driver is that is visible in the loaded module list, which many anti-viruses now scan as part of their "rootkit" detection. Perhaps use Turla Driver Loader, which would require a few small tweaks to the driver. Be warned that when using TDL, SEH will no longer function. In order to get around this, perhaps check to make sure the address resides in that applications memory space. 

There is also the posibilty that the anti-virus may attempt a signature/pattern search in kernel memory. Not only is this stupid, but I'd avoid any anti-virus that threatens system stability.

I'll keep working on this project. Eventually, I'll have a C# user-land counterpart to search a process for a pattern, which was the main point of it all. There is nothing you can do to prevent analysis on your program if someone is dedicated enough.

I thought I'd do a bit of a disclaimer: this driver is not created with the intention of doing anything malicious. The sole purpose of it is to bypass any anti-debugging/memory protection techniques that harmful processes such as malware employ.

# How to Use
Create a user-land app that implements DeviceIOControl. Use VirtualMemorySize64 or some variant to find the size and then make the IOCTL call. Or use this to read a specific address, it really doesn't matter. 

# Writing Memory
If you swap source and target, you can instead write to that area in memory. MmCopyVirtualMemory states "copies bytes from one address to another". Instead of writing to the current buffer, instead just write to another program's memory space. This will be useful when wanting to inject shellcode. Simply reverse until you find a function that is called frequently. Or even with a bit of work, inject a module into that process. MmCopyVirtualMemory is extremely versatile & quite honestly performs better than the alternatives.

# Blue-Screens
I have extensively tested this driver and added SEH where needed. When implementing a writing function, use ProbeForWrite Feel free to create a Github issue if one occurs. The only possible blue-screen is the user-land buffer (a pointer) being passed incorrectly.
