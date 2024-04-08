---
{"tags":["Linux"],"dg-publish":true,"permalink":"/knowledge point/Linux/Linux常用命令/","dgPassFrontmatter":true}
---

### 查看磁盘空间
	df -h

### 关闭系统
	poweroff

### apt安装指定版本包
	sudo apt install package_name=package-version-number

### 比较两个文件是否完全相同
	文本文件的比较: `vimdiff file1 file2`
	非文本文件的比较: `diff file1 file2`

### 查看文件详细信息
	`stat <filename>`

### 检索目录下.c/.h文件代码行数（忽略空行）
	```sfind . -name "*.h" -or -name "*.c" | xargs grep -ve "^$" | wc -l```
