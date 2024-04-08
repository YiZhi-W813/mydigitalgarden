---
{"dg-publish":true,"tags":["chisel"],"permalink":"/work diary/一生一芯/Chisel初体验——Hello World！/","dgPassFrontmatter":true}
---

将chisel-examples git下来，包含一个Hello World工程，代码源文件位于src/main/scala/目录下。
### 源文件
```scala
import chisel3._    //导入chisel库

class Hello extends Module {    //定义一个Hello类
	val io = IO(new Bundle {    //电路接口说明
    val led = Output(UInt(1.W))
	})
	val CNT_MAX = (50000000 / 2 - 1).U//逻辑功能实现

	val cntReg = RegInit(0.U(32.W))
	val blkReg = RegInit(0.U(1.W))

	cntReg := cntReg + 1.U
	when(cntReg === CNT_MAX) {
	    cntReg := 0.U
	    blkReg := ~blkReg
  }
	io.led := blkReg
}

object Hello extends App {    //输出Verilog
	(new chisel3.stage.ChiselStage).emitVerilog(new Hello())
}
```

### 仿真测试
测试文件源代码位于src/test/scala/目录下，sbt test运行测试文件产生vcd波形文件，最后gtkwave查看仿真波形。
```scala
import chiseltest._
import org.scalatest.flatspec.AnyFlatSpec

class HelloTest extends AnyFlatSpec with ChiselScalatestTester {
	behavior of "Hello"
	it should "pass" in {
		test(new Hello) { c =>
			c.clock.setTimeout(0)
			var ledStatus = BigInt(-1)
			println("Start the blinking LED")
			for (_ <- 0 until 100) {
				c.clock.step(10000)
				val ledNow = c.io.led.peek().litValue
				val s = if (ledNow == 0) "o" else "*"
				if (ledStatus != ledNow) {
					System.out.println(s)
					ledStatus = ledNow
				}
			}
			println("\nEnd the blinking LED")
		}
	}
}
```
