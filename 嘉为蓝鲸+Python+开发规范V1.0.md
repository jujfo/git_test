# Python 开发规范

## PEP8 规范
### 【必须】PEP8 规范

PEP8 python 编码规范：<https://www.python.org/dev/peps/pep-0008/>

- Eclipse 中配置 PEP8 代码提示

    将 PyDev 升级到高于 2.3.0 版本，打开 `Window > Preferences > PyDev > Editor > Code Analysis > pep8.py` 设置即可

- PyCharm 配置 PEP8 代码提示

    直接在右下角调整 Highlighting Level 为 Inspections 就能自动 PEP 8 提示

- 建议修改在使用的 IDE 中修改 PEP8 的每行字数不超 79 字符规范，可修改为 Django 建议的 119 字符

### Flake8 推荐配置

```ini
[flake8]
ignore =
    ;W503 line break before binary operator
    W503,

max-line-length = 120
max-complexity = 25

; exclude file
exclude =
    .tox,
    .git,
    __pycache__,
    build,
    dist,
    *.pyc,
    *.egg-info,
    .cache,
    .eggs
```

## 编码规范
### 【必须】命名规则

1. 包名、模块名、局部变量名、函数名: **全小写+下划线式驼峰**

    示例：`this_is_var`

2. 全局变量: **全大写+下划线式驼峰**

    示例：`GLOBAL_VAR`

3. 类名: **首字母大写式驼峰**

    示例：`ClassName()`

### 变量命名规范

无论是命名变量、函数还是类，都可以使用很多相同的原则。我们可以把名字当做一条小小的注释，尽管空间不算很大，但选择一个好名字可以让它承载很多信息。
所以，命名的关键思想是“把信息装入名字中”，主要有以下一些技巧:

- 选择专业的词。
- 避免泛泛的名字（或者说要知道什么时候可以使用泛泛的名字）。
- 用具体的名字代替抽象的名字。
- 使用前缀或者后缀给名字附带更多信息。
- 决定名字的长度。
- 利用名字的格式来表达含义。
- 使用避免误解的名字。

#### 选择专业的词

“把信息装入名字中”包括要选择非常专业的词，并且避免使用“空洞”的词。

```python

# 例子 1
def get_size():
    # ...


# 例子 2
class Thread(object):
    def stop():
        # ...
```

例子 1 中， `get` 这个词非常不专业，可能是从本地缓存获取、从数据库获取、从远程数据源获取，更专业的名字应该是 `acquire`、`query`、`fetch` 等。 `size` 也没有承载很多信息，可能表示高度、数量、空间，更专业的词应该是 `height`、`num_nodes`、`memory_bytes` 等。

例子 2 中，`stop` 这个名字也不够专业，如果是个重量级操作，不可恢复，不如改为 `kill`；如果可以恢复，可以改为 `pause`，这样也许会有对应的 `resume` 方法。

选择专业的名字，一般也更有表现力，下面更多的例子，可能适合你的语境。

单词 | 更多专业的选择
---|---
send | deliver、dispatch、announce、distribute、route
find | search、extract、locate、recover
start | launch、create、begin、open
make | create、set up、build、generate、compose、add、new

#### 避免像 tmp 和 retval 这样泛泛的名字

使用像 `tmp`、`retval`、`foo` 这样的名字往往是“我想不出名字”的托辞，好的名字应该描述变量的目的或者它所承载的值。

例如 `tmp` 只应该应用于短期存在且临时性为其主要存在因素的变量。一般情况下，需要使用时最好带上类型，如 `tmp_file`。

另外，大家一般使用 `i`、`j`、`k`、`index` 做索引和循环迭代器。尽管这些名字很空泛，但是大家一看就知道他们的意思。不过有时候，适当优化会有更好效果。
如：

```python
for i in clubs:
    for j in i.members:
        for k in j.users:
            ...
```

把 `i`、`j`、`k` 改为 `ci`、`mi`、`ui` ，使迭代元素的首字符和数据的一致会更清晰，并且不易混淆。

#### 用具体的名字代替抽象的名字

在给变量、函数还是类命名时，要把它描述得更具体而不是更抽象。

例如 `def run_locally()` ，只能从名字看出来是要本地运行使用，但是不知道它的具体作用。如果是使用本地数据库，可以改为 `def use_local_database()` 。

#### 使用前缀或者后缀给名字附带更多信息

如果你的变量是一个度量的话，最好把名字带上单位。例如 `start_secs`、`size_md`、`max_kbps` 等。

