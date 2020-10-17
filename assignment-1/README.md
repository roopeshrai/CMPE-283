# Assignment 1: Discovering VMX Features

### Development Environment:
* Operatinng Systems: Ubuntu 16.04
* Processor: Intel
* GNU toolchains

### Know the host machine
```bash
roopesh@roopesh:/media/roopesh/ROOPESH_PASSPORT/sjsu/283/resources$ lscpu | grep "Model name"
Model name:            Intel(R) Core(TM) i3-5010U CPU @ 2.10GHz
roopesh@roopesh:/media/roopesh/ROOPESH_PASSPORT/sjsu/283/resources$ lscpu | grep "Vendor ID"
Vendor ID:             GenuineIntel
roopesh@roopesh:/media/roopesh/ROOPESH_PASSPORT/sjsu/283/CMPE-283/assignment-1$ cat /etc/lsb-release 
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=16.04
DISTRIB_CODENAME=xenial
DISTRIB_DESCRIPTION="Ubuntu 16.04.6 LTS"
```

### Verify 'VMX' supported in CPU
```bash
roopesh@roopesh:/media/roopesh/ROOPESH_PASSPORT/sjsu/283/resources$ lscpu | grep vmx
Flags:                 fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts 
acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts rep_good 
nopl xtopology nonstop_tsc cpuid aperfmperf pni pclmulqdq dtes64 monitor ds_cpl vmx est tm2 ssse3 sdbg fma 
cx16 xtpr pdcm pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm
3dnowprefetch cpuid_fault epb invpcid_single pti ssbd ibrs ibpb stibp tpr_shadow vnmi flexpriority ept vpid
fsgsbase tsc_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap intel_pt xsaveopt dtherm ida arat pln 
pts md_clear flush_l1d
```
vmx is listed in flags.

### Git repository
* Create a project - CMPE-283
* Clone to local machine: 
```bash
git clone https://github.com/roopesh-rai-s-2/CMPE-283.git
```
Create a branch for assignment, Download Makefile and sourcecode template.

### List all MSRs and get information from Intel SDM
* MSR names: IA32_VMX_PINBASED_CTLS, IA32_VMX_PROCBASED_CTLS, IA32_VMX_PROCBASED_CTLS2, IA32_VMX_EXIT_CTLS, IA32_VMX_ENTRY_CTLS
* update `capability information` in cmpe283-1.c using information available in SDM documents
* Update `detect_vmx_features` function in cmpe283-1.c to print all capabiltiy informations 

#### IA32_VMX_PINBASED_CTLS
* Information is in SDM volume 3, Table 24.5

#### IA32_VMX_PROCBASED_CTLS
* Information is in SDM volume 3, Table 24.6

#### IA32_VMX_PROCBASED_CTLS2
* Information is in SDM volume 3, Table 24.7

#### IA32_VMX_EXIT_CTLS
* Information is in SDM volume 3, Table 24.11

#### IA32_VMX_ENTRY_CTLS
* Information is in SDM volume 3, Table 24.13

### Compiling module
* Use `make` command to compile module
* Make sure both `Makefile` and source file `cmpe283-1.c` in same directory

```bash
roopesh@roopesh:/media/roopesh/ROOPESH_PASSPORT/sjsu/283/CMPE-283/assignment-1$ make
make -C /lib/modules/4.15.0-120-generic/build M=/media/roopesh/ROOPESH_PASSPORT/sjsu/283/CMPE-283/assignment-1 modules
make[1]: Entering directory '/usr/src/linux-headers-4.15.0-120-generic'
  CC [M]  /media/roopesh/ROOPESH_PASSPORT/sjsu/283/CMPE-283/assignment-1/cmpe283-1.o
  Building modules, stage 2.
  MODPOST 1 modules
WARNING: modpost: missing MODULE_LICENSE() in /media/roopesh/ROOPESH_PASSPORT/sjsu/283/CMPE-283/assignment-1/cmpe283-1.o
see include/linux/module.h for more information
  CC      /media/roopesh/ROOPESH_PASSPORT/sjsu/283/CMPE-283/assignment-1/cmpe283-1.mod.o
  LD [M]  /media/roopesh/ROOPESH_PASSPORT/sjsu/283/CMPE-283/assignment-1/cmpe283-1.ko
make[1]: Leaving directory '/usr/src/linux-headers-4.15.0-120-generic'
```
* Check .ko file is generated
```bash
roopesh@roopesh:/media/roopesh/ROOPESH_PASSPORT/sjsu/283/CMPE-283/assignment-1$ ls -l
total 39
-rwxrwxrwx 1 roopesh roopesh  6186 Oct 17 14:19 cmpe283-1.c
-rwxrwxrwx 1 roopesh roopesh 11008 Oct 17 15:08 cmpe283-1.ko
-rwxrwxrwx 1 roopesh roopesh   603 Oct 17 15:08 cmpe283-1.mod.c
-rwxrwxrwx 1 roopesh roopesh  2584 Oct 17 15:08 cmpe283-1.mod.o
-rwxrwxrwx 1 roopesh roopesh 10840 Oct 17 15:08 cmpe283-1.o
-rwxrwxrwx 1 roopesh roopesh   425 Oct 17 15:08 cmpe283-1.o.ur-safe
-rwxrwxrwx 1 roopesh roopesh   161 Oct 17 14:13 Makefile
-rwxrwxrwx 1 roopesh roopesh    83 Oct 17 15:08 modules.order
-rwxrwxrwx 1 roopesh roopesh     0 Oct 17 15:08 Module.symvers
-rwxrwxrwx 1 roopesh roopesh    11 Oct 17 14:12 README.md
```

