# PHP Guide

---

## PHP规范指南

---

## 文档目录

1. [PSR规范](#1-PSR规范)
2. [CI规范](#2-CI规范)

## 1. PSR规范
> [PHP-FIG PSR中文版](https://github.com/PizzaLiu/PHP-FIG/edit/master/PSR-1-basic-coding-standard-cn.md "PHP-FIG PSR中文版")

### 1.1. PSR-1 基本代码规范

#### 1.1.1. PHP标签

PHP代码**必须**使用 `<?php ?>` 长标签 或 `<?= ?>` 短输出标签；
**一定不可**使用其它自定义标签。

#### 1.1.2. 字符编码

PHP代码**必须**且只可使用`不带BOM的UTF-8`编码。

#### 1.1.3. 从属效应（副作用）

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

#### 1.1.4. 命名空间和类

命名空间以及类的命名必须遵循 [PSR-4][]。

根据规范，每个类都独立为一个文件，且命名空间至少有一个层次：顶级的组织名称（vendor name）。

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
#### 1.1.5. 类的常量、属性和方法

此处的“类”指代所有的类、接口以及可复用代码块（traits）

## 2. CI规范

