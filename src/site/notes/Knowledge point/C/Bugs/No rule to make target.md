---
{"dg-publish":true,"tags":["C","Bugs"],"permalink":"/Knowledge point/C/Bugs/No rule to make target/","dgPassFrontmatter":true}
---

make C工程时报错：
	make[1]: Entering directory '/home/yizhi_w/ysyx_workbench/npc/obj_dir'
	make[1]: *** No rule to make target '../csrc/macro.h', needed by 'sim_main.o'. Stop.
	make[1]: Leaving directory '/home/yizhi_w/ysyx_workbench/npc/obj_dir'
按照一般的思路，会去检查Makefile中文件之间的依赖关系，有没有哪里写错了，引用的路径，文件名是否正确等等。
但还有一些不易察觉的原因也会导致这个问题，例如工程更改路径或者更改了其中的文件夹名称之后，之前生成的.o.d文件在再次编译时并不会重新编译，因而导致该问题，需要删除obj目录并重新编译。