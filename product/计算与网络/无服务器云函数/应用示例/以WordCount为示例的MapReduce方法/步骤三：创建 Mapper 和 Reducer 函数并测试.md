在此部分中，用户将创建两个函数来实现简单的WordCount，并通过控制台/API调用来测试函数。

## 创建 Mapper 函数
### 通过控制台创建函数
1) 登录[无服务器云函数控制台](https://console.cloud.tencent.com/scf)，在【广州】地域下点击【新建】按钮；

2) 进入函数配置部分，函数名称填写`Mapper`，剩余项保持默认，点击【下一步】；

3）进入函数代码部分，选择【本地上传zip包】。执行方法填写`map_function.main_handler`，选择步骤二：创建部署程序包中创建的 `mapper.zip`，点击【下一步】；

4) 进入触发方式部分，此时由于需要先手动测试函数，暂时不添加任何触发方式，点击【完成】按钮。

### 通过 API 创建函数
请参考 CreateFunction 接口

## 创建 Reducer 函数
### 通过控制台创建函数
1) 登录[无服务器云函数控制台](https://console.cloud.tencent.com/scf)，在【广州】地域下点击【新建】按钮；

2) 进入函数配置部分，函数名称填写`Reducer`，剩余项保持默认，点击【下一步】；

3）进入函数代码部分，选择【本地上传zip包】。执行方法填写`reduce_function.main_handler`，选择步骤二：创建部署程序包中创建的 `reducer.zip`，点击【下一步】；

4) 进入触发方式部分，此时由于需要先手动测试函数，暂时不添加任何触发方式，点击【完成】按钮。

### 通过 API 创建函数
请参考 CreateFunction 接口

## 测试函数
在创建函数时，通常会使用控制台或 API 先进行测试，确保函数输出符合预期后再绑定触发器进行实际应用。

### 使用控制台测试函数

1) 在刚刚创建的 Mapper 函数详情页中，点击【测试】按钮；

2) 在测试模版下拉列表中选择【COS 上传/删除文件测试代码】

3) 轻微改动测试代码，将 `name`设置为步骤一：准备 COS Bucket 中创建的`srcmr`存储桶名称，将`key`设置为步骤一：准备 COS Bucket 中上传的`/serverless.txt`键值，如下面示例：
```
{  
   "Records":[  
      {
        "event": {
          "eventVersion":"1.0",
          "eventSource":"qcs::cos",
          "eventName":"event-type",
          "eventTime":"Unix 时间戳",
          "eventQueue":"qcs:0:cos:gz:1251111111:cos",
          "requestParameters":{
            "requestSourceIP": "111.111.111.111",
            "requestHeaders":{
              "Authorization": "上传的鉴权信息"
            }
          }
         },
         "cos":{  
            "cosSchemaVersion":"1.0",
            "cosNotificationId":"设置的或返回的 ID",
            "cosBucket":{  
               "name":"srcmr", #set to demo bucket here
               "appid":"appId",
               "region":"gz"
            },
            "cosObject":{  
               "key":"/serverless.txt", #set to demo file here
               "size":"1024",
               "meta":{
                 "Content-Type": "text/plain",
                 "x-cos-meta-test": "自定义的 meta",
                 "x-image-test": "自定义的 meta"
               },
               "url": "访问文件的源站url"
            }
         }
      }
   ]
}
```

4) 点击【运行】按钮，观察运行结果。

