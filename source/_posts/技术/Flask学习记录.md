---
title: Flask 学习汇总
date: '2023/1/2 16:09:25'
categories:
  - 技术
---

## 写在前面

本篇文章涵盖了Python Flask从基础开发环境搭建到应用上云的全过程，囊括了`Flask基础`，`进价知识`，`jinjia2模板语法`，此外还插入了`Pipenv环境使用`，`Falsk前后端视图分离`，以及笔者学习过程中使用到的`Flask-SQLAlchemy`和`flask_login`这俩个flask扩展库等等的内容，涵盖齐全，但由于篇幅所限适合回顾或是了解flask大致走向所用。(`Flask-SQLAlchemy`部分没写，实在是太多了，可以看到文章的创建日期是1月2日事实上我实在1月23日发布的文章，由此可见……)

文章前半部分是笔者看过《FlaskWeb开发实战：入门、进阶与原理解析》_李辉这本书后回顾时边看边挑选摘出的内容，后半部分是笔者查看这段时间学习时的练习项目中的关键代码结合所学知识整理而出。在此强烈向大家推荐这本书，并向李辉作者致谢！

@[TOC](文章标题名)


[TOC]

## 环境准备

Python
    Pipenv

### Pipenv

说实话博主在开始学习Flask之前一直使用Pycharm和pip，像管理虚拟环境这些都是Pycharm来管理的，已经有把我养废的趋势。这次准备自己手动尝试管理，这里就在介绍Flask之前先介绍一下Pipenv

#### Pipenv介绍

首先要了解Pipenv是什么呢？简单来说它是pip的升级版，在默认使用pip时我们的工作方式一般是`pip + virtualenv + requirements.txt`，Pipenv能帮我们解决默认方式的弊端,具体来说：Pipenv呢它是pip，PIPfile和virtualenv的结合体可以帮我们更加方便地管理包和包依赖以及虚拟环境的安装，使用它能够使我们实现高效的Python项目开发流。

#### 安装

> 使用`pip install pipenv`直装就好

#### 创建虚拟环境

1. 进入项目的根目录
2. 执行`pip install`命令
   
   > `注意：`1. 在默认情况下，pipenv会统一管理所有的虚拟环境。在Windows系统中，虚拟环境文件夹会在C:\Users\用户名\.virtualenvs\目录下创建，而在Linux或MASOS系统中会在~/.local/share/virtualenvs/目录下创建。虚拟环境文件夹的目录名称形式为“当前项目目录名+一串随机字符，比如helloflask-5Pa0ZfZw”
   > 我们可以通过设置环境变量PIPENV_VENV_IN_PROJECT这时名为.venv的虚拟环境将在项目根目录被创建。
   > 2. 你可以通过`--three`和`two`选项来声明虚拟环境中使用的Python版本（分别对应Python3和Python2），或是使用`--python`选项指定具体的版本号。同时要哦确保对应版本的Python已经安装在电脑中。
   > 3. 使用`pipenv shell`命令显式地激活虚拟环境。当你执行`pipenv shell`或是`pipenv run`命令时，Pipenv会从项目目录下的.env文件中加载环境变量，而且当你使用Linux系统时激活成功后命令行提示符前会添加虚拟环境的名称
   > 4. 除了使用上述的`pipenv shell`命令外，Pipenv还提供了一个`pipenv run`命令，这个命令允许你不显示激活虚拟环境即可在当前项目的虚拟环境中执行命令，比如`pipenv run python hello.py`这会使用虚拟环境中的Python解析器。事实上，这种方式是更推荐的做法，因为它可以让你在执行操作时不用关心自己是否激活了虚拟环境，当然最终还是按你偏爱的用法操作了。

#### 管理依赖

在创建虚拟环境时，如果项目根目录下没有Pipfile文件，`Pipenv install`命令还会在项目文件夹根目录下创建Pipfile和Pipfile.lock文件，前者用于记录项目依赖包列表，而后者记录了固定版本的详细依赖安装包列表。当我们使用Pipenv安装删除更新依赖包时，Pipfile以及Pipfile.lock会自动更新，这比pip使用reqirements.txt，需要我们手动更新方便多了。

### 安装Flask

`pipenv install flask`Pipenv会帮我们自动管理虚拟环境，无论你是否激活虚拟环境这条命令都会将flask装入虚拟环境中

### 安装python-dotenv管理环境变量

`pipenv install python-dotenv`
具体环境变量管理知识详见下文

### 安装watchdog获取更好的调试体验

默认会使用Werkzeng内置的stat重载器，他的缺点是耗电严重且准确性一般，为了获取更优秀的体验，我们需要安装Watchdog来监测文件变动，安装后自动生效。
`pipenv install watchdog --dev`因为这个包只在开发时会用到，所以我们在安装命令后添加了一个--dev选项，这用来把这个包声明为开发依赖。在Pipfile文件中，这个包会被添加到dev-packages部分。

## Flask基础

> 这部分记录flask使用基础

### flask基本模式示例

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def index():
    return '<h1>hello,World!</h1>'
​```python
>传入Flask构造方法的第一个参数是模块或包的名称，我们使用特殊变量__name__
>
### 一个视图绑定多个URL
```

@app.rout('/hi')
@app.rout('/hello')
def say_hello():
    ......

```
### 动态路由
在路由定义中使用`<变量名>`,Flask在处理请求时会把变量传入视图函数，我们可以添加参数来获取这个变量值
​```python
@app.route('/greet/<name>')
def greet(name):
    return ......
