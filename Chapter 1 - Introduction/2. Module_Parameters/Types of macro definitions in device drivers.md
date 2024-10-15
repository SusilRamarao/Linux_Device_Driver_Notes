1. module_param(variable, int, S_IRWXU)
	1. arg1 : variable name
	2. arg2 : data_type
	3. arg3 : permission modes
2. MODULE_PARAM_DESC(variable, "A Sample integer modifiable parameter")
	1. arg1: A variable already defined
	2. arg2 : comment/description about the parameter

**Example usage:**
	unsigned int copybreak = DEFAULT_VALUE;
	**module_param(copybreak, uint, 0644)**
	MODULE_PARAM_DESC(copybreak, "Maximum size of packet that is copied to a new buffer");
	
	This example can be seen in Param.c of e1000e driver of net/ehternet/intel.

For more info about different other parameters
https://litux.nl/mirror/kerneldevelopment/0672327201/ch16lev1sec6.html

Usage:
insmod mymod.ko xyz=1 abc="Hello"
If we are writing our own module file, it is better to use insmod

If initiating a module through modsprobe, then we need to use option key and add the variables in a config file.

Note: Each driver might have parameters and can be present as a file. Parameter files are used only for non-zero permission modes.