5) 前往[对象存储控制台](https://console.cloud.tencent.com/cos4/index)，点击步骤一：准备 COS Bucket 中创建的`destmr`，观察该 COS Bucket 中是否有`result_middle_serverless.txt`文件生成，该文件中统计了刚刚上传的文本文件中文章各个单词出现的次数：

6) 下载该文件，文件内容应该类似如下：
```
the	29
as	25
to	20
of	17
and	16
a	16
code	16
in	15
be	14
or	12
serverless	10
is	10
that	10
computing	9
by	9
for	9
an	8
not	8
cloud	7
functions	6
edit	6
can	6
with	5
servers	5
on	5
because	5
at	5
OpenWhisk	5
Serverless	5
have	5
platform	5
such	5
time	4
virtual	4
up	4
function	4
provider	4
autoscaling	4
used	4
it	4
js	4
are	4
also	4
than	4
service	4
written	4
2016	4
1	4
runtime	4
example	4
use	4
Node	4
run	3
does	3
Java	3
execution	3
other	3
model	3
In	3
requests	3
means	3
AWS	3
typically	3
which	3
latency	3
any	3
could	3
This	3
may	3
Lambda	3
via	3
Azure	3
resources	3
required	2
10	2
developers	2
application	2
underutilisation	2
hosted	2
was	2
November	2
start	2
support	2
microservices	2
per	2
back	2
server	2
source	2
generally	2
It	2
well	2
user	2
running	2
Python	2
handling	2
JSON	2
addition	2
limits	2
both	2
available	2
For	2
request	2
Google	2
The	2
efficient	2
part	2
2	2
system	2
APIs	2
first	2
completely	2
supports	2
container	2
programmer	2
API	2
name	2
provision	2
now	2
they	2
3	2
its	2
operations	2
machine	2
end	2
IBM	2
more	2
from	2
public	2
officially	2
Amazon	2
even	2
cost	2
Functions	2
all	2
requires	1
At	1
2006	1
starting	1
serialized	1
calls	1
assets	1
InterConnect	1
exposure	1
needed	1
included	1
suited	1
includes	1
you	1
5	1
setting	1
when	1
no	1
periods	1
abstract	1
defined	1
called	1
continuously	1
simplifying	1
Infrequently	1
but	1
triggered	1
significantly	1
registration	1
down	1
functional	1
into	1
own	1
software	1
automatically	1
introduced	1
about	1
although	1
processing	1
business	1
However	1
latencies	1
some	1
creation	1
exposed	1
spins	1
either	1
specific	1
necessary	1
using	1
rules	1
usable	1
has	1
Despite	1
4	1
web	1
world	1
being	1
just	1
spend	1
without	1
person	1
task	1
provisioned	1
uses	1
Resource	1
meets	1
development	1
composition	1
units	1
make	1
pay	1
if	1
worry	1
technology	1
launched	1
stopping	1
owns	1
policies	1
will	1
batch	1
unlike	1
case	1
demand	1
ensuring	1
bulk	1
containers	1
offers	1
been	1
billing	1
tuning	1
GitHub	1
sequences	1
simply	1
simple	1
point	1
consume	1
Monitoring	1
s	1
sensitive	1
triggers	1
essentially	1
packages	1
premise	1
Programming	1
initially	1
terms	1
C	1
invoker	1
behind	1
capacity	1
Performance	1
configured	1
wrote	1
where	1
fixed	1
architecture	1
imposed	1
Swift	1
resource	1
infrequent	1
conjunction	1
cheaper	1
do	1
8	1
black	1
only	1
outside	1
renting	1
quantity	1
Cloud	1
Cost	1
traditional	1
considered	1
alpha	1
significant	1
performance	1
online	1
purchasing	1
responsible	1
offering	1
option	1
need	1
rent	1
debugging	1
jobs	1
PaaS	1
11	1
specialists	1
released	1
serve	1
Zimki	1
structures	1
their	1
management	1
given	1
open	1
directly	1
instances	1
number	1
features	1
major	1
launch	1
language	1
multithreading	1
announced	1
7	1
since	1
measure	1
Cognito	1
another	1
events	1
fully	1
9	1
introduce	1
Alternatively	1
greater	1
affected	1
non	1
might	1
2014	1
systems	1
production	1
actually	1
group	1
style	1
suffer	1
including	1
order	1
allow	1
Haskell	1
purchase	1
response	1
data	1
designed	1
produce	1
REST	1
provisioning	1
rather	1
involve	1
providers	1
dedicated	1
believed	1
this	1
hour	1
endpoint	1
high	1
billed	1
known	1
expose	1
Bluemix	1
sharing	1
6	1
Microsoft	1
would	1
environment	1
private	1
followed	1
FaaS	1
involves	1
amount	1
deserialized	1
box	1
groups	1
supporting	1
satisfy	1
new	1
version	1
Disadvantages	1
machines	1
likely	1
cron	1
HTTP	1
workloads	1
small	1
level	1
announcing	1
include	1
languages	1
Docker	1
go	1
manages	1
```