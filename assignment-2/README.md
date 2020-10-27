# Assignment 2:

### Development Environment:
* Operatinng Systems: Ubuntu 16.04
* Processor: Intel
* GNU toolchains

### Prepare machine
#### Check kernel version in Ubuntu machine
```bash
roopesh@roopesh:~/SJSU/283/assignment2/linux-5.9.1$ uname -a
Linux roopesh 4.4.0-103-generic #126-Ubuntu SMP Mon Dec 4 16:23:28 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux
```

#### Download kernel source

#### Compile new kernel source and install

### Modify KVM drivers
#### Update source file
#### Compile changes
```bash
roopesh@roopesh:~/SJSU/283/assignment2/linux-5.9.1$ sudo make -C . M=arch/x86/kvm modules
make: Entering directory '/home/roopesh/SJSU/283/assignment2/linux-5.9.1'
  CC [M]  arch/x86/kvm/vmx/vmx.o
  LD [M]  arch/x86/kvm/kvm-intel.o
  MODPOST arch/x86/kvm/Module.symvers
  LD [M]  arch/x86/kvm/kvm-intel.ko
make: Leaving directory '/home/roopesh/SJSU/283/assignment2/linux-5.9.1'
```
#### Installing

### Installing virtual machine


### Running test code on guest VM


### Testing changes
