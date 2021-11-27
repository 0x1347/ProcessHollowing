## Process Hollowing in C#

Adversaries may inject malicious code into suspended and hollowed processes in order to evade process-based defenses. Process hollowing is a method of executing arbitrary code in the address space of a separate live process.<br>
Process hollowing is commonly performed by creating a process in a suspended state then unmapping/hollowing its memory, which can then be replaced with malicious code. <br>
A victim process can be created with native Windows API calls such as <b>CreateProcess</b>, which includes a flag to suspend the processes primary thread. At this point the process can be unmapped using APIs calls such as <b>ZwUnmapViewOfSection</b> or <b>NtUnmapViewOfSection</b> before being written to, realigned to the injected code, and resumed via <b>VirtualAllocEx</b>, <b>WriteProcessMemory</b>, <b>SetThreadContext</b>, then <b>ResumeThread</b> respectively.
This is very similar to Thread Local Storage but creates a new process rather than targeting an existing process. <br>
This behavior will likely not result in elevated privileges since the injected process was spawned from (and thus inherits the security context) of the injecting process. <br>However, execution via process hollowing may also evade detection from security products since the execution is masked under a legitimate process.
