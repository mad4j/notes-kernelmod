# How to write a simple Kernel Module

daniele.olmisani@gmail.com, see [LICENSE](LICENSE) file.

## Install needed stuff

```
$ sudo apt-get install build-essential linux-headers-$(uname -r)
```

## Write your first kernel module

```c++
// file: hellow.c

#include <linux/module.h>
#include <linux/kernel.h>
#include <linux/init.h>

MODULE_LICENSE("GPL");
MODULE_AUTHOR("mad4j");
MODULE_DESCRIPTION("Simple HelloWorld!! module");

static int __init hellow_init(void)
{
	printk(KERN_INFO "HelloWorld!!\n");

	// non-zero means that the module couldn't be loaded
	return 0;
}

static void __exit hellow_exit(void)
{
	printk(KERN_INFO "Goodbye!!\n");
}

module_init(hellow_init);
module_exit(hellow_exit);
```

## Build

create the following makefile:

```makefile
# file: Makefile

KERNELDIR ?= /lib/modules/$(shell uname -r)/build

.PHONY: all clean

obj-m += hellowmod.o

all:
	$(MAKE) -C $(KERNELDIR)o M=$(PWD) modules

clean:
	$(MAKE) -C $(KERNELDIR) M=$(PWD) clean
```

Then build using the following command:

```
$ make all
```

## Install and remove

```
$ insmod hellow.ko

$ rmmod hellow
```

## References

See also:
* http://www.thegeekstuff.com/2013/07/write-linux-kernel-module/
