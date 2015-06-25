# rskbuild

This is a python script that can be placed in the top source directory of Linux. It can then be used to invoke rustc with the boolean style kernel configuration included. This means configurations are automatically translated into the rust builds.

It generates it's own .wrconfig so the --cfg options fit in with rust style; the preceeding `CONFIG_` is removed, everything is lowercase and the option is succeeded by `_kbuild`

eg.

`CONFIG_X86_32` becomes `x86_32_kbuild`

Thus to conditionally compile if that configuration is toggled, you do:

```
#[cfg(x86_32_kbuild)
fn only_32bit() { }
```

.wrconfig is also regenerated when .config is modified.

*note* --target is set to 32bit, change where necessary

### Options

* `crate` will compile and link the rust code into a crate
* `obj` will compile the rust code to object code for linking into the kernel

### example Makefile

```
$(obj)/egcrate.o: $(src)/egcrate.rs
	./rskbuild crate $(src)/egcrate.rs
	./rskbuild obj $(src)/egcrate.rs -o $(obj)/egcrate.o

$(obj)/example.o: $(src)/example.rs
	./rskbuild obj $(src)/example.rs -o $(obj)/example.o

obj-y=egcrate.o example.o
```

### License 

MIT