```

### 启动开发服务器

使用命令`flask run`可以启动内置的开发服务器，也可以使用`app.run()`方法

#### 自动发现程序的规则

使用上述命令后：

1. flask会从当前目录寻找app.py和wsgi.py模块，并从中寻找名为app或application的程序
2. 从环境变量FLASK_APP对应值寻找名为app或者application的程序实例
   在没有设置FLASK_APP时你需要将程序的主模块命名为app.py或者你使用环境变量指定需要搜寻的其他名称
   
   #### 管理环境变量
   
   如果你安装了python-dotenv，那么使用`flask run`或者其他命令时他会自动从.flaskenv文件和.env文件中加载环境变量，具体的优先级是：
   手动设置的环境变量>.env中设置的环境变量>.flaskenv中设置的环境变量

鉴于手动设置环境变量太繁琐且不便于管理，而且开发多个flask程序时需要频繁切换，最好安装python-dotenv使用.env和.flaskenv管理环境变量，其中前者用来存储包含敏感信息的环境变量，如账户，密码，秘钥之类。

.env和.flaskenv具体书写格式为:键值对形式，每行一个，'#'标注注释，如

```
#秘钥
SECRET_KEY=089de6ba-7874-11ed-8134-e4fd453f23ec
# FLASK_ENV 变量代表运行时的环境时开发环境(development)还是生产环境(production)
FLASK_DEBUG=1
# FLASK_DEBUG 变量代表是否开启调试器开启1关闭0,开发模式下默认开启,不建议手动操作此项目
# FLASK_DEBUG=1
# FLASK_APP 变量代表执行flask run命令时要运行的应用文件
FLASK_APP=run.py
# FLASK_RUN_HOST 变量代表运行的host 0.0.0.0代表外部计算机访问/127.0.0.1代表本机访问
FLASK_RUN_HOST=0.0.0.0
# FLASK_RUN_PORT 变量代表监听的端口默认5000
FLASK_RUN_PORT=5000
```

注意不要把.env上传到GIt仓库中哦。

### 启动选项-扩展语法

1. 使用`--host=0.0.0.0`指定Web服务器对外可见
   
   ```
   flask run --host=0.0.0.0
   ```
2. 使用`--port=端口值`改变5000的默认端口
   
   ```
   flask run --port=80
   ```

### Flask Shell

开发Flask程序时我们并不会直接使用python命令启动的Python Shell而是使用flask shell命令：在终端中执行`flask shell`命令启动flask shell

### Flask扩展

flask只提供基础的功能，我们需要各种其他的扩展，因为Flask扩展编写有一些约定，他们一般命名为`flask_扩展名`，且一般来说扩展会提供一个扩展类，实例化这个类，并传入我们创建的程序实例app作为参数，即可完成初始化过程

### 项目配置

配置变量实际上就是一些大写形式的Python变量，可以避免在程序中以硬编码的形式设置程序

> `注意`：配置的名称必须全是大写形式，小写的变量将不会被读取

这些变量通过Flask对象的app.congig属性作为统一的接口,有多种方式操作他们
存:

1. `app.config["NAME"]` = 'Peter'
2. `app.congig.update{NAME = True,SECRET_KEY='dsfasf@3dfa'}`
3. 将配置变量保存到单独的Python脚本，JSON格式的文件或者Python类中，config对象提供了相应的方法来导入配置，具体有：
   取:
   取出的方式与操作字典类似
    `value = app.config['NAME']`

### url_for()函数

使用url_for()函数可以避免使用硬编码的方式书写路由（URL）规则

```python

```

如果要获取绝对路径需要设置`_external`参数设为True

### 自定义Flask命令

通过特殊装饰器`app.cli.command()`装饰器

```python
@app.cli.command()
def hello():
    click.echo('Hello,World!')
```

上述注册的命令即hello，可以使用`flask hello`命令触发函数。作为提单，你也可以在`app.cli.command()`装饰器中传入参数来设置命令名称

```python
@app.cli.command('say-hello')
def hello():
    click.echo('Hello,World!')
