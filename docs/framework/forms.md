# 使用表单输入验证

我们的 [InputValidation](https://github.com/symlex/input-validation) 包提供白名单验证（"accept known good"），非常适合构建安全的 REST 服务。 它使用独立于编程语言的验证规则（普通数组），可以重用于其他客户端验证（ JavaScript ）或传递给模板渲染引擎，如 Twig 。 根据设计，它与任何框架和输入源（ HTML，REST，RPC，... ）兼容。

这种与数据源无关的方法的一个主要优点是，开发人员可以使用单元测试进行自下而上的开发，以便及早发现错误，并在没有现有 HTML 前端或存储后端的情况下处理验证规则。 特定于用例的输入值验证也比一般模型验证更安全，后者通常依赖于黑名单（"reject known bad"）。

除了类型或长度等基本验证规则外，还支持更高级的功能 - 例如依赖字段，国际化和多页表单。 验证值可以单独获取，如平面数组，按标签或按页获取。

用法很简单：表单类可以相互继承它们的定义。 如果需要，可以使用标准的面向对象方法更改验证行为。 您不需要拥有设计模式的博士学位来了解它的工作原理。

!!! example
    ```php
    <?php

    namespace App\Form\User;

    use App\Form\FormAbstract;

    class ProfileForm extends FormAbstract
    {
        protected function init(array $params = array())
        {
            $definition = [
                'userFirstName' => [
                    'caption' => 'First Name',
                    'type' => 'string',
                    'min' => 2,
                    'max' => 64,
                    'required' => true,
                ],
                'userLastName' => [
                    'caption' => 'Last Name',
                    'type' => 'string',
                    'min' => 2,
                    'max' => 64,
                    'required' => true,
                ],
                'userEmail' => [
                    'caption' => 'E-mail',
                    'type' => 'email',
                    'max' => 127,
                    'required' => true,
                ],
                'userNewsletter' => [
                    'caption' => 'Receive newsletter and other occasional updates',
                    'type' => 'bool',
                    'required' => false,
                ]
           ];

            $this->setDefinition($definition);
        }
    }
    ```

## 属性 ##

名字                    | 描述
---------------------- | ---------------------------------------------------------------------------------------------------
caption                | 字段标题（用于表单呈现和验证消息）
type                   | 数据类型： int, numeric, scalar, list, bool, string, email, ip, url, date, datetime, time and switch
type_params            | 数据类型验证的可选参数
options                | 字段的可能值的数组（用于选择列表或单选按钮组）
min                    | 数字/日期的最小值，字符串的长度或列表的元素数
max                    | 数字/日期的最大值，字符串的长度或列表的元素数
required               | 字段不能为空（如果为 false ，则 setDefinedValues() 和 setDefinedWritableValues() 仍会抛出异常（如果它根本不存在）
optional               | setDefinedValues() 和 setDefinedWritableValues() 如果输入值中缺少字段，请不要抛出异常 (对于复选框或某些 JavaScript 框架很有用，它们不为空表单元素提交任何数据 例如 AngularJS)
readonly               | 用户不允许更改字段（不可写）
hidden                 | 用户无法看到该字段（对验证没有影响）
default                | 默认值
regex                  | 正则表达式匹配
matches                | 字段值必须与另一个表单字段匹配（例如，用于密码或电子邮件验证）。 属性可以以 "!" 为前缀 说明字段必须不同。
depends                | 如果给定的表单字段不为空，则字段是必需的
depends_value          | 如果“depends”中定义的字段具有此值，则字段是必需的
depends_value_empty    | 如果“depends”中定义的字段为空，则字段是必需的
depends_first_option   | 如果“depends”中定义的字段具有第一个值，则字段是必需的（请参阅“选项”）
depends_last_option    | 如果“depends”中定义的字段具有最后一个值，则字段是必需的（请参阅“选项”）
page                   | 多页表单的页码
tags                   | 可选的标签列表（可用于按标签提取值，请参阅 getValuesByTag()）

## 客户端，表单和模型验证 ##

以下可视化突出显示了客户端，表单（输入值）和模型验证之间的差异。 模型验证通常在可信数据（内部系统状态）上运行，并且在任何时间点都应该是可重复的，而输入验证对来自不可信源（取决于用例和用户权限）的数据显式操作一次。 这种分离使得构建可通过依赖注入耦合的可重用模型，控制器和表单成为可能（参见 REST 控制器示例）。

将表单验证视为白名单验证("accept known good")并将模型验证视为黑名单验证("reject known bad")。 白名单验证更安全，而黑名单验证可防止您的模型层过度受限于非常具体的用例。

无效的模型数据应始终导致抛出异常（否则应用程序可以继续运行而不会注意到错误），而来自外部源的无效输入值不是意外的，而是常见的（除非您让用户永远不会出错）。 如果必须一起验证一组输入值（因为它们彼此依赖），那么特定模型中的验证可能根本不可能，但是然后将各个值存储在不同的模型中 - 至少它可以在模型之间创建额外的依赖关系。 不会在那里，所有模型都相互依赖。 简而言之：应用程序可能仍然按预期工作，但代码很乱。

从理论的角度来看，任何复杂系统都具有比暴露给外部更多的内部状态，因此仅使用模型验证永远不够 - 除了模型提供两组方法：一些在内部使用，一些可以暴露 来自任何来源的任意输入数据。 除了有限的用户反馈（异常消息）和臃肿的模型代码等副作用外，这种方法很容易导致严重的安全漏洞。 与传统的单用户桌面应用程序相比，恶意输入数据对多用户 Web 应用程序的威胁要大得多。 对于桌面应用程序，简单的黑名单模型验证可能完全足够，桌面应用程序完全控制用户界面（视图层）。

客户端（ JavaScript 或 HTML ）表单验证始终只是一个方便的功能，并不可靠。 但是，使用此库，您可以（至少部分地）重用现有的服务器端表单验证规则来执行客户端验证，因为它们可以轻松转换为 JSON （用于 JavaScript ）或传递给模板呈现引擎（如 Twig ）或 Smarty（适用于 HTML ）。 以类似的方式重用模型层验证规则至少是困难的，如果不是不可能的话。

另请参见 [包含业务规则验证（ OWASP ）的位置](https://www.owasp.org/index.php/Data_Validation#Where_to_include_business_rule_validation).

![客户端，输入值（表单）和模型验证之间的差异](https://www.lucidchart.com/publicSegments/view/5461f867-ae1c-44a4-b565-6f780a00cf27/image.png)
