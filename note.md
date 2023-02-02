

JobThread 每个任务根据jobId一个线程去处理







```
示例：registry，callback，registryRemove
执行中心 每隔一个beat
ExecutorRegistryThread#registryThread
注册本地执行器节点；

调度中心 接收
JobApiController
执行相应的操作

调度中心同样会自己检测执行器的节点是否存在JobRegistryHelper#registryMonitorThread
借助中间表xxl_job_registry操作：执行器操作xxl_job_registry表，调度中心根据xxl_job_registry表操作xxl_job_group表，之后的调度依据xxl_job_group操作
```



~~~
调度中心向执行器发送执行的请求
JobTriggerPoolHelper#addTrigger
XxlJobTrigger#trigger
XxlJobTrigger#processTrigger
XxlJobTrigger#runExecutor
ExecutorBizClient#run
XxlJobRemotingUtil#postBody

执行器的netty服务检测到请求
EmbedServer#EmbedHttpServerHandler#channelRead0#process
ExecutorBizImpl#run //获取JobThread和IJobHandler，把TriggerParam放在JobThread的队列中，待线程取出处理（处理的事情为请求）



TriggerCallbackThread.pushCallBack//放入队列中后，
TriggerCallbackThread#doCallback//下边线程方法取出处理，
AdminBizClient#callback//调用调度中心接口
调度中心
JobApiControlle#callback
AdminBizImpl#callback//更新处理结果到日志中




~~~

[调度流程](https://blog.csdn.net/weixin_42241455/article/details/117249534)



~~~
XxlJobContext中的一个对象
InheritableThreadLocal<XxlJobContext> contextHolder
线程中创建XxlJobContext，使该线程持有该对象的相关信息
~~~