```

### 模板与静态文件

默认情况下flask会读取根目录中的`templates`文件夹作为HTML模板文件夹，而今天资源则会在根目录下`static`文件夹中获取
HTML中引用static文件夹中静态资源时需要使用url_for()函数
`url_for('admin_view.static', filename='css/add.css')`

## Flask进阶

### Reques对象

我们首先使用导入
`from flask import Flask requeat`

#### 使用Request的属性获取请求的URL

假设请求的URL是http://helloftask.com/hello?name=Grey 当Flask接收到请求后，请求对象会提供多个属性来获取URL的各部分，以下是常用属性表：
|    属性    |    值    |
|-----|-----|
|path    |    u'/hello'    |
|full_path    |u'hello?name=Grey'|
|host        |u'helloftask.com'|
|host_url    |u'http://helloftask.com/' |
|base_url    |u'http://helloftask.com/hello' |
|url        |u'http://helloftask.com/hello?name=Grey' |
|url_root    |http://helloftask.com/ |

#### Request对象常用的属性和方法

| 属性/方法                                                    | 说明                                                                                                                                        |
| -------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| arg                                                      | 解析后的查询字符串，可通过字典方式获取键值，如果想要获取未解析的原生字符串，可以使用query_string属性                                                                                  |
| blueprint                                                | 当前蓝本的名称                                                                                                                                   |
| cookies                                                  | 随请求提交的cookies字典                                                                                                                           |
| data                                                     | 字符串形式的请求数据                                                                                                                                |
| endpoint                                                 | 与当前请求匹配的端点值                                                                                                                               |
| files                                                    | 所有上传文件，可以使用字典的形式获取文件。使用的键为文件input标签中的name属性值作为键获取                                                                                         |
| form                                                     | 包含解析后的表单数据，通过input的name属性值作为键获取                                                                                                           |
| values                                                   | 结合了args和form属性的值                                                                                                                          |
| get_data(cache=True,as_text=False,parse_from_data=False) | 获取请求中的数据，默认读取为字节串，将as_text设置为True则返回值将是解码后的unicode字符串                                                                                     |
| get_json(self,force=False,silent=False,cache=True)       | 作为Json解析并返回数据，如果MIME类型不是JSON，则返回None(除非force设为True)；解析出错则抛出BadRequest异常（如果为开启调试模式，则返回400错误响应），如果silent设为True则返回None；cache设置是否缓存解析后的JSON数据 |
| headers                                                  | 首部字段，以字典形式操作                                                                                                                              |
| is_json                                                  | 通过MIME类型判断是否为JSON数据，返回布尔值                                                                                                                 |
| json                                                     | 包含解析后的JSON数据，内部调用get_json(),通过字典方式获取键值                                                                                                    |
| method                                                   | 请求的HTTP方法                                                                                                                                 |
| referrer                                                 | 请求发起的源，即referer                                                                                                                           |
| scheme                                                   | 请求的URL模式（http或https）                                                                                                                      |
| user_agent                                               | 用户代理，包含了用户的客户端类型，操作系统类型等信息                                                                                                                |

> 其中最长使用的是arg（获取get方法请求传入的数据）form（获取post请求传入的数据）values（同时获取get和post请求获取的数据）get_json(获取JSON格式的数据)

### 处理请求

#### 设置监听的HTTP方法

```python
@app.route('/hello',methods=['GET','POST'])
def ......
```

#### URL处理

在URL中使用变量直接传入会造成安全隐患，我们应该对奇根据我们需要进行转义后再继续使用，Flask提供一些转换器帮我们简化了这个任务例如你可以使用变量<int:year>,Flask在解析这个URL变量时会将其转换为整型数据传入

Flask内置的URL变量转换器
|转换器|说明|
|-----|-----|
|string|不包含斜线的字符串（默认值）|
|int|整型|
|float|浮点数|
|path|包含斜线的字符串，static路由规则中的filename变量就使用了这个转换器|
|any|匹配一系列给定值中的一个元素，具体使用方法为<any(value1,value2,......):变量名>|
|uuid|UUID字符串|

#### 请求钩子

对请求进行预处理和后处理

```python
# 预处理例子
@app.before_request
def do_something():
    pass # 这里的代码会在每个请求处理前执行
```

注意，这些请求都能注册任意多个处理函数，函数名并不要求必须和钩子名称相同，下面是请求钩子的列表

| 钩子                   | 说明                                                |
| -------------------- | ------------------------------------------------- |
| before_first_request | 处理第一个请求之前执行                                       |
| before_request       | 处理每个请求前都要执行                                       |
| after_request        | 如果没有未处理的异常抛出，会在每个请求结束后运行                          |
| treardown_request    | 即使有未处理异常抛出，也会在每个请求结束后执行，如果发生异常，会传入异常对象作为参数到注册的函数中 |
| after_this_request   | 在视图内注册一个函数，会在这个请求结束后运行                            |

#### 生成响应

Flask的视图函数必须有返回，否则报错！
视图函数可以返回最多由三个元素组成的元组：响应主体、状态码、首部字段
其中普通的请求只需要响应主体就行：`return 'hello world'`
指定状态码：`return 'hello world,201'`
修改或附加某个首部字段：重定向目标URL`return '',302,{'Location':'http:www.exmple.com'}`

#### 重定向

使用函数`redirect(url)`

```Python
@app.route(...)
def hello():
    ...
    return redirect('http:...')
```

其中默认状态码为302，即临时重定向，可以通过在函数中传入第二个参数或者使用code关键字传入替代默认值
如果需要在程序内重定向到其他视图，只需在redirect中使用url_for()生成目标URL就行

```python
@app.route(...)
def hello():
    ...
    return redirect(url_for('hi')) # 重定向到/hi
```

#### 错误响应

Flask会自动处理常见的错误响应，可以通过Flask提供的abort()函数抛出异常，about()前不需要return，且about()后的代码不会被执行

```python
@app.route('/404')
def not_found():
    about(404)
```

#### jsonify()函数

jsonify包装了json模块的内容，且会自动生成响应对象且设置正确的MIME类型，你可以传入普通参数，也可以传入关键字参数，还可以像使用dumps()一样传入更加直观的字典或元组

```python
from flask import jsonify
@app.rout('/foo')
def foo():
    return jsonify({'name':"Grey Li"})
    # 或者使用
    return jsonify(name='Grey')
    # 或者使用
    return jsonify("{'name':'Grey Li'}")
    # 你还可以附加状态码来自定义响应类型
    return jsonify({'name':"Grey Li"}),500
```

### 使用Cookie

#### 设置cookie

发送cookie需要用到Response类提供的set_cookie()方法，下面简单罗列一下此方法所支持的常用参数

| 属性       | 说明                                    |
| -------- | ------------------------------------- |
| key      | cookie创建的键名称                          |
| value    | cookie的值                              |
| max_age  | cookie被保存的时间数，单位秒，默认在用户会话结束（关闭浏览器）时过期 |
| expires  | 具体的过期时间，一个datetime对象或UNIX时间戳          |
| path     | 限制cookie只给指定的路径可用，默认为整个域名             |
| domain   | 设置cookie可用的域名                         |
| secure   | 如果为True，只有通过Https才可以使用                |
| httponly | 若果设为True那么禁止客户端javaScript获取cookie     |

```python
from flask import Falsk,make_response
@app.rout('/set/<name>')
def set_cookie(name):
    response = make_response(redirect(url_for('hello')))
    response.set_cookie('name',name)
    return response

