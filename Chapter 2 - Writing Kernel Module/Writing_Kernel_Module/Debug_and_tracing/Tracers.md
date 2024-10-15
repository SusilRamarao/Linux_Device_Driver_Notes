Go to /sys/kernel/debug/tracing to see all the files that are related to tracing.

`tracing_on` is a file that can have either 0 or 1 - 1 means tracing is on
`available_tracers` shows all the available tracers that can be used
`current_tracer` is a file that can be used to see what is the current tracer that is used. We can edit to change the tracer that we require. All tracers that can be added is in `available_tracers` file.
`echo :mod:{name_of_the_kernel_module_created}>set_ftrace_filter` to tell which kernel to be traced.
`trace` file is like a debug file. It shows which functions are called with arguments that are passed.