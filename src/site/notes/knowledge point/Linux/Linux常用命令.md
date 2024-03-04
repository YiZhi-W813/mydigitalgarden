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