# 上面这段示例代码可以看做是简单登录功能的一部分实现
用户登录过后跳转到set_cookie为其设置cookie记录登录转态，然后跳转到正式界面，事实上上面代码漏洞很大，此处仅仅是作为设置cookie的实例
```

当浏览器保存了服务端设置的cookie后，浏览器再次发送到该服务器的请求会自动携带设置的cookie信息

#### 读取cookie

设置好cookie后我们就可以在需要的时候读取cookie了，正如上文所诉，cookie是和请求一起发送的，因此我们可以使用flask的request对象获取cookie
使用`request.cookie.get(p_srt)`方法

```python
from flask import Falsk,request
@app.rout('/hello')
def hello():
    name = request.cookies.get('name')
```

### session：更加安全的cookie

有经验的小伙伴一定知道我们在浏览器中可以查看和修改存储的cookie,有些时候比如我们使用cookie存储用户的登录状态的时候，我们不想让其他人查看和修改，以防恶意用户伪造cookie他人账户，这时我们就需要对敏感的cookie内容进行加密，方便的是flask提供了session对象来将cookie进行加密储存，默认情况下他把数据储存在浏览器上一个名为session的cookie中

要想使用session你需要先为flask设置秘钥,你可以使用`Flask.secret_key`属性或配置环境变量`SECRET_KET`具体做法是`app.secret_key='secret string'`这样做固然可以，但更加安全的做法是将其写入系统环境变量，考虑到不同项目间的干扰我们最好是写入上文所提到的`.env`文件中你应该在`.env`这么写`SECRET_KEY=secret string`然后在脚本程序中使用os提供的`getenv`获取：

```python
import os


# ......


app.secret_key=os.getenv('SECRET_KEY','secret string') 
# 第二个参数是在环境变量中没有获取到情况时使用的默认值，（此处应该报警^-^）
```

有了秘钥接下是写入session 的正式环节了

```python
from falsk import session

@app.route('/login')
def login():
    session['logged_in'] = True # 写入session

    ......
# session中的数据可以像字典一样读取或是使用get方法，区别是前者没有报错后者返回None    
```

### Falsk上下文

## JinJia2

上面我们已经简单说过了flask的模板与静态文件，而flask中渲染网页使用的jinjia2这个库，下面我们来具体说说它的语法,这部分网上有很多讲解，笔者直接粘贴了，打字好累

> 来源版权 | 版权声明：
> 
> 以下jinjia2用法来自博客园博主「浩浩学习」的原创文章，
> 原文链接：`https://www.cnblogs.com/hhaostudy/p/16119959.html`

### 变量

jinja2模板中使用 {% raw %}{{ }}{% endraw %} 语法表示一个变量，它是一种特殊的占位符。当利用jinja2进行渲染的时候，它会把这些特殊的占位符进行填充/替换，jinja2支持python中所有的Python数据类型比如列表、字段、对象等。

使用`{% raw %}{{变量名}}{% endraw %}`包裹的是一个变量
下面是一个简单的实例：

```HTML
# /templates/index.html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
你好{% raw %}{{name}}{% endraw %}
</body>
</html>
```

在Python中我们这样使用

```Python
# ./app.py

......

return render_template("index.html",
                        name="世界"   
                           )
```

其他支持的数据类型

```
<p>this is a dicectory:{% raw %}{{ mydict['key'] }}{% endraw %} </p>
<p>this is a list:{% raw %}{{ mylist[3] }}{% endraw %} </p>
<p>this is a object:{% raw %}{{ myobject.something() }}{% endraw %} </p>
```

### 过滤器

 jinja2中的过滤器

| 过滤器名称       | 说明                     |
| ----------- | ---------------------- |
| safe        | 渲染时值不转义                |
| capitialize | 把值的首字母转换成大写，其他子母转换为小写  |
| lower       | 把值转换成小写形式              |
| upper       | 把值转换成大写形式              |
| title       | 把值中每个单词的首字母都转换成大写      |
| trim        | 把值的首尾空格去掉              |
| striptags   | 渲染之前把值中所有的HTML标签都删掉    |
| join        | 拼接多个值为字符串              |
| replace     | 替换字符串的值                |
| round       | 默认对数字进行四舍五入，也可以用参数进行控制 |
| int         | 把值转换成整型                |

### 循环

#### 迭代列表

```HTML
<ul>
{% for user in users %}
<li>{% raw %}{{ user.username|title }}{% endraw %}</li>
{% endfor %}
</ul>
```

#### 迭代字典

```HTML
<dl>
{% for key, value in my_dict.iteritems() %}
<dt>{% raw %}{{ key }}{% endraw %}</dt>
<dd>{% raw %}{{ value}}{% endraw %}</dd>
{% endfor %}
</dl>
```

当然也可以加入else语句，在循环正确执行完毕后，执行

### 判断 if语句使用

if条件判断语句必须放在{% if statement %}中间，并且还必须有结束的标签{% endif %}。和python中的类似，

可以使用>，<，<=，>=，==，!=来进行判断，也可以通过and，or，not，()来进行逻辑合并操作

```HTML
{% if name==1 %}                    <!--name的值是否等于1-->
<h1>恭喜，您抽中了一等奖！</h1>        <!--name的值等于1，显示本行h1代码-->
{% elif name==2 %}                    <!--name的值是否等于2-->
<h1>恭喜，您抽中了二等奖！</h1>
{% else %}                            <!--name的值是否等于其他-->
<h1>恭喜，您抽中了三等奖！</h1>
{% endif %}                            <!--结束if语句-->
```

在for循环中，jinja2还提供了一些特殊的变量，用以来获取当前的遍历状态：

