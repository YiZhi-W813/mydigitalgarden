---
{"tags":["Linux","tools"],"dg-publish":true,"permalink":"/Knowledge point/Linux/vim常用快捷键汇总/","dgPassFrontmatter":true}
---

移动光标：`h (left)       j (down)       k (up)       l (right)
删除光标处内容：`x
光标处插入内容：`i 插入在光标前 a插入在光标后
删除光标到下个单词：`dw
删除光标到下个单词末尾：`de
删除光标到行尾：`d$
删除整行：`dd
撤回操作: `u`
撤回撤回操作：`CTRL-R
剪切：`先删除d再p
显示您在文件中的位置和文件状态：`CTRL+G
移动到指定行：`number G
移动到第一行：`Gg
向前搜索：`/`  向后搜多：`?`
继续搜索：正方向`n` 反方向`N`
用new替换行中第一个old`:s/old/new`
用new代替一行中所有的old`:s/old/new/g`
替换两行#之间的短语:#，`#s/old/new/g`
替换文件类型中出现的所有内容:`%s/old/new/g`
要每次请求确认，请添加'c':`%s/old/new/gc`
复制粘贴：`y` `p`
查找cmd的帮助：`help cmd`
跳转到另一个窗口：`CTRL-W`