这种给名字附带额外信息的技巧不限于单位，对于这个变量存在危险或者意外的时候都应该采用。下面有更多的例子可以参考。

背景 | 变量名 | 更好的名字
---  |  ---   | ---
一个“纯文本”的密码，需要加密后存储 | password | text_password
一条用户输入的注释，需要转义后才能用于显示 | comment | unescaped_comment
已转换成 UTF-8 格式的 html | html | html_utf8

#### 决定名字的长

- 在小的作用域使用短的名字
如 `if cond: ...print(m)...`
- “不方便输入”不应该作为避免使用长名字的理由
各种编辑器已经能支持自动补全和快速导入。
- 首字母缩略词和缩写应该是通用的
如 `doc` 可以代替 `document`，但是 `BEManager` 没法替换 `BackendManager`，新成员会很难理解。
- 丢掉没用的词
如果名字中的某些单词拿掉后不会损失任何消息，可以直接去掉。如 `convert_to_string` 不如 `to_string` 简短。

#### 利用名字的格式来表达含义

在 Python 中，一般使用驼峰命名表示类，小写字母加下划线用来命名模块、文件、函数、普通变量，使用大写字母加下划线命名全局变量、常量。

#### 使用避免误解的名字

- 推荐使用 `min` 和 `max` 来表示极限。
- 推荐使用 `first` 和 `last` 来表示包含的范围。
- 推荐使用 `begin` 和 `end` 来表示左闭右开的范围。
- 给布尔值命名时加上像 `is`、`has`、`should`、`to` 这样的词或者使用过去式格式的单词。

### 【必须】import 顺序

1. 标准库

2. 第三方库

3. 项目自身的模块

不同类型的库之间空行分隔

> 注：尽量不要引用 `*`，例如 `from xxx import *`

### 【必须】models 内部定义顺序

1. All database fields

2. Custom manager attributes

3. class Meta

4. def \_\_str\_\_()

5. def save()

6. def get_absolute_url()

7. Any custom methods

### 异常捕获处理原则

1. 尽量只包含容易出错的位置，不要把整个函数 try catch

2. 对于不会出现问题的代码，就不要再用 try catch 了

3. 只捕获有意义，能显示处理的异常

4. 能通过代码逻辑处理的部分，就不要用 try catch

5. 异常忽略，一般情况下异常需要被捕获并处理，但有些情况下异常可被忽略，只需要用 log 记录即可，可参考以下代码：

    ![log记录异常](media/ae0d9a23f02553d1900a365c8816e169.png)

### return early 原则

提前判断并 return，减少代码层级，增强代码可读性

### Fat model， thin view

逻辑代码和业务代码解耦分离，功能性函数以及对数据库的操作定义写在 models 里面，业务逻辑写在 view 里面。


### 权限校验装饰器异常抛出问题

建议权限不足时直接抛出异常，可以使用 django 自带的：

`from django.core.exceptions import PermissionDenied`

权限不足时抛出异常 `PermissionDenied`，之后应该返回什么样的页面由 handler 或者中间件去处理。

### 分 method 获取 request 参数问题

一般可以分 method 获取 request 参数，这样能够使代码更可读，且之后修改 method 时不必每个参数都修改。

```python
kwargs = getattr(request, request.method)
business_name = kwargs.get('business_name', '')
template_name = kwargs.get('template_name', '')
```

### 使用数字、常量表示状态

两种的话改为 true/false，多种改为 enum 可读性更好。

### 其他注意问题

1. 【必须】去除代码中的`print`，否则导致正式和测试环境 uwsgi 输出大量信息。

2. 逻辑块空行分隔。

3. 变量和其使用尽量放到一起。

4. 【必须】import 过长，要放在多行的时候，使用括号，不要用 \\ 换行。
```python
from xxx import (
        a,
        b,
        c
    )
```
5. Django Model 定义的 choices 直接定义在类里面。

6. 【必须】参考蓝鲸应用开发框架，把 Models 的初始化代码放到 migrations 里面。

## 日志规范

生产和测试环境中需要日志来记录、跟踪和分析系统的运行状态，但是有太多带有杂讯的日志又会影响跟踪，甚至可能对系统的运行带来影响。

### 日志级别

- ERROR

    ERROR 是最高级别错误，反映系统发生了非常严重的故障，无法自动恢复到正常态工作，必须通知到开发者及时修复。系统需要将错误相关痕迹以及错误细节记录 ERROR 日志中，方便后续人工回溯解决。

