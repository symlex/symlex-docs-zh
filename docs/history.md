# Symfony 和 Silex 的关系

Symlex 于 2014 年作为一个简单的 Silex 样板开始，因为 Silex 本身没有『标准版』这指向了正确的方向。 SensioLabs『创作者』推荐使用 Silex 代替 Symfony 两种框架，作为 Symfony + FOSRestBundle 的轻量级替代品，可快速构建高性能 REST 服务和单页 Web 应用程序。

很快就注意到 Pimple -- Silex 附带的服务容器 - 对于来自 Symfony 的开发人员来说感觉很麻烦，并且很难重用现有的代码。 此外，许多 Silex 代码示例甚至是现实世界的应用程序都从代码的所有部分『不仅仅是框架本身』访问服务容器，这避免了控制的反转并导致难以测试的可测试性。 因此， Symlex 促进了依赖注入的严格使用，并将完整服务容器的便利性与微框架的速度相结合。

今天，Symlex有自己的路由组件基于 Symfony 4，不再使用 Silex了 。 事实证明，该框架对大量不同的应用程序非常有用。 其中一些基于之前的常规 Symfony 内核并进行了更改，因为它们在复杂性方面淹没，并且在开发模式下的响应时间远远超过30秒。 Symlex 使他们重回正轨，而不对现有代码库进行大的改动。

<img src="https://symlex.org/images/symlex-vs-symfony.svg" alt="Micro-Kernel Architecture" width="100%">
