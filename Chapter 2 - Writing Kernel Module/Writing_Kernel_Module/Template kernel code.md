`#include<linux/module.h>`

`int my_init_module(void){`
	`printk("initialization of module");`
	`return 0;`
`}`

`void cleanup_module(void){`
	`printk("exit of module");`
`}`

`module_init(my_init_module);`
`module_exit(cleanup_module);`
`MODULE_LICENSE("GPL");`


Example actual static Kernel code
![[Pasted image 20240922195914.png]]