### Clear dmesg logs and insert module
```bash
roopesh@roopesh:/media/roopesh/ROOPESH_PASSPORT/sjsu/283/CMPE-283/assignment-1$ sudo dmesg --clear
roopesh@roopesh:/media/roopesh/ROOPESH_PASSPORT/sjsu/283/CMPE-283/assignment-1$ sudo ins
insmod                 install                install-docs           install-info           installkernel          install-printerdriver  install-sgmlcatalog    instmodsh
roopesh@roopesh:/media/roopesh/ROOPESH_PASSPORT/sjsu/283/CMPE-283/assignment-1$ sudo insmod cmpe283-1.ko
```

### Print dmesg
```bash
roopesh@roopesh:/media/roopesh/ROOPESH_PASSPORT/sjsu/283/CMPE-283/assignment-1$ dmesg
[ 5573.682618] CMPE 283 Assignment 1 Module Start
[ 5573.682620] Pinbased Controls MSR: 0x7f00000016
[ 5573.682621]   External Interrupt Exiting: Can set=Yes, Can clear=Yes
[ 5573.682622]   NMI Exiting: Can set=Yes, Can clear=Yes
[ 5573.682623]   Virtual NMIs: Can set=Yes, Can clear=Yes
[ 5573.682624]   Activate VMX Preemption Timer: Can set=Yes, Can clear=Yes
[ 5573.682625]   Process Posted Interrupts: Can set=No, Can clear=Yes
[ 5573.682626] Processor Based Controls MSR: 0xfff9fffe0401e172
[ 5573.682628]   Interrupt-window exiting: Can set=Yes, Can clear=Yes
[ 5573.682629]   Use TSC offsetting: Can set=Yes, Can clear=Yes
[ 5573.682630]   HLT exiting: Can set=Yes, Can clear=Yes
[ 5573.682631]   INVLPG exiting: Can set=Yes, Can clear=Yes
[ 5573.682632]   MWAIT exiting: Can set=Yes, Can clear=Yes
[ 5573.682633]   RDPMC exiting: Can set=Yes, Can clear=Yes
[ 5573.682634]   RDTSC exiting: Can set=Yes, Can clear=Yes
[ 5573.682635]   CR3-load exiting: Can set=Yes, Can clear=No
[ 5573.682636]   CR3-store exiting: Can set=Yes, Can clear=No
[ 5573.682637]   CR8-load exiting: Can set=Yes, Can clear=Yes
[ 5573.682638]   CR8-store exiting: Can set=Yes, Can clear=Yes
[ 5573.682639]   Use TPR shadow: Can set=Yes, Can clear=Yes
[ 5573.682640]   NMI-window exiting: Can set=Yes, Can clear=Yes
[ 5573.682641]   MOV-DR exiting: Can set=Yes, Can clear=Yes
[ 5573.682642]   Unconditional I/O exiting: Can set=Yes, Can clear=Yes
[ 5573.682643]   Use I/O bitmaps: Can set=Yes, Can clear=Yes
[ 5573.682644]   Monitor trap flag: Can set=Yes, Can clear=Yes
[ 5573.682645]   Use MSR bitmaps: Can set=Yes, Can clear=Yes
[ 5573.682646]   MONITOR exiting: Can set=Yes, Can clear=Yes
[ 5573.682647]   PAUSE exiting: Can set=Yes, Can clear=Yes
[ 5573.682648]   Activate secondary controls: Can set=Yes, Can clear=Yes
[ 5573.682649] Secondary Processor Based Controls MSR: 0x53cff00000000
[ 5573.682650]   Virtualize APIC accesses: Can set=Yes, Can clear=Yes
[ 5573.682651]   Enable EPT: Can set=Yes, Can clear=Yes
[ 5573.682652]   Descriptor-table exiting: Can set=Yes, Can clear=Yes
[ 5573.682653]   Enable RDTSCP: Can set=Yes, Can clear=Yes
[ 5573.682654]   Virtualize x2APIC mode: Can set=Yes, Can clear=Yes
[ 5573.682655]   Enable VPID: Can set=Yes, Can clear=Yes
[ 5573.682656]   WBINVD exiting: Can set=Yes, Can clear=Yes
[ 5573.682658]   Unrestricted guest: Can set=Yes, Can clear=Yes
[ 5573.682659]   APIC-register virtualization: Can set=No, Can clear=Yes
[ 5573.682660]   Virtual-interrupt delivery: Can set=No, Can clear=Yes
[ 5573.682661]   PAUSE-loop exiting: Can set=Yes, Can clear=Yes
[ 5573.682662]   RDRAND exiting: Can set=Yes, Can clear=Yes
[ 5573.682663]   Enable INVPCID: Can set=Yes, Can clear=Yes
[ 5573.682664]   Enable VM functions: Can set=Yes, Can clear=Yes
[ 5573.682665]   VMCS shadowing: Can set=No, Can clear=Yes
[ 5573.682666]   Enable ENCLS exiting: Can set=No, Can clear=Yes
[ 5573.682667]   RDSEED exiting: Can set=Yes, Can clear=Yes
[ 5573.682668]   Enable PML: Can set=No, Can clear=Yes
[ 5573.682669]   EPT-violation #VE: Can set=Yes, Can clear=Yes
[ 5573.682670]   Conceal VMX from PT: Can set=No, Can clear=Yes
[ 5573.682671]   Enable XSAVES/XRSTORS: Can set=No, Can clear=Yes
[ 5573.682672]   Mode-based execute control for EPT: Can set=No, Can clear=Yes
[ 5573.682673]   Sub-page write permissions for EPT: Can set=No, Can clear=Yes
[ 5573.682674]   Intel PT uses guest physical addresses: Can set=No, Can clear=Yes
[ 5573.682675]   Use TSC scaling: Can set=No, Can clear=Yes
[ 5573.682677]   Enable user wait and pause: Can set=No, Can clear=Yes
[ 5573.682678]   Enable ENCLV exiting: Can set=No, Can clear=Yes
[ 5573.682679] VM Exit Controls MSR: 0x7fffff00036dff
[ 5573.682680]   Save debug controls: Can set=Yes, Can clear=No
[ 5573.682681]   Host address-space size: Can set=Yes, Can clear=Yes
[ 5573.682683]   Load IA32_PERF_GLOBAL_CTRL: Can set=Yes, Can clear=Yes
[ 5573.682684]   Acknowledge interrupt on exit: Can set=Yes, Can clear=Yes
[ 5573.682685]   Save IA32_PAT: Can set=Yes, Can clear=Yes
[ 5573.682686]   Load IA32_PAT: Can set=Yes, Can clear=Yes
[ 5573.682687]   Save IA32_EFER: Can set=Yes, Can clear=Yes
[ 5573.682688]   Load IA32_EFER: Can set=Yes, Can clear=Yes
[ 5573.682689]   Save VMXpreemption timer value: Can set=Yes, Can clear=Yes
[ 5573.682690]   Clear IA32_BNDCFGS: Can set=No, Can clear=Yes
[ 5573.682691]   Conceal VMX from PT: Can set=No, Can clear=Yes
[ 5573.682692]   Clear IA32_RTIT_CTL: Can set=No, Can clear=Yes
[ 5573.682693]   Load CET state: Can set=No, Can clear=Yes
[ 5573.682695]   Load PKRS: Can set=No, Can clear=Yes
[ 5573.682696] VM Entry Controls MSR: 0xffff000011ff
[ 5573.682697]   Load debug controls: Can set=Yes, Can clear=No
[ 5573.682698]   IA-32e mode guest: Can set=Yes, Can clear=Yes
[ 5573.682699]   Entry to SMM: Can set=Yes, Can clear=Yes
[ 5573.682701]   Deactivate dualmonitor treatment: Can set=Yes, Can clear=Yes
[ 5573.682702]   Load IA32_PERF_GLOBA L_CTRL: Can set=Yes, Can clear=Yes
[ 5573.682703]   Load IA32_PAT: Can set=Yes, Can clear=Yes
[ 5573.682704]   Load IA32_EFER: Can set=Yes, Can clear=Yes
[ 5573.682705]   Load IA32_BNDCFGS: Can set=No, Can clear=Yes
[ 5573.682706]   Conceal VMX from PT: Can set=No, Can clear=Yes
[ 5573.682707]   Load IA32_RTIT_CTL: Can set=No, Can clear=Yes
[ 5573.682709]   Load CET state: Can set=No, Can clear=Yes
[ 5573.682710]   Load PKRS: Can set=No, Can clear=Yes
```

### Remove module
```bash
roopesh@roopesh:/media/roopesh/ROOPESH_PASSPORT/sjsu/283/CMPE-283/assignment-1$ sudo rmmod cmpe283_1 
```

* Make sure status in dmesg
```bash
[ 5642.586284] CMPE 283 Assignment 1 Module Exits
```
### Upload changes to git repository
* git add <changes>
* git push

### References:
* https://software.intel.com/content/www/us/en/develop/download/intel-64-and-ia-32-architectures-sdm-combined-volumes-1-2a-2b-2c-2d-3a-3b-3c-3d-and-4.html
* Template posted in SJSU canvas 
