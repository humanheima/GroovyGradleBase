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