- WARNING

    WARNING 是低级别异常日志，反映系统在业务处理时触发了异常流程，但系统可恢复到正常态，下一次业务可以正常执行。但 WARN 级别问题需要开发人员给予足够关注，往往表示有参数校验问题或者程序逻辑缺陷，当功能逻辑走入异常逻辑时，应该考虑记录 WARN 日志。

- INFO

    INFO 日志主要记录系统关键信息，旨在保留系统正常工作期间关键运行指标，开发人员可以将初始化系统配置、业务状态变化信息，或者用户业务流程中的核心处理记录到 INFO 日志中，方便日常运维工作以及错误回溯时上下文场景复现。

- DEBUG

    开发人员可以将各类详细信息记录到 DEBUG 里，起到调试的作用，包括参数信息，调试细节信息，返回值信息等等。其他等级不方便显示的信息都可以通过 DEBUG 日志来记录。

### 日志格式

#### SaaS

开发框架提供了 logger 对象，建议使用 logger 对象记录日志。使用方法如下：

```python
from blueapps.utils.logger import logger         # 普通日志
from blueapps.utils.logger import logger_celery  # celery日志

logger.error('严重错误！')
logger.warning('异常错误')
logger.info('一般信息')
logger.debug('调试信息')

# 希望打印error级别的错误日志时打印调用栈信息
logger.exception('严重错误！')

```

可在开发者中心查询应用的日志，日志内容包括：调用的文件、调用的函数、所在的行号、日志详细信息

#### 后台服务

```bash
时间戳 [模块/包/类名|方法名] 文件名|行号|PID|TID|REQID 日志级别 日志内容
```

其中：

- `时间戳` -  `YYYY-mm-dd HH:MM:SS`
- `PID` - 进程 ID
- `TID` -  线程 ID
- `REQID` - 用于链路追踪的请求 ID

> 根据的不同编程语言的特点，可以对日志格式做适当调整。不存在的字段不必完全相符，意义在于能快速识别产生日志内容的基本情况，缩小问题排查范围

### 日志内容

- 打印日志时，必须记录上下文信息，避免使用固定字符串
  - 错误的例子

    ```bash
    2018-07-11 JOB接口调用失败
    ```

  - 正确的例子

    ```bash
    2018-07-11 JOB接口调用失败 接口名称(manage_proc) 请求参数(xxx=yyy) 返回参数(xxx=yyy) 状态码(403)
    ```

- SaaS 请求日志，建议打印当前请求的用户名，便于问题定位

- 禁止打印敏感信息，如`app_secret`等密钥信息

- 打印日志时，如果日志内容包含参数，请通过 args 传参，而不要直接进行字符串格式化

    ```python
    # Bad
    logger.info("bk_biz_id->[%s] result_table->[%s] refresh consul config success." % (bk_biz_id, result_table.table_id))

    # Good
    logger.info("bk_biz_id->[%s] result_table->[%s] refresh consul config success.", bk_biz_id, result_table.table_id)
    ```

### 日志打印

- 调用外部系统 API 时**必须**打印日志，记录请求参数和返回参数
- 避免重复打印日志，浪费磁盘空间
- 正确使用日志级别，生产环境中禁止输出 debug 日志
- 谨慎记录日志，日志埋点并非越多越好，日志打印过于频繁会产生性能问题
- 避免打印无意义的日志，会对问题排查造成干扰。记录日志时请思考：这些日志真的有人看吗？看到这条日志你能做什么？能不能给问题排查带来好处？


## 代码注释规范
### 【必须】Python 代码注释

1. 方法必须使用标注注释，如果是公有方法或对外提供的 API 相关方法，则最好给出使用样例。

2. Module 注释：在开头要加入对该模块的注释。

3. 普通代码注释应该以 `#` 和单个空格开始。

4. 如果有需要调整或者需要优化的地方，可以使用。

```python
# TODO 这里是注释内容
a = func()
```
   进行注释，格式：'\#'+单个空格+'TODO'+单个空格+注释内容。

5. 方法的返回，如果数据结构比较复杂，则必须要对返回结果的每个属性做解释。

### WEB API 文档规范