| 变量             | 描述               |
| -------------- | ---------------- |
| loop.index     | 当前迭代的索引（从1开始）    |
| loop.index0    | 当前迭代的索引（从0开始）    |
| loop.first     | 是否是第一次迭代，返回bool  |
| loop.last      | 是否是最后一次迭代，返回bool |
| loop.length    | 序列中的项目数量         |
| loop.revindex  | 到循环结束的次数（从1开始）   |
| loop.revindex0 | 到循环结束的次数(从0开始）   |

### jinja2的宏

宏类似于Python中的函数，我们在宏中定义行为，还可以进行传递参数，就像Python中的函数一样一样儿的。

在宏中定义一个宏的关键字是macro，后面跟其 宏的名称和参数等

```HTML
{% macro input(name,age=18) %}   # 参数age的默认值为18

 <input type='text' name="{% raw %}{{ name }}{% endraw %}" value="{% raw %}{{ age }}{% endraw %}" >

{% endmacro %}
调用方法也和Python的类似

<p>{% raw %}{{ input('daxin') }}{% endraw %} </p>
<p>{% raw %}{{ input('daxin',age=20) }}{% endraw %} </p>
```

### jinja2的继承和Super函数

jinja2中最强大的部分就是模板继承。模板继承允许我们创建一个基本(骨架)文件，其他文件从该骨架文件继承，然后针对自己需要的地方进行修改。

jinja2的骨架文件中，利用block关键字表示其包涵的内容可以进行修改。

以下面的骨架文件base.html为例：

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    {% block head %}
    <link rel="stylesheet" href="style.css"/>
    <title>{% block title %}{% endblock %} - My Webpage</title>
    {% endblock %}
</head>
<body>
<div id="content">{% block content %}{% endblock %}</div>
<div id="footer">
    {% block  footer %}
    <script>This is javascript code </script>
    {% endblock %}
</div>
</body>
</html>
```

这里定义了四处 block，即：head，title，content，footer。那怎么进行继承和变量替换呢？注意看下面的文件

```HTML
{% extend "base.html" %}       # 继承base.html文件

{% block title %} Dachenzi {% endblock %}   # 定制title部分的内容

{% block head %}
    {% raw %}{{  super()  }}{% endraw %}        # 用于获取原有的信息
    <style type='text/css'>
    .important { color: #FFFFFF }
    </style>
{% endblock %}   
```

其他不修改的原封不同的继承
PS: super()函数 表示获取block块中定义的原来的内容。

## Flask 蓝图

要正式开发一个web项目把所有的路由关系都塞到一个app.py中是显然行不通的，最起码这是不是一个很好的办法，这时候就需要falsk为我们提供的蓝图

使用蓝图大致可以分为三个步骤

1. 创建一个蓝图对象
2. 在这个蓝图对象上进行操作,注册路由,指定静态文件夹 ......
3. 在应用对象上注册这个蓝图对象

下面三段代码分别代表上述三个过程

```Python
admin_view = Blueprint(name='admin_view',
                       import_name=__name__,
                       static_folder="../models/admin/static",
                       static_url_path="/admin_static",
                       template_folder='../models/admin/templates',
                       url_prefix="/admin",
                       )
# 参数分别为“蓝图名称”,"静态文件夹目录","静态文件夹需要映射到的路由","模板文件文件夹"，“为蓝图中所有路由前添加的前缀”
```

使用蓝图 `@admin_view.route('/')`

```Python
# 根目录
@admin_view.route('/')
def root():  # put application's code here

    data = dbModel.User.query.filter_by(id=1).first()
    name = data.name
    head_portrait_id = data.head_portrait
    head_portrait = dbModel.Pic.query.filter_by(id=head_portrait_id).first().data
    head_portrait = base64.b64encode(head_portrait).decode("utf-8")
    rootPath = current_app.config.get('ROOT_PATH')

    data_templates = render_template("admin_data.html", rootPath=rootPath, left_page_name_List=left_page_name_List)

    return render_template("admin_base.html",
                           head_portrait=head_portrait,
                           name=name,
                           rootPath=rootPath,
                           data_templates=data_templates,
                           )
```

## 大型项目构建

```
Flask项目文件分配示意图

项目根目录
|
|——+app(项目文件夹)
|    |
|    |+——api（所有api存放文件夹）
|    |
|    |+——application
|    |    |
|    |    |——app.py(这里真正创建app对象，其他文件从这里调用app)
|    |    |
|    |    |——extensions.py(导入扩展并初始化)
|    |    |
|    |    |——setup.py(这里创建app装配环境变量，路由等等，将app返回)
|    |
|    |+——database
|    |    |
|    |    |+——init_data(存放数据库初始化需要信息)
|    |    |
|    |    |——*.db/.sqlite3(数据库文件)
|    |    |
|    |    |——dbModel.py(数据库模型)
|    |
|    |+——log（日志）
|    |    
|    |+——models(视图模型的静态文件以及模板)
|    |    |
|    |    |+——admin(后台管理视图)
|    |    |    |
|    |    |    |——static
|    |    |    |
|    |    |    |——templates
|    |    |    |
|    |    |    |——images
|    |    |    |
|    |    |    |——fonts
|    |    |    
|    |    |+——login(登录)    
|    |    |    |
|    |    |    |+——static
|    |    |    |
|    |    |    |+——templates
|    |    |    |
|    |    |    |+——images
|    |    |    |
|    |    |    |+——fonts
|    |    |
|    |    |+——pc_user(Pc用户)
|    |    |    |
|    |    |    |+——static
|    |    |    |
|    |    |    |+——templates
|    |    |    |
|    |    |    |+——images
|    |    |    |
|    |    |    |+——fonts
|    |    |    
|    |    |+——public（公用静态或模板）
|    |
|    |——tools(编写的公用工具)
|    |    |
|    |    |+——redis（redis数据库）
|    |    |
|    |    |——initdb.py(初始化数据库数据)
|    |
|    |+——views(视图)
|    |    |
|    |    |——admin.py
|    |    |
|    |    |——login.py
|    |    |
|    |    |——mc_user.py(手机用户)
|    |    |
|    |    |——pc_user.py（Pc用户）
|    |
|    |——config.py（Flask配置文件）
|
|+——install（存放安装辅助脚本，方便用户安装系统）
|
|+——venv（圩您环境）
|
|——.env（敏感环境变量）
|
|——.flaskenv(普通flask环境变量)
|
|——ReadMe.md(项目自述文件)
|
|——run.py（项目启动脚本）
```

下面是具体文件配置
根目录我用root代替，上面图示的app文件夹根据我实际情况开发博客我用的是blogApp

```Python
# root/application/app.py
# 这里真正创建app对象，其他文件从这里调用app
from blogApp.application.setup import create_app
from flask import redirect, url_for

app = create_app()


@app.route("/index")
def root():
    return redirect("/pc")
```

```Python
# root/application/extensions.py
# 存放扩展
from flask_sqlalchemy import SQLAlchemy

# define global extensions in a separate file so that they can be imported from
# anywhere else in the code without creating circular imports
# the proper initialization is made within the `create_app` function
db = SQLAlchemy()
```

```Python
# root/application/setup.py
# 这里创建app装配环境变量，路由等等，将app返回
import os

from flask import Flask
from flask_cors import CORS

from blogApp.application.extensions import db
import blogApp
from blogApp import config


def create_app():
    app: Flask = Flask(import_name=blogApp.__name__)

    print("app.rootpath(app根路径):",app.root_path)
    app.config.from_object(config.DevConfig) # 导入配置

    # 相对路劲太坑了，不玩儿了，上绝对吧
    app.static_folder = os.path.join(app.config.get("ROOT_PATH_OBJECT"), r"blogApp\models\public\static")
    app.template_folder = os.path.join(app.config.get("ROOT_PATH_OBJECT"), r"blogApp\models\public\templates")

    app.secret_key = os.getenv('SECRET_KEY') # 导入秘钥
    CORS(app, resources={r"/*": {"origins": "*"}}{% endraw %})

    # Initialize extensions
    # 初始化数据库
    db.init_app(app)

    with app.app_context():
        # 自动初始化数据库
        from blogApp.tools.initdb import initdb
        initdb()
        # 添加jinji2全局方法
        from blogApp.application.template_global import strlen
        app.add_template_global(strlen, 'strlen')
        # Register Blueprints
        # 注册蓝图
        from blogApp.views.pc_user import pc_user_view
        from blogApp.views.admin import admin_view
        from blogApp.api.pc_user_api import pc_user_api
        from blogApp.views.login import login_view
        app.register_blueprint(pc_user_view)
        app.register_blueprint(login_view)
        app.register_blueprint(admin_view)
        app.register_blueprint(pc_user_api)
        return app
```

```Python
# root/blogApp/database/dbModel.py
# 数据库模型
from blogApp.application.extensions import db
class Article(db.Model):
    '''
    文章
    '''
    id = db.Column(db.Integer, primary_key=True)
    author = db.Column(db.String(80),nullable=False) # 作者
    copyright = db.Column(db.Integer,nullable=False) # 版权
    title = db.Column(db.String(120),nullable=False) # 标题
    outline = db.Column(db.String(255),nullable=False) # 概要
    content = db.Column(db.Text) # 内容
    creatDate = db.Column(db.Date,nullable=False) # 创建日期
    updateDate = db.Column(db.Date,nullable=False) # 更新日期
    album = db.Column(db.Text) # 专辑
    source = db.Column(db.String(5),nullable=False) # 来源
    sort = db.Column(db.Integer,nullable=False) # 分类
    tag = db.Column(db.Text) # 标签
    cover = db.Column(db.Integer,nullable=False) # 封面图
    state = db.Column(db.Integer, nullable=False) # 状态
    readnum = db.Column(db.Integer, nullable=True) # 阅读量

class Drafts(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(120), nullable=False)  # 标题
    content = db.Column(db.Text)  # 内容
    creatDate = db.Column(db.Date, nullable=False)  # 创建日期
    updateDate = db.Column(db.Date, nullable=False)  # 更新日期

class Tag_index(db.Model):
    '''
    标签索引
    '''
    id = db.Column(db.Integer,primary_key=True) # id
    name = db.Column(db.String(80),nullable=False) # 名称

class Sort_index(db.Model):
    '''
    分类索引
    '''
    id = db.Column(db.Integer,primary_key=True) # id
    name = db.Column(db.String(80),nullable=False) # 名称

class Album_index(db.Model):
    '''
    专辑索引
    '''
    id = db.Column(db.Integer,primary_key=True) # id
    name = db.Column(db.String(80),nullable=False) # 名称
    outline = db.Column(db.String(255), nullable=False)  # 概要

class Copyright_index(db.Model):
    '''
    版权索引
    '''
    id = db.Column(db.Integer,primary_key=True) # id
    name = db.Column(db.String(80),nullable=False) # 名称
    link = db.Column(db.String(255), nullable=False) # 链接

class Pic(db.Model):
    '''
    图片
    '''
    id = db.Column(db.Integer,primary_key=True) # id
    name = db.Column(db.String(255), nullable=False) # name
    data = db.Column(db.LargeBinary,nullable=False)  # 封面图
    type = db.Column(db.Text,nullable=False) # 类型,jpg/png
    sort = db.Column(db.Integer,nullable=False) # 文章还是其他(0/1)
    alt = db.Column(db.Text,nullable=False) # 提示信息


class User(db.Model):
    '''
    用户表
    '''
    id = db.Column(db.Integer,primary_key=True) # id
    name = db.Column(db.String(80),nullable=False) # 昵称
    account = db.Column(db.String(120),nullable=False) # 账号
    password = db.Column(db.String(120),nullable=False) # 密码
    email = db.Column(db.String(120)) # 邮箱
    head_portrait = db.Column(db.Integer,nullable=False) # 头像图片id

class Token(db.Model):
    '''
    cookie token表
    '''
    id = db.Column(db.Integer,primary_key=True) # id
    accountId = db.Column(db.Integer) # 对应账户id
    token = db.Column(db.String(64),nullable=False) # token
    creatTime = db.Column(db.DateTime,nullable=False) # 创建时间

class Friends_link(db.Model):
    '''
    友链表
    '''
    id = db.Column(db.Integer,primary_key=True) # id
    name = db.Column(db.String(256),nullable=False) # 标题
    url = db.Column(db.String(256),nullable=False) # 链接
    img = db.Column(db.String(256), nullable=False)  # 头像图片地址
    describe = db.Column(db.Text, nullable=False)  # 描述
    email = db.Column(db.Text, nullable=False)  # 描述
    state = db.Column(db.Integer, nullable=False)  # 状态 0 正常 1 失效  2 申请添加 3 优质友链
    message = db.Column(db.Text, nullable=False)  # 留言
    response_time = db.Column(db.Integer,nullable=False) # 响应时间
    applyDateTime = db.Column(db.DateTime,nullable=False) # 申请时间
    addDataTime = db.Column(db.DateTime,nullable=False) # 添加时间
```

```Python
# root/blogApp/tools/initdb.py
'''
用于初始化database
'''
import datetime

'''
顺序不能改先导入db，之后导入app这个过程创建了app并初始化db，最后调用dbModel（他需要等app，db都创建好了才能用）
'''
from blogApp.application.extensions import db
from blogApp.database import dbModel


import click

#@app.cli.command()
def initdb():
    '''初始化数据库'''
    db.create_all()
    # 默认添加三个分类项目
    if dbModel.Sort_index.query.all() == []:
        sort1 = dbModel.Sort_index(name="技术")
        sort2 = dbModel.Sort_index(name="折腾")
        sort3 = dbModel.Sort_index(name="文章")
        sort4 = dbModel.Sort_index(name="生活")
        db.session.add(sort1)
        db.session.add(sort2)
        db.session.add(sort3)
        db.session.add(sort4)
    # 默认添加几个标签
    if dbModel.Tag_index.query.all() == []:
        tag1 = dbModel.Tag_index(name="Python基础")
        tag2 = dbModel.Tag_index(name="前端")
        tag3 = dbModel.Tag_index(name="后端")
        tag4 = dbModel.Tag_index(name="恋爱")
        db.session.add(tag1)
        db.session.add(tag2)
        db.session.add(tag3)
        db.session.add(tag4)
    # 默认添加几个文章版权信息
    if dbModel.Copyright_index.query.all() == []:
        copyright1 = dbModel.Copyright_index(name="BSD（Berkeley Software Distribution license）", link="https://upimg.baike.so.com/doc/352283-373141.html")
        copyright2 = dbModel.Copyright_index(name="MIT（Massachusetts Institute of Technology）", link="https://opensource.org/licenses/MIT")
        copyright3 = dbModel.Copyright_index(name="Apache Licence 2.0", link="https://www.apache.org/licenses/LICENSE-2.0.html")
        copyright4 = dbModel.Copyright_index(name="GPL（General Public License）", link="https://getrebuild.com/legal/license")
        copyright5 = dbModel.Copyright_index(name="LGPL（Lesser General Public License）", link="https://opensource.org/licenses/lgpl-license")
        copyright6 = dbModel.Copyright_index(name="Mozilla（Mozilla Public License）", link="https://www.mozilla.org/en-US/MPL/2.0/")
        db.session.add(copyright1)
        db.session.add(copyright2)
        db.session.add(copyright3)
        db.session.add(copyright4)
        db.session.add(copyright5)
        db.session.add(copyright6)
    # 默认为Pic添加几张图片
    if dbModel.Pic.query.all() == []:
        # 默认封图
        with open("blogApp/database/init_data/cover.jpeg", "rb") as p:
            data = p.read()
        pic1 = dbModel.Pic(name="文章默认封面图",data=data, type="jpg", sort=0, alt="我为我代言")
        db.session.add(pic1)
        # 默认头像
        with open("blogApp/database/init_data/head_portrait.jpg","rb")as p:
            data = p.read()
        pic2 = dbModel.Pic(name="默认博主头像",data=data, type="jpg",sort=1,alt="头像")
        db.session.add(pic2)
    # 添加管理账户
    if dbModel.User.query.all() == []:
        user = dbModel.User(name="I Speak for myself", account="2061360308",
                                     password="123456", email=None, head_portrait=1)
        db.session.add(user)
    db.session.commit()
    click.echo('Initiali zed database.(数据库初始化完毕)')
    # 添加测试文章数据
    if dbModel.Article.query.all() == []:
        author = "卢澳"
        Article1 = dbModel.Article(author=author,
                                   copyright = 0,
                                   title="测试文章一",
                                   outline="这是测试文章",
                                   content="",
                                   creatDate=datetime.datetime.now(),
                                   updateDate=datetime.datetime.now(),
                                   album=None,
                                   source="原创",
                                   sort=1,
                                   tag=None,
                                   cover=1,
                                   state=0,
                                   readnum=0,
                                   )
        Article2 = dbModel.Article(author=author,
                                   copyright=0,
                                   title="测试文章二",
                                   outline="这是测试文章",
                                   content="",
                                   creatDate=datetime.datetime.now(),
                                   updateDate=datetime.datetime.now(),
                                   album=None,
                                   source="原创",
                                   sort=1,
                                   tag=None,
                                   cover=1,
                                   state=0,
                                   readnum=0,
                                   )
        Article3 = dbModel.Article(author=author,
                                   copyright=0,
                                   title="测试文章三",
                                   outline="这是测试文章",
                                   content="",
                                   creatDate=datetime.datetime.now(),
                                   updateDate=datetime.datetime.now(),
                                   album=None,
                                   source="原创",
                                   sort=1,
                                   tag=None,
                                   cover=1,
                                   state=0,
                                   readnum=0,
                                   )
        Article4 = dbModel.Article(author=author,
                                   copyright=0,
                                   title="测试文章四",
                                   outline="这是测试文章",
                                   content="",
                                   creatDate=datetime.datetime.now(),
                                   updateDate=datetime.datetime.now(),
                                   album=None,
                                   source="原创",
                                   sort=1,
                                   tag=None,
                                   cover=1,
                                   state=0,
                                   readnum=0,
                                   )
        Article5 = dbModel.Article(author=author,
                                   copyright=0,
                                   title="测试文章五",
                                   outline="这是测试文章",
                                   content="",
                                   creatDate=datetime.datetime.now(),
                                   updateDate=datetime.datetime.now(),
                                   album=None,
                                   source="原创",
                                   sort=1,
                                   tag=None,
                                   cover=1,
                                   state=0,
                                   readnum=0,
                                   )
        Article6 = dbModel.Article(author=author,
                                   copyright=0,
                                   title="测试文章六",
                                   outline="这是测试文章",
                                   content="",
                                   creatDate=datetime.datetime.now(),
                                   updateDate=datetime.datetime.now(),
                                   album=None,
                                   source="原创",
                                   sort=1,
                                   tag=None,
                                   cover=1,
                                   state=0,
                                   readnum=0,
                                   )
        db.session.add(Article1)
        db.session.add(Article2)
        db.session.add(Article3)
        db.session.add(Article4)
        db.session.add(Article5)
        db.session.add(Article6)
        db.session.commit()

    # 添加测试友链申请
    if dbModel.Friends_link.query.all() == []:
        apply1 = dbModel.Friends_link(name="XX小站",
                                      url="https://baidu.com",
                                      img="img.com",
                                      describe="测试小站",
                                      email="2061360308@qq.com",
                                      state=2,
                                      message="加下友链",
                                      response_time=5,
                                      applyDateTime=datetime.datetime.now(),
                                      addDataTime=datetime.datetime.now(),
                                      )
        db.session.add(apply1)
        db.session.commit()
    click.echo("测试数据载入完毕！")
```

```
# root/blogApp/views/admin.py
# 后端视图
# 这里只给一个后端蓝图创建的例子，具体根据实际情况
import base64
import datetime
import uuid

from flask import Blueprint, render_template, abort, url_for, \
    current_app, jsonify, request, redirect, session

from blogApp.database import dbModel
from blogApp.tools.IndexData import indexData
from blogApp.tools.check_power import checkLoginState
from blogApp.application.extensions import db
from blogApp.tools.check_power import checkLoginState

admin_view = Blueprint(name='admin_view',
                       import_name=__name__,
                       static_folder="../models/admin/static",
                       static_url_path="/admin_static",
                       template_folder='../models/admin/templates',
                       url_prefix="/admin",
                       )
# 根目录
@admin_view.route('/')
def root():  # put application's code here

    data = dbModel.User.query.filter_by(id=1).first()
    name = data.name
    head_portrait_id = data.head_portrait
    head_portrait = dbModel.Pic.query.filter_by(id=head_portrait_id).first().data
    head_portrait = base64.b64encode(head_portrait).decode("utf-8")
    rootPath = current_app.config.get('ROOT_PATH')

    data_templates = render_template("admin_data.html", rootPath=rootPath, left_page_name_List=left_page_name_List)

    return render_template("admin_base.html",
                           head_portrait=head_portrait,
                           name=name,
                           rootPath=rootPath,
                           data_templates=data_templates,
                           )
```

```Python
# root/blogApp/__init__.py
# 包文件，导出app
from blogApp.application import app
```

```Python
# root/blogApp/config.py
# 配置文件，这个其实可以放在很多中格式的文件里的，我这里给出一个py的例子
class BaseConfig(object):
    '''
    公用配置
    '''
    ROOT_PATH_OBJECT = r"E:\speakForMyself" # 项目根目录
    ROOT_PATH = "http://127.0.0.1:5000/" # 网络环境根目录
    SESSION_TYPE = 'filesystem'  # session类型为filesystem
    LOGIN_COOKIE_NAME = 'identity_token' # 登录后身份session的键值名称
    SQLALCHEMY_TRACK_MODIFICATIONS = False


class TestConfig(BaseConfig):
    '''
    测试环境
    '''
    pass

class DevConfig(BaseConfig):
    '''
    开发环境
    '''
    SQLALCHEMY_DATABASE_URI = 'sqlite:///E:/speakForMyself/blogApp/database/data.sqlite3' # 数据库位置


class ProConfig(BaseConfig):
    '''
    正式环境
    '''
    pass
```

```
# root/.env

#秘钥
SECRET_KEY=089de6ba-7874-11ed-8134-e4fd453f23ec
```

```
# root/.flaskenv

# FLASK_ENV 变量代表运行时的环境时开发环境(development)还是生产环境(production)
FLASK_DEBUG=1
# FLASK_DEBUG 变量代表是否开启调试器开启1关闭0,开发模式下默认开启,不建议手动操作此项目
# FLASK_DEBUG=1
# FLASK_APP 变量代表执行flask run命令时要运行的应用文件
FLASK_APP=run.py
# FLASK_RUN_HOST 变量代表运行的host 0.0.0.0代表外部计算机访问/127.0.0.1代表本机访问
FLASK_RUN_HOST=0.0.0.0
# FLASK_RUN_PORT 变量代表监听的端口默认5000
FLASK_RUN_PORT=5000
```

```Python
# root/run.py
# 启动文件
from blogApp.application.app import app

if __name__ == "__main__":
    app.run()
```