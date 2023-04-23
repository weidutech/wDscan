# wDscan
A Super Fast and Passive Bug Bounty Scanner

```
           ____                                 _____  _____  _____
 _      __/ __ \______________ _____     _   __/ ___/ / ___/ / ___/
| | /| / / / / / ___/ ___/ __ `/ __ \   | | / / __ \ / __ \ / __ \
| |/ |/ / /_/ (__  ) /__/ /_/ / / / /   | |/ / /_/ // /_/ // /_/ /
|__/|__/_____/____/\___/\__,_/_/ /_/    |___/\____(_)____(_)____/
```




# 主从架构

1.主扫描器模块：

主扫描器模块是整个分布式扫描器的核心，负责管理整个工作流程。该模块包含三个子模块：

- 端口扫描器：扫描目标主机的端口，以确定哪些服务正在运行。可以使用第三方库/工具扫描。
- 爬虫扫描器：以用户指定的方式（例如深度优先或广度优先）爬取网站，并从网页中提取域名和URL，针对登陆的功能，取决与爬虫的能力，可以随着技术的进步进行迭代，所以解耦合开发。
- 调度模块：分配任务并将其分配给工作节点。使用Celery库进行任务调度。

将爬到的域名和URL存储到数据库中。使用MongoDB或Elasticsearch等非关系型数据库，处理大量数据和高并发。

2.从扫描器模块：

从扫描器模块是负责扫描目标主机的模块。这个模块启动一个被动代理服务，接收来自主扫描器模块发送的HTTP报文或URLwithData，并对其进行扫描。

该模块需要与主扫描器模块进行通信，以获取任务并报告扫描结果。

每个模块可以共享数据，进行SmartScan减少没用的发包量，因为它需要同时处理多个扫描任务。使用Python的异步框架（例如asyncio）来实现高并发扫描。

3.通信模块：

主扫描器模块和从扫描器模块之间需要进行通信。可以使用ZeroMQ或Redis等消息队列来实现这种通信。

这个架构可以在不同的服务器上部署多个工作节点来扩展扫描能力。主扫描器模块可以轻松地向工作节点分配任务，并从它们那里收集扫描结果。同时，从扫描器模块能够并行处理多个扫描任务，并将结果发送回主扫描器 模块。

4.预备POC模块

能够支持新0day直接快速打全网

5.尝试AI技术，不强求，随着技术进步进行迭代。
