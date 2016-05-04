# PHP Guide

---

## PHP规范

---

## 文档目录

1. [基本代码规范](#1-基本代码规范)
2. [注释规范](#2-注释规范)
3. [良好实践](#3-良好实践)

---

## 1 基本代码规范

> 基本代码规范以现有行业的主流规范**PHP-FIG PSR**规范为主，遇到CodeIgniter框架不能绕过的地方（如model命名等）则用CodeIgniter框架建议规范；<br />
> 
> 参考：<br />
> [PHP-FIG PSR中文版](https://github.com/PizzaLiu/PHP-FIG "PHP-FIG PSR中文版") <br />
> [CodeIgniter PHP开发规范](http://codeigniter.org.cn/user_guide/general/styleguide.html "CodeIgniter PHP开发规范")

### 1.1. PHP标签

PHP代码**必须**使用 `<?php ?>` 长标签或 `<?= ?>` 短输出标签；
**一定不可**使用其它自定义标签。

### 1.2. 字符编码

PHP代码**必须**且只可使用`不带 BOM 的 UTF-8`编码。

> 和 UTF-16 和 UTF-32 不一样，UTF-8 编码格式的文件不需要指定字节序; BOM 会在 PHP 的输出中产生副作用，它会阻止应用程序设置它的头信息。

### 1.3. 文件

- 所有PHP文件**必须**使用`Unix LF (linefeed)`作为行的结束符。

- 所有PHP文件**必须**以一个空白行作为结束。

- 纯PHP代码文件**必须**省略最后的 `?>` 结束标签。

> PHP 结束标签 `?>` 对于 PHP 解析器来说是可选的，但是只要使用了，结束标签之后的空格 有可能会导致不想要的输出，这个空格可能是由开发者或者用户又或者 FTP 应用程序引入的， 甚至可能导致出现 PHP 错误，如果配置了不显示 PHP 错误，就会出现空白页面。基于这个原因， 所有的 PHP 文件将不使用结束标签，而是以一个空行代替。

类文件的命名必须以大写字母开头，其他文件（配置文件，视图，一般的脚本文件等）的命名是全小写。

错误的：

```php
somelibrary.php
someLibrary.php
SOMELIBRARY.php
Some_Library.php

Application_config.php
Application_Config.php
applicationConfig.php
```

正确的：

```php
Somelibrary.php

application_config.php
```

### 1.4. 从属效应（副作用）

一份PHP文件中**应该**要不就只定义新的声明，如类、函数或常量等不产生从属效应的操作，要不就只有会产生从属效应的逻辑操作，但**不该**同时具有两者。

“从属效应”(side effects)一词的意思是，仅仅通过包含文件，不直接声明类、
函数和常量等，而执行的逻辑操作。

“从属效应”包含却不仅限于：生成输出、直接的 `require` 或 `include`、连接外部服务、修改 ini 配置、抛出错误或异常、修改全局或静态变量、读或写文件等。

以下是一个错误的例子，一份包含声明以及产生从属效应的代码：

```php
<?php
// 从属效应：修改 ini 配置
ini_set('error_reporting', E_ALL);

// 从属效应：引入文件
include "file.php";

// 从属效应：生成输出
echo "<html>\n";

// 声明函数
function foo()
{
    // 函数主体部分
}
```

下面是一个范例，一份只包含声明不产生从属效应的代码：

```php
<?php
// 声明函数
function foo()
{
    // 函数主体部分
}

// 条件声明**不**属于从属效应
if (! function_exists('bar')) {
    function bar()
    {
        // 函数主体部分
    }
}
```

### 1.5. 命名空间和类

每个类都独立为一个文件，且命名空间至少有一个层次：顶级的组织名称（vendor name）。

类的命名必须 遵循 `StudlyCaps` 大写开头的驼峰命名规范。

PHP 5.3及以后版本的代码**必须**使用正式的命名空间。

例如：

```php
<?php
// PHP 5.3及以后版本的写法
namespace Vendor\Model;

class Foo
{
}
```

5.2.x及之前的版本**应该**使用伪命名空间的写法，约定俗成使用顶级的组织名称（vendor name）如 `Vendor_` 为类前缀。

```php
<?php
// 5.2.x及之前版本的写法
class Vendor_Model_Foo
{
}
```

### 1.6. 变量

变量的命名**必须**遵循 `camelCase` 小写开头的驼峰命名规范， 并且应该语义明确，能指出该变量的用途。非常短的无意义的变量只应该在 `for` 等循环中作为迭代器使用。

### 1.7. 常量

类的常量中所有字母都**必须**大写，词间以下划线分隔。
参照以下代码：

```php
<?php
namespace Vendor\Model;

class Foo
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
```

### 1.8. 属性

类的属性命名**必须**遵循小写开头的驼峰式 `$camelCase`，同变量的命名方式。

每个属性都**必须**添加访问修饰符。

**一定不可**使用关键字 `var` 声明一个属性。

每条语句**一定不可**定义超过一个属性。

**不要**使用下划线作为前缀，来区分属性是 protected 或 private。

以下是属性声明的一个范例：

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public $foo = null;
}
```

### 1.9. 方法

方法名称**必须**符合 `camelCase()` 式的小写开头驼峰命名规范。

所有方法都**必须**添加访问修饰符。

**不要**使用下划线作为前缀，来区分方法是 protected 或 private。

方法名称后**一定不能**有空格符，其开始花括号**必须**独占一行，结束花括号也**必须**在方法主体后单独成一行。参数左括号后和右括号前**一定不能**有空格。

一个标准的方法声明可参照以下范例，留意其括号、逗号、空格以及花括号的位置。

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function fooBarBaz($arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
```

### 1.10. 行

- 行的长度**一定不能**有硬性的约束。
    软性的长度约束**一定**要限制在120个字符以内，若超过此长度，带代码规范检查的编辑器**一定**要发出警告，不过**一定不可**发出错误提示。

- 每行**不应该**多于80个字符，大于80字符的行**应该**折成多行。

- 非空行后**一定不能**有多余的空格符。

- 空行**可以**使得阅读代码更加方便以及有助于代码的分块。

- 每行**一定不能**存在多于一条语句

### 1.11. 缩进

代码**必须**使用4个空格符的缩进，**一定不能**用 tab键 。

> 备注: 使用空格而不是tab键缩进的好处在于，
> 避免在比较代码差异、打补丁、重阅代码以及注释时产生混淆。
> 并且，使用空格缩进，让对齐变得更方便。

### 1.12. 关键字 以及 True/False/Null

常量 `true` 、`false` 和 `null` 也**必须**全部小写。

> 注意这里与CodeIgniter框架建议规范相反

关键字文档：[http://php.net/manual/zh/reserved.keywords.php](http://php.net/manual/zh/reserved.keywords.php "关键字")

### 1.13. namespace 以及 use 声明

`namespace` 声明后 必须 插入一个空白行。

所有 `use` 必须 在 `namespace` 后声明。

每条 `use` 声明语句 必须 只有一个 `use` 关键词。

`use` 声明语句块后 必须 要有一个空白行。

例如：

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

// ... additional PHP code ...

```

### 1.14. 扩展与继承

关键词 `extends` 和 `implements`**必须**写在类名称的同一行。

类的开始花括号**必须**独占一行，结束花括号也**必须**在类主体后独占一行。

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements \ArrayAccess, \Countable
{
    // constants, properties, methods
}
```

`implements` 的继承列表也**可以**分成多行，这样的话，每个继承接口名称都**必须**分开独立成行，包括第一个。

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements
    \ArrayAccess,
    \Countable,
    \Serializable
{
    // constants, properties, methods
}
```

### 1.15. 方法的参数

参数列表中，每个逗号后面**必须**要有一个空格，而逗号前面**一定不能**有空格。

有默认值的参数，**必须**放到参数列表的末尾。

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function foo($arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
```

参数列表**可以**分列成多行，这样，包括第一个参数在内的每个参数都**必须**单独成行。

拆分成多行的参数列表后，结束括号以及方法开始花括号 必须 写在同一行，中间用一个空格分隔。

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function aVeryLongMethodName(
        ClassTypeHint $arg1,
        &$arg2,
        array $arg3 = []
    ) {
        // method body
    }
}
```

### 1.16. `abstract` 、 `final` 、 以及 `static`

需要添加 `abstract` 或 `final` 声明时， **必须**写在访问修饰符前，而 `static` 则**必须**写在其后。

```php
<?php
namespace Vendor\Package;

abstract class ClassName
{
    protected static $foo;

    abstract protected function zim();

    final public static function bar()
    {
        // method body
    }
}
```

### 1.17. 方法及函数调用

方法及函数调用时，方法名或函数名与参数左括号之间**一定不能**有空格，参数右括号前也 **一定不能**有空格。每个逗号前**一定不能**有空格，但其后**必须**有一个空格。

```php
<?php
bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
```

参数**可以**分列成多行，此时包括第一个参数在内的每个参数都**必须**单独成行。

```php
<?php
$foo->bar(
    $longArgument,
    $longerArgument,
    $muchLongerArgument
);
```

### 1.18. 控制结构

控制结构的基本规范如下：

- 控制结构关键词后**必须**有一个空格。
- 左括号 `(` 后**一定不能**有空格。
- 右括号 `)` 前也**一定不**能有空格。
- 右括号 `)` 与开始花括号 `{` 间**一定**有一个空格。
- 结构体主体**一定**要有一次缩进。
- 结束花括号 `}` **一定**在结构体主体后单独成行。

每个结构体的主体都**必须**被包含在成对的花括号之中，
这能让结构体更加结构话，以及减少加入新行时，出错的可能性。

### 1.19. `if` 、 `elseif` 和 `else`

标准的 `if` 结构如下代码所示，留意 括号、空格以及花括号的位置，
注意 `else` 和 `elseif` 都与前面的结束花括号在同一行。

```php
<?php
if ($expr1) {
    // if body
} elseif ($expr2) {
    // elseif body
} else {
    // else body;
}
```

**应该**使用关键词 `elseif` 代替所有 `else if` ，以使得所有的控制关键字都像是单独的一个词

### 1.20. `switch` 和 `case`

标准的 `switch` 结构如下代码所示，留意括号、空格以及花括号的位置。
`case` 语句**必须**相对 `switch` 进行一次缩进，而 `break` 语句以及 `case` 内的其它语句都 必须 相对 `case` 进行一次缩进。
如果存在非空的 `case` 直穿语句，主体里必须有类似 `// no break` 的注释。

```php
<?php
switch ($expr) {
    case 0:
        echo 'First case, with a break';
        break;
    case 1:
        echo 'Second case, which falls through';
        // no break
    case 2:
    case 3:
    case 4:
        echo 'Third case, return instead of break';
        return;
    default:
        echo 'Default case';
        break;
}
```

### 1.21. `while` 和 `do while`

一个规范的 `while` 语句应该如下所示，注意其 括号、空格以及花括号的位置。

```php
<?php
while ($expr) {
    // structure body
}
```

标准的 `do while` 语句如下所示，同样的，注意其 括号、空格以及花括号的位置。

```php
<?php
do {
    // structure body;
} while ($expr);
```

### 1.22. `for`

标准的 `for` 语句如下所示，注意其 括号、空格以及花括号的位置。

```php
<?php
for ($i = 0; $i < 10; $i++) {
    // for body
}
```

### 1.23. `foreach`

标准的 `foreach` 语句如下所示，注意其 括号、空格以及花括号的位置。

```php
<?php
foreach ($iterable as $key => $value) {
    // foreach body
}
```

### 1.24. `try`, `catch`

标准的 `try catch` 语句如下所示，注意其 括号、空格以及花括号的位置。

```php
<?php
try {
    // try body
} catch (FirstExceptionType $e) {
    // catch body
} catch (OtherExceptionType $e) {
    // catch body
}
```

### 1.25. 逻辑操作符

逻辑操作符的前后都**必须**要有一个空格，但 `!` 操作符只需在后面加一个空格即可。

```php
if ($foo || $bar)
if ($foo && $bar)
if (! is_array($foo))
```

### 1.26. 字符串

字符串使用单引号引起来，当字符串中有变量时使用双引号，并且使用大括号将变量包起来。 另外，当字符串中有单引号时，也应该使用双引号，这样就不用使用转义符。

```php
'My String'
"My string {$foo}"
"SELECT foo FROM bar WHERE baz = 'bag'"
```

### 1.27. 闭包

闭包声明时，关键词 `function` 后以及关键词 `use` 的前后都**必须**要有一个空格。

开始花括号**必须**写在声明的同一行，结束花括号**必须**紧跟主体结束的下一行。


参数列表和变量列表的左括号后以及右括号前，**必须不能**有空格。

参数和变量列表中，逗号前**必须不能**有空格，而逗号后**必须**要有空格。

闭包中有默认值的参数**必须**放到列表的后面。


标准的闭包声明语句如下所示，注意其 括号、逗号、空格以及花括号的位置。

```php
<?php
$closureWithArgs = function ($arg1, $arg2) {
    // body
};

$closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {
    // body
};
```

参数列表以及变量列表**可以**分成多行，这样，包括第一个在内的每个参数或变量都**必须**单独成行，而列表的右括号与闭包的开始花括号**必须**放在同一行。

以下几个例子，包含了参数和变量列表被分成多行的多情况。

```php
<?php
$longArgs_noVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) {
   // body
};

$noArgs_longVars = function () use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_longVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_shortVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use ($var1) {
   // body
};

$shortArgs_longVars = function ($arg) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};
```

注意，闭包被直接用作函数或方法调用的参数时，以上规则仍然适用。

```php
<?php
$foo->bar(
    $arg1,
    function ($arg2) use ($var1) {
        // body
    },
    $arg3
);
```

### 1.29. 类的层次结构

此处的“类”泛指所有的class类、接口、traits可复用代码块以及其它类似结构。


一个完整的类名需具有以下结构:

    \<命名空间>(\<子命名空间>)*\<类名>

    1. 完整的类名**必须**要有一个顶级命名空间，被称为 "vendor namespace"；

    2. 完整的类名**可以**有一个或多个子命名空间；

    3. 完整的类名**必须**有一个最终的类名；

    4. 完整的类名中任意一部分中的下划线都是没有特殊含义的；

    5. 完整的类名**可以**由任意大小写字母组成；

    6. 所有类名都**必须**是大小写敏感的。

例子

下表展示了符合规范完整类名、命名空间前缀和文件基目录所对应的文件路径。

| 完整类名    | 命名空间前缀   | 文件基目录           | 文件路径
| ----------------------------- |--------------------|--------------------------|-------------------------------------------
| \Acme\Log\Writer\File_Writer  | Acme\Log\Writer    | ./acme-log-writer/lib/   | ./acme-log-writer/lib/File_Writer.php
| \Aura\Web\Response\Status     | Aura\Web           | /path/to/aura-web/src/   | /path/to/aura-web/src/Response/Status.php
| \Symfony\Core\Request         | Symfony\Core       | ./vendor/Symfony/Core/   | ./vendor/Symfony/Core/Request.php
| \Zend\Acl                     | Zend               | /usr/includes/Zend/      | /usr/includes/Zend/Acl.php

---

## 2 注释规范

通常情况下，**必须**写 DocBlock 风格的注释，写在类、方法和属性定义的前面，可以被 IDE 识别，方便生成API文档，这不仅可以向新加入开发的人员描述代码的流程和意图， 而且当你几个月后再回过头来看自己的代码时仍能帮你很好的理解。

> 注释规范参考：[phpDocumentor标签文档](https://www.phpdoc.org/docs/latest/index.html "phpDocumentor标签文档")

类注释：

```php
/**
 * Super Class
 *
 * @package     Package Name
 * @subpackage  Subpackage
 * @category    Category
 * @author      Author Name
 * @link        http://example.com
 */
class SuperClass {
```

函数/方法注释：

```php
/**
 * Encodes string for use in XML
 *
 * @param   string  $str  Input string
 * @return  string
 */
public function xmlEncode($str)
```

变量/属性注释：

```php
/**
 * Data for class manipulation
 *
 * @var array
 */
public $data = array();
```

单行注释应该和代码合在一起，大块的注释和代码之间应该留一个空行:

```php
// break up the string by newlines
$parts = explode("\n", $str);

// A longer comment that needs to give greater detail on what is
// occurring and why can use multiple single-line comments.  Try to
// keep the width reasonable, around 70 characters is the easiest to
// read.  Don't hesitate to link to permanent external resources
// that may provide greater detail:
//
// http://example.com/information_about_something/in_particular/

$parts = $this->foo($parts);
```

在控制结构的分支中**必须**加上注释以说明运行走向，方便其他人员理解。

```php
if (exp1) {         // 走向1

} elseif (exp2) {   // 走向2

} else {            // 走向3

}
```

---

## 3 良好实践

### 3.1. 公共和保护接口最小化原则

面向对象程序设计的基本点之一是最小化一个类的公共接口。这样做有几个理由：

- 可学习性。要了解如何使用一个类，只需了解它的公共接口即可。公共接口越小，类越容易学习。

- 减少耦合。当一个类的实例向另一个类的实例或者直接向这个类发送一条消息时，这两个类变 得耦合起来。最小化公共接口意味着将耦合的可能降到最低。
更大的灵活性。这直接与耦合相联系。一旦想改变一个公共接口的成员函数的实现方法，如你可能想修改成员函数的返回值，那么你很可能不得不修改所有调用了该成员函数的代码。公共接口越小，封装性就越大，代码的灵活性也越大。

- 尽力使公共接口最小化这一点明显地很值得你的努力，但通常不明显的是也应使被保护接口最小化。基本思想是，从一个子类的角度来看，它所有超类的被保护接口是公共的。任何在被保护接口内的成员函数可被一个子类调用。所以，出于与最小化公共接口同样的理由，应最小类的被保护接口。
- 首先定义公共接口。大多数有经验的开发者在开始编写类的代码之前就先定义类的公共接口。 第一，如果你不知道一个类要完成怎样的服务/行为，你仍有一些设计工作要做。第二，这样做使这个类很快地初具雏形，以便其他有赖于该类的开发者在“真正的”类被开发出来以前至少可以用这个雏形开始工作。第三，这种方法给你提供了一个初始框架，围绕着这个框架你构造类。

### 3.2. 测试维护

- 时间允许，原则上要求写单元测试，且至少达到语句覆盖。

- 清理、整理或优化后的代码要经过审查及测试。

- 代码版本升级要经过严格测试。

### 3.3. 安全性

- 所有用户输入的数据都要经过安全过滤，防止注入或xss跨站攻击等。

- 所有数据在插入数据库之前，均需要进行`addslashes()`处理，以免特殊字符未经转义在插入数据库的时候出现错误。CI默认情况下已经使用了`addslashes()`进行了转义，不必重复进行。如果数据处理必要(例如用于直接显示)，可以使用 `stripslashes()` 恢复，但数据在插入数据库之前必须再次进行转义。

- 在CI框架内文件首行都要写上：`if (! defined('BASEPATH')) exit('No direct script access allowed')`;

### 3.4. 性能约束

在写代码的时候，从头至尾都应该考虑性能问题。这不是说时间都应该浪费在优化代码上，而是我们时刻应该提醒自己要注意代码的效率。

比如：如果没有时间来实现一个高效的算法，那么我们应该在文档中记录下来，以便在以后有空的时候再来实现她。

不是所有的人都同意在写代码的时候应该优化性能这个观点的，他们认为性能优化的问题应该在项目的后期再去考虑，也就是在程序的轮廓已经实现了以后。

一些良好性能写法如：

- 单引号中，任何变量(`$var`)、特殊转义字符(如“`\t \r \n`”等)不会被解析，因此PHP的解析速度更快，转义字符仅仅支持“`\`’”和“`\\`”这样对单引号和反斜杠本身的转义；双引号中，变量(`$var`)值会代入字符串中，特殊转义字符也会被解析成特定的单个字符，还有一些专门针对上述两项特性的特殊功能性转义，例如“`\$`”和“`{$array['key']}`。这样虽然程序编写更加方便，但同时PHP的解析也很慢；在绝大多数可以使用单引号的场合，禁止使用双引号。

- 数组中，如果下标不是整型，而是字符串类型，请务必用单引号将下标括起，正确的写法为`$array['key']`，而不是`$array[key]`，因为不正确的写法会使PHP解析器认为key是一个常量，进而先判断常量是否存在，不存在时才以“`key`”作为下标带入表达式中，同时出发错误事件，产生一条Notice级错误。

- 如非必要，能用字符操作函数绝不用正则表达式。

- 自增自减，操作符置于变量前面，如`++$i`, `$i++`会多一步赋值临时变量操作。

- 尽量用php原生的函数，google或百度后确实没有原生函数能实现目的，再自己封装。

- 检测变量是否设置，用`isset()`是最快的，要多注意php函数的时间复杂度,尽量用最佳的函数达到目的。

- 不要在循环中调用函数或赋值，如`for（$i, $i < count($arr), ++$i）`每次循环都会count一次。

### 3.5. 不做无用功

- 当时间不是很足时，如非必要，不要大范围的重构代码；

- 在代码量不是很大的时候，不要滥用设计模式，因为这有时会导致性能的下降；