一份规范的、易用的 WEB API 文档对于前后端联调及其重要，能够减少沟通成本，提高开发效率。推荐使用[apidoc](http://apidocjs.com/)作为 Web API 文档生成工具。

apidoc 是通过源码中的注释来生成 Web API 文档。因此，apidoc 对现有代码可以无侵入性，并且支持多种编程语言。

#### apidoc 文档规范

一个规范的 apidoc 接口文档必须包含以下标签信息

##### @api

该标签是必填的，只有使用 @api 标签的注释块才会被解析生成文档内容。

###### 格式

```python
"""
@api {method} path [title]
"""
```

|   参数名称    | 描述 |
| ---------- | --- |
| method |  请求方法：`GET`, `POST`, `PUT`, `DELETE` |
| path   |  请求路径（相对路径） |
| `选填` title |  短标题，会被解析成二级导航栏菜单标题 |

###### 示例

```python
"""
@api {GET} /user/:id 获取用户信息
"""
```

##### @apiDescription

对 API 接口进行详细描述

###### 格式

```python
"""
@apiDescription text
"""
```

|   参数名称    | 描述 |
| ---------- | --- |
| text |  API 描述文本，支持多行 |

###### 示例

```python
"""
@apiDescription 根据用户ID获取用户信息
如果ID对应的用户不存在，则返回404
如果ID对应的用户无权限查看，则返回403
"""
```

##### @apiGroup

表示 API 所属分组名称，它会被解析成一级导航栏菜单标题。注意不能是中文，否则会解析错误。

###### 格式

```python
"""
@apiGroup name
"""
```

|   参数名称    | 描述 |
| ---------- | --- |
| name |  API 分组名称 |

###### 示例

```python
"""
@api {GET} /user/:id 获取用户信息
@apiGroup 用户
"""
```

##### @apiName

API 接口标识名称。需要注意的是，在同一个 `@apiGroup` 下，具有相同的 `@apiName`的 `@api` 必须通过 `@apiVersion` 区分，否则后面定义的 `@api` 会覆盖前面定义的 `@api`。

###### 格式

```python
"""
@apiGroup name
"""
```

|   参数名称    | 描述 |
| ---------- | --- |
| name |  API 名称 |

###### 示例

```python
"""
@api {GET} /user/:id 获取用户信息
@apiName GetUser
"""
```

##### @apiParam

定义 API 接口需要的请求参数格式

###### 格式

```python
"""
@apiParam [(group)] [{type}] [field=defaultValue] [description]
"""
```

|   参数名称    | 描述 |
| ---------- | --- |
| `选填` (group)  |  参数分组 |
| `选填` {type}  |  参数类型，包括`{Boolean}`, `{Number}`, `{String}`, `{Object}`, `{String[]}`, ... |
| `选填` {type{size}})  |  可以声明参数范围，例如{string{..5}}， {string{2..5}}， {number{100-999}} |
| `选填` {type=allowedValues}  |  可以声明参数允许的枚举值，例如{string="small","huge"}， {number=1,2,3,99} |
| field |  参数名称 |
| [field] |  声明该参数可选 |
| `选填` =defaultValue |  声明该参数默认值 |
| `选填` description |  声明该参数描述 |

###### 示例

请求参数

```json
{
    "country": "",
    "age": 19,
    "name": {
        "firstname": "",
        "lastname": ""
    },
    "roles": [
        {
            "code": 1,
            "name": "superuser",
        },
        {
            "code": 2,
            "name": "admin",
        }
    ]
}
```

```python
"""
@api {POST} /user/ 创建用户
@apiName CreateUser

@apiParam {String}          country="DE"    地区名称，字符串类型，默认值为"DE"
@apiParam {Integer{1-100}}  [age=18]        年龄，整数类型，取值范围1~100，默认值为18，可选参数
@apiParam {Object}          name            用户名，Object（字典）类型
@apiParam {String}          name.firstname  名字，字符串类型
@apiParam {String}          name.lastname   姓氏，字符串类型
@apiParam {Object[]}        roles           所属角色，Object（字典）类表类型
@apiParam {Integer=1,2,3}   roles.code      角色代码，数值类型，取值为1, 2, 3任意一个
@apiParam {String}          roles.name      角色名称，字符串类型

"""
```

##### @apiSuccess

定义 API 接口请求成功时返回的数据格式

###### 格式

```bash
@apiSuccess [(group)] [{type}] field [description]
```

|   参数名称    | 描述 |
| ---------- | --- |
| `选填` (group)  |  参数分组，默认为`Success 200` |
| `选填` {type}  |  参数类型，包括`{Boolean}`, `{Number}`, `{String}`, `{Object}`, `{String[]}`, ... |
| field |  参数名称 |
| `选填` description |  声明该参数描述 |

