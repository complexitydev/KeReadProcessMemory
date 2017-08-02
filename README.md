# KeReadProcessMemory
Created because of a post that stated "Just block access to your app and they will never be able to search for sigs"... Right.

I'll keep working on this project. Eventually, I'll have a C# user-land counterpart to search a process for a pattern. Please note that this method of reading process memory defeats every known anti-cheat, anti-virus, anti-debugging, etc.

Please note that this driver is not created with the intention of doing something malicious. The sole purpose of it is to bypass any anti-debugging/memory protection techniques that harmful processes such as malware employ.

# How to Use
Create a user-land app that implements DeviceIOControl. Use VirtualMemorySize64 or some variant to find the size and then make the IOCTL call.
