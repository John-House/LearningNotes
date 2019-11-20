[TOC]
# 一、利用表达式提取内容
## 1.1 正则表达式
> 正则表达式由"核心内容"、"数量修饰"和"断言修饰"三个部分组成。匹配的关键在于"核心内容",其余两个部分相当于"量词"和"形容词",用于辅助定位核心内容。
>
### 核心内容
| 字符 | 含义 | 字符 | 含义 |
| :- | :- | :- | :- |
| . | 匹配任意一个非\n的字符 | | |
| \w | 匹配任意一个字母、数字、下划线或汉字 | \W | 匹配任意一个非字母、数字、下划线或汉字 |
| \d | 匹配任意一个数字 | \D | 匹配任意一个非数字 |
| \s | 匹配任意一个空白字符 | \S | 匹配任意一个非空白字符 |
| \b | 匹配单词边界 | \B | 匹配非单词边界 |
| ^ | 匹配字符串开头 | $ | 匹配字符串结尾 |
| [...] | 匹配...中的任意一个字符 | [^...] | 匹配...之外的任意一个字符 |
| [a-zA-Z] | [...]的连写形式 | | |
### 数量修饰
| 字符 | 含义 | 字符 | 含义 |
| :- | :- | :- | :- |
| * | 匹配0或多个核心内容 | *? | 非贪婪模式,尽可能少的匹配 |
| + | 匹配1或多个核心内容 | +? | 非贪婪模式,尽可能少的匹配 |
| ? | 匹配0或1个核心内容 | ?? | 非贪婪模式,尽可能少的匹配 |
| {n} | 匹配n个核心内容 | | |
| {n,} | 匹配n或n以上个核心内容 | {n,}? | 非贪婪模式,尽可能少的匹配 |
| {n,m} | 匹配最少n个最多m个核心内容 | {n,m}? | 非贪婪模式,尽可能少的匹配 |
### 断言修饰
| 字符 | 含义 | 字符 | 含义 |
| :- | :- | :- | :- |
| (?<=Expr>) | 匹配前面是Expr的核心内容<br>Expr不能存在数量修饰 | (?<!Expr) | 匹配前面不是Expr的核心内容<br>Expr不能存在数量修饰 |
| (?=Expr) | 匹配后面是Expr的核心内容<br>Expr不能存在数量修饰 | (?!Expr) | 匹配后面不是Expr的核心内容<br>Expr不能存在数量修饰 |
### 分组捕获
| 字符 | 含义 | 字符 | 含义 |
| :- | :- | :- | :- |
| (核心内容) | 捕获()之间的核心内容,并为之分配组号(从1开始) | \组号 | 匹配之前捕获的对应组号的"实际内容"(而非核心内容) |
| (?P<组名>核心内容) | 捕获()之间的核心内容,并为之分配组名 | (?P=组名) | 匹配之前捕获的对应组名的"实际内容"(而非核心内容) |
| (?:核心内容) | 不捕获()之间的核心内容 | (?#注释内容)| 在正则表达式中添加注释 |
| (?(组号或组名)正则1\|正则2) | 若组号或组名对应的"实际内容"存在,<br>则此处适用"正表达式1",否则适用"正则表达式2" | | |
### 条件匹配
| 字符 | 含义 |
| :- | :- |
| 正则1 \| 正则2 | 短路式匹配正则1或正则2 |

## 1.2 XPath表达式
> XPath以"HTML元素"作为操作节点。
>
### 核心内容
| 字符 | 含义 |
| :- | :- |
| 元素 | 匹配"元素"节点下的所有子节点 |
| / | 匹配HTML文档的根节点 | 
| //元素 | 匹配所有"元素"节点(无视位置结构)|
| . | 匹配当前节点 |
| .. | 匹配当前节点的父节点 |
| @属性 | 匹配所有的"属性" |
| * | 匹配任何节点 |
| @* | 匹配任何存在属性的节点 |
### 定位修饰
| 字符 | 含义 |
| :- | :- |
| [n] | 匹配第n个核心内容 |
| [last()] | 匹配最后一个核心内容 | 
| [last()-n] | 匹配倒数第n+1个核心内容 |
| [position()<n] | 匹配最开始的n-1个核心内容 |
| [@属性] | 匹配存在"属性"的核心内容 |
| [@属性='...'] | 匹配"属性"为...的核心内容 |
### 条件修饰
| 字符 | 含义 |
| :- | :- |
| 核心内容1 \| 核心内容2 | 匹配核心内容1或核心内容2 |
# 二、利用标准库爬取页面
> 1. 构造Request请求
> 2. 发送Request请求并返回Response响应
> 3. 提取Response响应的内容
>
## 2.1 构造Request请求
```Python
import urllib.request as ur             # 用于封装请求
import urllib.parse as up               # 用于URL编码

# 构造完整URL
part_url = up.urlencode({'kw': '中文'}) # 若参数为中文(形如http://url?kw=中文)需要转码
full_url = '...' + part_url 

# 请求体内容需要转码
data = up.urlencode({...})

# 设置请求头内容
headers = {'User-Agent': '...', ...}

# 封装成Request请求
req = ur.Request(full_url, method='大写', data=data, headers=headers)

# Request对象具有以下方法和属性
req.full_url                            # 请求的URL
req.type                                # 请求的协议类型
req.header_items()                      # 请求头内容(返回值为嵌套2-tuple的列表)
req.data                                # 请求体内容
req.set_proxy('host', 'protocal')       # 为请求设置代理
```
## 2.2 发送Request请求,获取Response响应
```Python
1)通过urlopen()发送请求
rsp = ur.urlopen(req)

# 若出现SSL证书需求
import ssl
cxt = ssl.SSLContext()
cxt.verify_mode = ssl.CERT_NONE
rsp = ur.urlopen(req, content=cxt)

2)通过open()发送请求
# 爬取HTTP页面,无需理会SSL证书认证
http_handler = ur.HTTPHandler()

# 爬取HTTPS页面,需要跳过SSL证书认证
cxt = ssl.SSLContext()
cxt.verify_mode = ssl.CERT_NONE
https_handler = ur.HTTPHandler(context=cxt) 

# 爬取需要使用公开代理IP的页面
proxyHandler = ur.ProxyHandler({'protocal': 'IPAdress:Port'})
# 爬取需要使用私密代理IP的页面
proxyHandler = ur.ProxyHandler({'protocal': '账号:密码@IPAdress:Port'})

# 爬取需要设置Cookie的页面
import http.cookiejar as hc
cookie = hc.CookieJar()
cookie_handler = ur.HTTPCookieProcessor(cookie)

# 发送Request请求,获取Response响应
opener = ur.build_opener(http_handler/https_handler/proxyHandler/cookie_handler)
rsp = opener.open(req)
```
## 2.3 处理Response响应
```Python
bytes_content = rsp.read()
str_content = bytes_content.decode('编码格式')

# Response对象具有以下方法和属性
rsp.info()          # 响应头内容
rsp.geturl()        # 响应的URL
```
# 三、利用Requests库爬取页面
## 3.1 构造和发送Request请求
```Python
import requests

# 创建Session对象
sess = requests.Session()

# 发送请求,获取响应
rsp = sess.request(
    method='xx',                            # 'get'/'post'/'head'/'put'/'delete'/'patch'/'options'
    url='...',
    params={'xx': 'xx', ...},               # 查询字符串中的内容
    data={'xx': 'xx', ...},                 # 请求体中form表单的内容
    headers={'xx': 'xx', ...},
    cookies=requests.cookies.RequestsCookieJar(),
    timeout=(connect, accept),              # 请求超时设置,元组中connect是连接服务超时设置,accept是客户端接收超时设置
    proxies={'protocal': 'IPAdress:Port'},  # 代理IP设置
    verify=bool,                            # 是否验证服务器证书
    allow_redirects=bool                    # 是否允许请求重定向
)
```
## 3.2 处理Response响应
```Python
rsp.text            # 响应内容为Unicode类型
rsp.content         # 响应内容为Bytes类型
rsp.json()          # 响应内容为JSON类型

rsp.url             # 响应对象的最终URL
rsp.history         # 响应重定向过程产生的URL列表

rsp.status_code     # 响应的状态码
rsp.cookies         # 响应的Cookie
rsp.headers         # 响应头内容
```
# 四、利用Scrapy框架爬取页面
## 4.1 项目目录结构
```Python
# scrapy startproject 项目名
项目名                      # 爬虫项目顶级目录
    项目名                  # 爬虫项目根目录(所有的import和cmd从这里键入)
        __init__.py
        items.py
        middlewares.py
        pipelines.py
        settings.py
        spiders
            __init__.py
            customSpider.py
    scrapy.cfg

# items.py                  # 构建ORM(即对象关系映射)的文件。
    1.import scrapy  
    2.新建的item类继承scrapy.Item类  
    3.在新建的item类中创建字段名(即scrapy.Field类的实例对象)
# middlewares.py            # 中间件文件。
# pipelines.py              # 处理从customSpider.py文件中yield的item数据的文件。
    1.新建的管道类继承object  
    2.必须实现process_item(self, item, spider)方法  
    3.process_item方法中必须最后return item
# settings.py               # 爬虫项目的高级配置文件
# customSpider.py           # 编写爬虫项目业务逻辑的文件(可以有多个)
# scrapy.cfg                # 爬虫项目的低级配置文件
```
## 4.2 详解-item.py
```Python
from scrapy.item import Item, Field

# 自定义Item类
class 类名(Item):
    xx = Field()
    xx = Field()
    ...
```
## 4.3 详解-customSpider.py
```Python
from scrapy.spiders import (Spider, CrawlSpider, 
    XMLFeedSpider, CSVFeedSpider, SitemapSpider)

# Spider是最基础的Spider类
# 其他还有CrawlSpider/XMLFeedSpider/CSVFeedSpider/SitemapSpider
class 类名(Spider):
    # 设置启动爬虫的密钥,即在终端键入scrapy crawl xx来启动爬虫
    name = 'xx'
    # 设置爬虫爬取的域
    allowed_domains = ['xx', ...]
    # 设置爬虫初始爬取的URL
    start_urls = ['xx', ...]
    # 个性化配置,仅对当前的spider有效,优先级高于settings.py文件
    custom_settings = {...}
    # 设置哪些状态码对应的响应内容会被处理(默认只处理状态码为200~300的响应)
    handle_httpstatus_list = [...]

    # 重写start_requests()方法来自定义处理start_urls
    # 默认是将start_urls中的每个初始URL封装成Request对象yield出去
    def start_requests(self):
        # 自定义如何yield每个初始URL
        pass

    def parse(self, response):
        # 业务逻辑(即提取数据)
        ...
        # 导入Item类,并映射提取的数据
        xx = 类名()
        xx['field1'] = data1
        xx['field2'] = data2
        ...
        # yield Item类
        yield xx

        # 若想跟进爬取URL可以yield Request对象
        # from scrapy.http import Request
        yield Request(
            url,                    # 跟进请求的URL
            callback=None/func,     # 设置处理跟进请求返回的响应的函数
            method='GET',           # 设置跟进请求的方式类型
            headers=None/{...},     # 设置跟进请求的请求头内容
            cookies=None/{...},     # 设置跟进请求的cookies内容
            meta=None/{...},        # 设置跟进请求的元数据
            encoding='utf-8',	
            priority=0/num,         # 设置跟进请求发送的优先级(数字越大优先级越高)
            dont_filter=False       # 设置是否允许对同一URL多次跟进
        )

# Request对象具有以下属性和方法
req.url             # return:str    返回跟进请求的URL
req.method          # return:str    返回跟进请求的方法类型
req.headers         # return:dict   返回跟进请求的请求头内容
req.body            # return:str    返回跟进请求的请求体内容
req.replace(...)    # return:None   通过相同的关键字参数修改跟进请求对象
req.meta = {...}    # return:None   通过赋值修改Request对象的元数据(默认为空字典)
    # .meta属性一般用于middlewares或extensions识别使用,其中的key值可以自定义
    # .meta属性的原生key值有:
        # 为当前的request请求设置代理信息
        req.meta['proxy'] = 'http://IPAdress:Port'或者'http://用户名:密码@IPAdress:Port'

        # 根据Response对象的状态码设置重定向操作
        # 在settings.py中可以设置'REDIRECT_ENABLED'和'REDIRECT_MAX_TIMES'
        req.meta['redirect_urls'] = 'URL'
        req.meta['dont_redirect'] = bool
        
        # 设置哪些状态码对应的响应内容会被处理
        req.meta['handle_httpstatus_list] = [状态码,...] 
        
        # 设置是否所有状态对应的响应内容会被处理
        req.meta['handle_httpstatus_all] = bool

        # 跟进请求失败时是否重新发送
        # 在settings.py中可以设置'RETRY_ENABLED', 'RETRY_TIMES'和'RETRY_HTTP_CODES'
        req.meta['dont_retry'] = bool
        req.meta['max_retry_times'] = num

        # 跟进请求的cookies设置
        # 在setting.py中可以设置'COOKIES_ENABLED'
        # 一个spider中可以设置多个Cookie(默认只有一个),
        # 当前request请求发送时可以指定{'cookiejar': 身份数字}从而使用其他的Cookie；
        # 但无法根据'身份数字'便捷地复用该Cookie,
        # 在Response中复用需要Response.meta['cookiejar']取值
        req.meta['cookiejar'] = 身份数字
                
        # 设置跟进请求是否遵循Robots协议,它的优先级高于setting.py文件中的设置
        req.meta['dont_obey_robotstxt'] = bool

        # 设置下载器下载超时时最多等待多少秒
        req.meta['download_timeout'] = secs
        
        # 设置下载器下载到的Response对象最大允许为多少字节
        req.meta['download_maxsize'] = size

# FormRequest类继承自Request类,通常用于模拟登陆
FormRequest(
    url,                        # 跟进请求的URL
    callback=None/func,         # 设置处理跟进请求返回的响应的函数
    method='POST',              # 设置跟进请求的方式类型
    formdata={...},             # 设置待POST的表单数据
    headers=None/{...},         # 设置跟进请求的请求头内容
    cookies=None/{...},         # 设置跟进请求的cookies内容
    meta=None/{...},            # 设置跟进请求的元数据
    encoding='utf-8',	
    priority=0/num,             # 设置跟进请求发送的优先级(数字越大优先级越高)
    dont_filter=False           # 设置是否允许对同一URL多次跟进
)
# FormRequest类具有以下类方法
FormRequest.from_response(
    response, 
    formname=None/'...',        # 通过表单的name属性定位"表单元素"
    formid=None/'...',          # 通过表单的id属性定位"表单元素"
    formnumber=0,               # (多表单时)通过表单序号定位"表单元素"(默认定位到第1个表单元素)
    formdata=None/{...},        # 待POST的表单数据
    clickdata=None/{...},       # 是否要点击提交表单诗句
    dont_click=False/bool,      # 是否点击元素来提交表单(默认为点击元素提交)
    formxpath=None/'...',       # 通过XPath表达式定位"表单元素"
    formcss=None/'...',         # 通过CSS表达式定位"表单元素"
    **kwargs
)

# CrawlSpider继承自Spider,通常用于深度爬取
class 类名(CrawlSpider):
    # 设置启动爬虫的密钥,即在终端键入scrapy crawl xx来启动爬虫
    name = 'xx'
    # 设置爬虫爬取的域
    allowed_domains = ['xx', ...]
    # 设置爬虫初始爬取的URL
    start_urls = ['xx', ...]
    # 个性化配置,仅对当前的spider有效,优先级高于settings.py文件
    custom_settings = {...}
    # 设置哪些状态码对应的响应内容会被处理(默认只处理状态码为200~300的响应)
    handle_httpstatus_list = [...]
    
    # 设置Rule对象(即跟进爬取的规则)
    # from scrapy.spiders import Rule
    # from scrapy.linkextractors import LinkExtractor
    Rule(
        LinkExtractor(
            allow=()/(正则/[正则1,正则2,...]),      # 设置跟进URL的匹配规则
            deny=()/(正则/[正则1,正则2,...]),       # 设置不跟进URL的匹配规则
            restrict_xpaths=()/(XPath表达式),      # 配合allow/deny过滤出跟进URL(XPath表达式只需要定位到"具体元素")
            allow_domains=()/(...),               # 设置跟进URL允许在的域
            deny_domains=()/(...),                # 设置跟进URL不允许在的域
            unique=True/bool,                     # 设置是否过滤重复的跟进URL
            process_value=None/func,              # 设置微处理跟进URL的函数
            strip=True/bool                       # 设置去掉跟进URL中的空格
        ),
        callback=None/'func',           # 设置由哪个函数处理跟进URL返回的响应
        cb_kwargs=None,                 # 为callback指定的函数提供参数
        follow=None,                    # 设置是否对跟进URL再跟进(即深度爬取)
        process_links=None,             # 设置微处理跟进URL对象的函数(在process_value之后)
        process_request=identity        # 设置微处理跟进URL封装成的请求对象的函数(identity是个"def func(x): return x"函数)
    )
    rules = [Rule_obj, Rule_obj, ...]

    # 对应Rule(...)中的callback参数(禁止命名为parse)
    def 函数(self, response):
        # 业务逻辑(即提取数据)
        ...
        # 导入Item类,并映射提取的数据
        xx = 类名()
        xx['field1'] = data1
        xx['field2'] = data2
        ...
        # yield Item类
        yield xx

        # 也可以yield新的请求
        # from scrapy.http import Request
        yield Request(...)

# from scrapy.http import Response
# Response对象具有以下属性和方法：
rsp.url                     # return:str    返回Response对象的URL
rsp.status                  # return:int    返回Response对象的状态码
rsp.headers                 # return:dict   返回Response对象的响应头内容
rsp.body                    # return:bytes  返回Response对象的响应体内容
rsp.request                 # return:req    返回Response对象对应的Request对象
rsp.meta                    # return:dict   返回跟进URL对应的Request对象的元数据

rsp.text                    # return:str    返回Response对象的内容(Unicode类型)
rsp.xpath(...).extract()    # return:list   返回Response对象中根据xpath规则提取的内容
rsp.css(...).extract()      # return:list   返回Response对象中根据css规则提取的内容
```
## 4.4 详解-pipelines.py
> 管道文件通常用于"清理HTML数据","验证item数据是否包含需要的内容","将数据储存到数据库中"等
>
```Python
class 类名(object):
    # 必须重写该方法,必须返回item或raise DropItem
    def process_item(self, item, spider):
        ...
        # 通常会将item转换成JSON格式写入文件中
        # 利用内置函数dict()将item转换成字典格式
        import json
        data = json.dumps(dict(item)) 
        file_obj.write(data)
        return item
    
    # 爬虫开启时调用该方法
    def open_spider(self, spider):
        pass
    
    # 爬虫结束时调用该方法
    def close_spider(self, spider):
        pass
```
```Python
# 下载文件或图片时,激活FilesPipeline或ImagesPipeline
# 本质上,在业务逻辑中yield files_url或images_URL,
# 程序会自动发送请求并处理队列,将返回的响应映射到"内置字段"中
为此,需要额外配置：
1.安装Pillow库

2.在items.py文件的Item类中添加
    file_urls = Field()
    files = Field()
  和/或
    image_urls = Field()
    images = Field()

3.在settings.py文件中添加
    ITEM_PIPELINES = {'scrapy.pipelines.files.FilesPipeline': 权重}
    FILES_STORE = '文件储存路径'
    # 避免下载近期下载过的内容(默认为90天)
    FILES_EXPIRES = 90		

    ITEM_PIPELINES = {'scrapy.pipelines.images.ImagesPipeline': 权重}
    # 路径下会生成'full'子目录用于储存爬取到的图片,'thumbs'用于储存缩略图(若已配置)
    IMAGES_STORE = '图片储存路径'	
    # 设置缩略图的信息,'缩略图名'就是储存该类缩略图的目录名, 
    # 例如：IMAGES_THUMBS = {'small': (20,20), 'big': (120, 120)}
    IMAGES_THUMBS = {'缩略图名': (宽, 高), ...} 
    # 设置下载图片的最小宽度(用于图片过滤,可以仅设置最小宽度,也可以都设置；
    # 都设置时,全都满足的图片才会被下载)	
    IMAGES_MIN_WIDTH = 数字  
    # 设置下载图片的最小高度(用于图片过滤,可以仅设置最小高度,也可以都设置；
    # 都设置时,全都满足的图片才会被下载)
    IMAGES_MIN_HEIGHT = 数字 
    # 避免下载近期下载过的内容(默认为90天)
    IMAGES_EXPIRES = 90		

    # 自定义字段时才会添加
    FILES_URLS_FIELD = '自定义的file_urls'
    FILES_RESULT_FIELD = '自定义的files'

    IMAGES_URLS_FIELD = '自定义的image_urls'
    IMAGES_RESULT_FIELD = '自定义的images'

    # 设置下载file或image时,是否允许重定向
    MEDIA_ALLOW_REDIRECTS = bool  

    # 如果你通过继承和重写设置了多个pipeline,那么你可以在settings.py文件中
    # 在'默认设置项'前加上'全大写的'自定义pipeline类名,从而让你自定义的pipeline生效,
    # 例如：IMAGES_URLS_FIELD 应该为 MYPIPELINE_IMAGES_URLS_FIELD

4.在customSpider.py文件中"提取结构数据"时
    xx = Item类()
    xx['field'] = ['url']	# 映射file或image的URL时必须以"列表"形式
    yield item

5.自定义管道文件
    5.1 继承FilesPipeline或ImagesPipeline类
    5.2 重写一个或多个方法
        get_media_requests(self, item, info)
            --> 该方法的本质是遍历item[key],对每个URL重新yield Request(...),
                注意: key对应的value是列表形式,因为spider.py中yield的是列表；至于key是什么,取决于items.py中的字段名
                注意: 可以在重新yield Request(...)时添加meta信息,便于后续方法提取；
            --> 例如：def get_media_requests(self, item, info):
                        for file_url in item['file_urls']:
                            yield scrapy.Request(file_url)
        
        file_path(self, request, response=None, info=None):
            return 'full/%s' % (自定义文件名,)
            --> 注意: 该方法本质是提供文件(包括图片)的储存路径,可以利用request中储存的meta数据来传递信息；
            --> 例如：def file_path(self, request, response=None, info=None):
                        image_path = request.url.split('/')[-1]
                        return 'full/%s' % (image_path,)

        thumb_path(self, request, thumb_id, response=None, info=None):
            ...
            return 'thumbs/%s/%s' % (thumb_id, 自定义缩略图名)
            --> 例如：def thumb_path(self, request, thumb_id, response=None, info=None):
                        thumb_path = request.url.split('/')[-1]
                        return 'thumbs/%s/%s' % (thumb_id, thumb_path,)
            
        item_completed(self, results, item, info):
            --> 当【1个item】中所有的URL请求都发送完毕后(无论结果是OK,还是Failed),就会调用该方法,
                其本质是过滤出results中状态为OK(即下载成功)的数据并return,以便其他的pipeline处理它,
                注意:
                results参数是一个二元元组构成的列表,形如：(success,file_info_or_error),
                其中,file_info_or_error是一个字典,形如：
                {'url': '数据下载的来源', 'path': '相对于settings中xx_STORE的相对路径', 'checksum': '数据的MD5哈希值'}
                示例:
                [(True, {'checksum': '2b00042f7481c7b056c4b410d28f33cf',
                    'path': 'full/0a79c461a4062ac383dc4fade7bc09f1384a3910.jpg',
                    'url': 'http://www.example.com/files/product1.pdf'}),
                (False, Failure(...)),
                ...]

            --> 例如：from scrapy.exceptions import DropItem
                def item_completed(self, results, item, info):
                    file_paths = [x['path'] for ok, x in results if ok]
                    if not file_paths:
                        raise DropItem("Item contains no files")
                    # 如果是图片,则改为item['image_paths] = xxx
                    # 如果是自定义的字段,则改为item['自定义字段名'] = xxx
                    item['file_paths'] = file_paths   
                    return item
```
## 4.5 详解-settings.py
```Python
CONCURRENT_ITEMS
    --> 默认100
    --> 每个Response可以并发处理的最大item数量

CONCURRENT_REQUESTS
    --> 默认16
    --> 可以并发请求的最大数量

CONCURRENT_REQUESTS_PER_DOMAIN
    --> 默认8
    --> 单个域中可以并发请求的最大数量

DEFAULT_REQUEST_HEADERS
    --> 默认的请求头设置

DOWNLOAD_DELAY
    --> 默认0
    --> 设置连续下载页面之间的间隔时间(即延时时间)

DUPEFILTER_CLASS
    --> 默认'scrapy.dupefilters.RFPDupeFilter'
    --> 设置默认的去重类
```
## 4.6 详解-middlewares.py
```Python
# 下载中间件用于处理每一个请求和响应(若对应的权重设为None,则表示禁用该中间件),
# 其本质就是修改Request对象和Response对象的属性
1.在settings.py文件中配置下载中间件
    DOWNLOADER_MIDDLEWARES = {
        'myproject.middlewares.CustomDownloaderMiddleware': 543,
        'xxxx': None,
    }

2.下载中间件可以配置多个
    处理request时,会按照权重升序(由小到大)依次调用每个中间件的process_request()方法
    处理response时,会按照权重降序(由大到小)依次调用每个中间件的process_response()方法

3.自定义下载中间件类(实现了以下一个或多个方法的普通类)
    def process_request(self, request, spider):
        --> return: None/Request对象/Response对象/raise IgnoreRequest异常
        --> 如果返回None,则表示该中间件不做任何加工处理,直接将request对象交给下一个中间件处理；
            如果返回Request对象,则程序不再调用(后续中间件中的)process_request()方法,而是直接将这个返回的Request对象重新入队列
                (之后拿到的response对象仍然会正常走一系列中间件process_response()方法调用)；
            如果返回Response对象,则程序不再调用任何其他的process_request()和process_exception()方法,
                而是直接返回这个Response对象(开启中间件中process_response()方法的依次调用)
            如果抛出IgnoreRequest异常,则调用process_exception()方法,如果没有则依次向上传递,
                直到顶层时被忽略(不记入log中,这点与一般的异常处理不同)

    def process_response(self,request,response,spider):
        --> ctype: Request对象/Response对象/raise IgnoreRequest异常
        --> 如果返回Request对象,则程序不再调用(后续中间件中的)process_response()方法,而是直接将这个返回的Request对象重新入队列；
        --> 如果返回Response对象,则程序将这个返回的Response对象(可以不变,也可以是新的)交给下一个中间件处理；
            如果抛出IgnoreRequest异常,则依次向上传递异常,直到顶层时被忽略(不记入log中,这点与一般的异常处理不同)

    def process_exception(self,request,exception,spider):
        --> ctype: None/Request对象/Response对象
        --> 如果返回None,则程序将依次调用下一个中间件的process_exception()方法,直到异常被处理或者被抛到顶层时忽略；
            如果返回Request对象,则程序不再调用(后续中间件中的)process_exception()方法,而是直接将这个返回的Request对象重新入队列；
            如果返回Response对象,则程序不再调用(后续中间件中的)process_exception()方法,
                                而是直接返回这个Response对象(开启中间件中process_response()方法的依次调用)
```
## 4.7 分布式爬虫
```Python
1.安装scrapy-redis库:
    pip3 install scrapy-redis

2.开启Master端的redis-server,并注释掉"bind 127.0.0.1"配置项或者添加多个"bind 爬虫端的IPAdress":
    爬虫端无需启动redis-server,只要能够连接Master端的redis-server进行数据读/写即可

3.在原有scrapy项目的基础上调整部分代码：
    # 在customSpider.py中
    from scrapy_redis.spiders import RedisSpider, RedisCrawlSpider
        -> 原有scrapy项目中,继承spider类的现在继承RedisSpider类,
                           继承CrawlSpider类的现在继承RedisCrawlSpider类,

        删除"start_urls = [...]"代码,使用：
        -> redis_key = 'xxx'
           # 'xxx'建议为'myspider:start_urls'
           # 这个字符串被用作Master端redis-server推送初始URL的命令符之一；

    # 在settings.py中
        -> 指定使用scrapy-redis的调度器
        SCHEDULER = "scrapy_redis.scheduler.Scheduler"

        -> 指定使用scrapy-redis的去重
        DUPEFILTER_CLASS = 'scrapy_redis.dupefilters.RFPDupeFilter'

        -> 指定排序爬取地址时使用的队列,
        # 默认的,按优先级排序(Scrapy默认),由sorted set实现的一种非FIFO、LIFO方式。
        SCHEDULER_QUEUE_CLASS = 'scrapy_redis.queue.SpiderPriorityQueue'

        # 可选的 按先进先出排序(FIFO)
        SCHEDULER_QUEUE_CLASS = 'scrapy_redis.queue.SpiderQueue'

        # 可选的 按后进先出排序(LIFO)
        SCHEDULER_QUEUE_CLASS = 'scrapy_redis.queue.SpiderStack'

        # 只在使用SpiderQueue或者SpiderStack是有效的参数,指定爬虫关闭的最大间隔时间
        SCHEDULER_IDLE_BEFORE_CLOSE = 10

        # 在redis中保持scrapy-redis用到的各个队列,从而允许暂停和暂停后恢复,也就是不清理redis queues
        SCHEDULER_PERSIST = True

        # 通过配置RedisPipeline将item写入key为 spider.name : items 的redis的list中,供后面的分布式处理item
        ITEM_PIPELINES = {
            'example.pipelines.ExamplePipeline': 300,
            'scrapy_redis.pipelines.RedisPipeline': 400
        }

        # 配置redis数据库的连接参数
        REDIS_HOST = '127.0.0.1'
        REDIS_PORT = 6379
        REDIS_PASS = 'redisP@ssw0rd'

4.将修改后的scrapy项目代码分发到各个爬虫端,并在爬虫文件同级目录的终端下输入：
    scrapy runspider 爬虫文件.py

5.在Master端的redis-cli交互模式下输入：
    lpush xxx 初始URL   # xxx 即redis_key的值(不带引号)
```