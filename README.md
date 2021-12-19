### 日志级别

通过 `gradle -q + 任务名称`来运行一个指定的task，这个q是命令行开关选项，通过开关选项可以控制输出的日志级别。

请参考：[Gradle核心思想（二）Gradle入门前奏](http://liuwangshu.cn/application/gradle/2-primer.html)
http://liuwangshu.cn/application/gradle/2-primer.html

### 排除任务

比如 go 任务依赖 hello 这个任务，在执行的时候想只执行go任务，可以使用`-x`参数：
```
-x, --exclude-task        Specify a task to be excluded from execution.
```

```groovy

 gradle -q go -x hello      

```
`-q`参数是可选项，即`--quiet`，只打印错误日志
 
```groovy

task hello {
    doLast {
        println 'Hello world!'
    }
}

/**
 * go 任务依赖 hello 这个任务
 */
def Task go = task(go, dependsOn: hello)
go.doLast {
    println "go for it"
}

```
### 执行多个任务

```groovy
gradle hello1 go
```

### 闭包的概念

Groovy中的闭包是一个开放的、匿名的、可以接受参数和返回值的代码块。
定义闭包
闭包的定义遵循以下语法：

```
{ [closureParameters -> ] statements }
```
闭包分为两个部分，分别是参数列表部分`[closureParameters -> ]`和语句部分 `statements` 。
参数列表部分是可选的，如果闭包只有一个参数，参数名是可选的，Groovy会隐式指定it作为参数名，如下所示。

```
{ println it }     //使用隐式参数it的闭包
```
当需要指定参数列表时，需要用箭头 `->` 将参数列表和闭包体相分离。

```
{ it -> println it }   //it是一个显示参数 
{ String a, String b ->                                
    println "${a} is a ${b}"
}
```
闭包是groovy.lang.Cloush类的一个实例，这使得闭包可以赋值给变量或字段，如下所示。
```
//将闭包赋值给一个变量
def onlyPrintln ={ it -> println it }     
assert println instanceof Closure
//将闭包赋值给Closure类型变量
Closure doCloush= { println 'do!' }
```
### Gradle 插件类型
在Gradle中一般有两种类型的插件，分别叫做脚本插件和对象插件。脚本插件是额外的构建脚本，它会进一步配置构建，可以把它理解为一个普通的build.gradle。对象插件又叫做二进制插件，是实现了Plugin接口的类