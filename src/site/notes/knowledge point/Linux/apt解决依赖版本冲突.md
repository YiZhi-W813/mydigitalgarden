---
{"tags":["Linux","tools"],"dg-publish":true,"permalink":"/knowledge point/Linux/apt解决依赖版本冲突/","dgPassFrontmatter":true}
---

在 Unbuntu 系统上安装各种软件时，经常会遇到各种各样的依赖问题而导致安装无法进行。我作为一枚 Linux 小白正深受其苦，经常越弄越乱导致不得不重装系统（哭）。通常来说，这类问题可以通过 `更换下载源`、`apt-get update` 和 `apt-get upgrade` 来解决。但更经常会遇到连这三幻神（雾）都没法解决问题的时候。
此时可以使用`aptitude`来解决安装过程中过包依赖问题。
首先安装aptitude
	sudo apt install aptitude
然后利用aptitude安装指定包
	sudo aptitude install *package_name*
aptitude会给出一个solution，阅读solution如果不是我们想要的就输入n，aptitude会给出别的solution，直到给出我们需要的solution。