###### 示例

返回参数

```json
{
    "id": 1,
    "country": "",
    "age": 19,
    "name": {
        "firstname": "",
        "lastname": ""
    },
    "roles": [
        {
            "code": 1,
            "name": "superuser",
        },
        {
            "code": 2,
            "name": "admin",
        }
    ]
}
```

```python
"""
@api {POST} /user/ 创建用户
@apiName CreateUser

@apiSuccess {Integer}   id              用户ID，整数类型
@apiSuccess {String}    country         地区名称，字符串类型
@apiSuccess {Integer}   age             年龄，整数类型
@apiSuccess {Object}    name            用户名，Object（字典）类型
@apiSuccess {String}    name.firstname  名字，字符串类型
@apiSuccess {String}    name.lastname   姓氏，字符串类型
@apiSuccess {Object[]}  roles           所属角色，Object（字典）类表类型
@apiSuccess {Integer}   roles.code      角色代码，数值类型
@apiSuccess {String}    roles.name      角色名称，字符串类型

"""
```

##### @apiSuccessExample

API 接口请求成功时返回样例数据。若返回参数结构和含义较为简单，则`@apiSuccess`和`@apiSuccessExample`任取其一定义即可。

###### 格式

```bash
@apiSuccessExample [{type}] [title]
                   example
```

|   参数名称    | 描述 |
| ---------- | --- |
| `选填` type  |  返回参数格式 |
| `选填` title  |  返回样例短标题 |
| example |  详细返回数据，支持多行 |

###### 示例

```python
"""
@api {POST} /user/ 创建用户
@apiName CreateUser

@apiSuccessExample {json} Success-Response:
    HTTP/1.1 200 OK
    {
        "id": 1,
        "country": "",
        "age": 19,
        "name": {
            "firstname": "",
            "lastname": ""
        },
        "roles": [
            {
                "code": 1,
                "name": "superuser",
            },
            {
                "code": 2,
                "name": "admin",
            }
        ]
    }
"""
```

##### 完整示例

```python
"""
@api {POST} /user/ 创建用户
@apiName CreateUser
@apiGroup User

@apiParam {String}          country="DE"    地区名称，字符串类型，默认值为"DE"
@apiParam {Integer{1-100}}  [age=18]        年龄，整数类型，取值范围1~100，默认值为18，可选参数
@apiParam {Object}          name            用户名，Object（字典）类型
@apiParam {String}          name.firstname  名字，字符串类型
@apiParam {String}          name.lastname   姓氏，字符串类型
@apiParam {Object[]}        roles           所属角色，Object（字典）类表类型
@apiParam {Integer=1,2,3}   roles.code      角色代码，数值类型，取值为1, 2, 3任意一个
@apiParam {String}          roles.name      角色名称，字符串类型

@apiSuccess {Integer}   id              用户ID，整数类型
@apiSuccess {String}    country         地区名称，字符串类型
@apiSuccess {Integer}   age             年龄，整数类型
@apiSuccess {Object}    name            用户名，Object（字典）类型
@apiSuccess {String}    name.firstname  名字，字符串类型
@apiSuccess {String}    name.lastname   姓氏，字符串类型
@apiSuccess {Object[]}  roles           所属角色，Object（字典）列表类型
@apiSuccess {Integer}   roles.code      角色代码，数值类型
@apiSuccess {String}    roles.name      角色名称，字符串类型

@apiSuccessExample {json} Success-Response:
    HTTP/1.1 200 OK
    {
        "id": 1,
        "country": "",
        "age": 19,
        "name": {
            "firstname": "",
            "lastname": ""
        },
        "roles": [
            {
                "code": 1,
                "name": "superuser",
            },
            {
                "code": 2,
                "name": "admin",
            }
        ]
    }

"""
```


#### django API 注释规范

##### Django Views

对于提供给前端使用的 views 函数，直接把 apidoc 作为函数注释，例如：

```python

def create_user(request):
    """
    @api {POST} /user/ 创建用户
    @apiName CreateUser
    @apiGroup 用户
    """

    # 创建用户的逻辑
    pass

```

##### DRF Viewset

对于使用 Django Rest Framework 提供的接口，将 apidoc 写在对应的 action 函数中，例如：

```python
class UserViewSet(viewsets.ViewSet):

    def create(self, request):
        """
        @api {POST} /user/ 创建用户
        @apiName CreateUser
        @apiGroup 用户
        """

        # 创建用户的逻辑
        pass
```

