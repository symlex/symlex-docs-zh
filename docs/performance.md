# 性能

很明显， PHP 框架性能主要取决于必须为每个请求执行的代码行。虽然 Symlex 的设计简单而精简，但良好的性能是这种方法非常重要的副产品。

!!! quote
    最好的代码是没有代码。 没有代码的地方，没有错误。 没有 API 可以学习。
    没有尴尬的用户界面。 最好的重构是删除。<br> ― *Eric Elliott*

由 [phpbenchmarks.com](http://www.phpbenchmarks.com/en/benchmark/apache-bench/php-7.3/symlex-4.1.html) 发布，
与下一个最好的 PHP 框架相比， Symlex 目前为 REST 请求减少了 42％ 的开销：

<img src="https://symlex.org/images/performance-large.svg" alt="Response times of popular PHP frameworks" width="100%">

请注意，这些响应时间是在 [快速服务器硬件](http://www.phpbenchmarks.com/en/benchmark-protocol.html) 上以完全优化的生产模式测量的，只有 5 个并发请求。 在实践中，绝对时间方面的差异可能会大得多。 还应考虑内存消耗：

<img src="https://symlex.org/images/memory-large.svg" alt="Memory footprint of popular PHP frameworks" width="100%">

## 你为什么要在意框架性能？ ##

首先，您的用户会喜欢它。 根据经验，**100 毫秒** 是让他们感受到系统的极限即时反应，意味着除了显示结果外不需要特殊反馈。总响应时间还包括网络（~25毫秒），浏览器和其他开销，只留下一小部分那些 100 毫秒用于实现实际业务逻辑。

其次，您将为服务器基础架构节省大量资金，并且随着测试运行速度的提高，开发人员的工作效率也会提高。
