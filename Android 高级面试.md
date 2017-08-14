[http://www.cnblogs.com/deman/p/5860976.html](http://www.cnblogs.com/deman/p/5860976.html "Android 高级面试题及答案")


1.如何对Android应用进行性能分析（[http://www.cnblogs.com/wanqieddy/p/4494896.html](http://www.cnblogs.com/wanqieddy/p/4494896.html "android 性能调试工具")）

	A: 函数分析  工具TraceView 1.设置开始点和结束点  2.采集数据  3.按执行时间排序  4.按照函数被调用次数以及递归调用占总调用次数的百分比排序

				工具 Systrace 是Android4.1中新增的性能数据采样和分析工具。它可帮助开发者收集Android关键子系统（如surfaceflinger、WindowManagerService等Framework部分关键模块、服务）的运行信息，从而帮助开发者更直观的分析系统瓶颈，改进性能。

				工具 Oprofile (它利用性能计数器(Performance Counter)或者定时器(针对kernel不支持性能计数器的情况)，通过连续的采样获得统计数据，从而对内核和用户空间进程进行性能分析。)