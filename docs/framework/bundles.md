# 依赖包

Symlex 不支持 Symfony Bundles ，因为我们致力于构建专注，精益和完全可测试的应用程序。

  - 我们发现使用它们通常会增加整体架构的复杂性：它们隐藏了引导程序/配置细节，并鼓励构建膨胀的应用程序
  - 如果某些功能仅在框架中编码，则无法编写有意义的单元测试
    配置文件或由 bundle 神奇地生成
  - 可以创建验收测试，但它们很慢并且不适合测试驱动开发（ TDD ）和重构

!!! quote
    我见过很多人试图使用 FOSUserBundle 。我现在已经挣扎了 6 个小时了。 只是为了能够制作自定义用户注册表单。基本文档长达 6 页......<br>
    ― *Olivier Pons 在 [Stack Overflow](http://stackoverflow.com/questions/19064719/fosuserbundle-what-is-the-point)*

## 注释 ##

由于我们发现难以维护和测试，因此也不支持注释。 我们更喜欢使用一小段代码来 [配置](config.md) 我们的应用或使用服务容器。
