# 项目名称
迷你抓取器spider

## 编译
```
go build -o mini_spider main.go
```

## 用法
```
Usage of ./mini_spider:
  -alsologtostderr
    	log to standard error as well as files
  -c string
    	assign config file path (default "./conf/spider.conf")
  -h	show spider help
  -log_backtrace_at value
    	when logging hits line file:N, emit a stack trace
  -log_dir string
    	If non-empty, write log files in this directory
  -logtostderr
    	log to standard error instead of files
  -stderrthreshold value
    	logs at or above this threshold go to stderr
  -v value
    	log level for V logs
  -version
    	show spider version
  -vmodule value
    	comma-separated list of pattern=N settings for file-filtered logging
```

## 测试
```
go list ./...| grep -vE "vendor" | xargs -n1 go test -v -cover
```

##设计思路
1. 根据配置文件起n个gorountinue协程，这n个协程用来处理爬虫任务，既是生产者，又是消费者
2. 程序启动时，根据url.data文件读取的地址信息作为初始爬虫任务种子，交给gorountinue消费
3. 爬虫任务开始时通过http连接目标url，抓取网页内容，依次解析html页面标签，根据配置文件内正则表达式和设置的抓取深度匹配页面内符合条件的数据，若匹配成功，用匹配成功的数据构建新的爬虫任务，通过新协程扔进入任务通道
4. 当所有爬虫任务消费完，程序退出


