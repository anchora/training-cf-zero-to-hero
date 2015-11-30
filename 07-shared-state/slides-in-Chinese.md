## Cloud Foundry <br />从入门到精通
### [07 我的应用该将状态保存于何处?](#/0)

<p style="font-size: 50%; opacity: 0.2;">
  本文版权归CloudCredo所有。 &copy; CloudCredo 2015. 保留一切权利。
</p>

---

# [特性](#/1)

```nohighlight
身为一名CF高手
我需要一个新版应用来呈现相同的数据
这样我便可以对用户反馈进行迭代了
```

---

## 应用状态[该保存于何处?](#/2)

保存在独立于Cloud Foundry之外部署的服务中。

通过[Service Broker](https://docs.cloudfoundry.org/services/overview.html)向CF注册。

---

## [什么是](#/3) Service Broker[?](#/3)

一个用于提供某种特定类型资源的服务。

一组用于公告服务类型及相应套餐计划目录的API接口。

注:
  Service Broker API由Cloud Controller注册及调用。

---

## [如何查看所有可用的](#/4) <br />服务类型[?](#/4)

```bash
$ cf marketplace
```

```bash
service          plans               description
____________________________________________________________________

cleardb          spark, boost*, ...  Highly available MySQL for y...
cloudamqp        lemur, tiger*, ...  Managed HA RabbitMQ servers ...
elephantsql      turtle, panda*, ..  PostgreSQL as a Service     ...
ironworker       large*,        ...  Scalable Background and Asyn...
loadimpact       lifree, li100*, ..  Automated and on-demand perf...
memcachedcloud   100mb*, 250mb*, ..  Enterprise-Class Memcached f...
mongolab         sandbox        ...  Fully-managed MongoDB-as-a-S...
newrelic         standard       ...  Manage and monitor your apps...
rediscloud       100mb*, 250mb*, ..  Enterprise-Class Redis for D...
searchly         small*, micro*, ..  Search Made Simple. Powered-...
sendgrid         free, bronze*,  ..  Email Delivery. Simplified.
....................................................................
```

---

## [创建一个](#/5) Redis服务实例

```bash
$ cf create-service rediscloud 30mb redis
```

```bash
$ cf services

name    service      plan   bound apps   last operation
redis   rediscloud   30mb                create succeeded
```

---

## [我想让我的应用](#/6) 将状态保存至 <br />这个Redis服务实例中

```bash
# 切换至training主目录:
$ cd 07-shared-state/stateful-app
$ cf push --no-start
```

```bash
$ cf bind-service stateful-app redis

App stateful-app is already bound to redis.
# Services can be defined in the app manifest
```

```bash
$ cf start stateful-app
```

<img src="images/index.png" style="background:none; border:none; box-shadow:none;" />

注:
  您也可以在manifest.yml文件中绑定服务。前提是所绑定的服务必须已经预先创建。

  刷新示例应用的页面，提高访问数，查看redis的连通性。

---

## [是否](#/7) 应用的所有实例 <br />均使用同一个Redis[?](#/7)

```bash
$ cf scale stateful-app -i 2
```

<img src="images/alt-index.png" style="background:none; border:none; box-shadow:none;" />

---

## 服务详情如何暴露给应用[?](#/8)

```bash
$ cf env stateful-app
...
 "VCAP_SERVICES": {
  "rediscloud": [
   {
    "credentials": {
     "hostname": "pub-redis-15708.us-east-1-4.6.ec2.redislabs.com",
     "password": "PASSWORD",
     "port": "15708"
    },
...
```

```
$ redis-cli -h pub-redis-15708...redislabs.com -p 15708 -a PASSWORD
pub-redis-15708.us-east-1-4.6.ec2.redislabs.com:15708> keys *
1) "9f397b68...total_instance_10.10.81.59:61500_responses"
2) "9f397b68...total_instance_10.10.17.49:61509_responses"
3) "9f397b68...total_app_responses"
```

```bash
pub-redis-15708.us-east-1-4.6.ec2.redislabs.com:15708> exit
```

---

## [一起来](#/9)对用户反馈进行迭代

```bash
$ cf set-env stateful-app SHOW_APP_SUPPORTERS true
$ cf restart stateful-app
```

<img src="images/app-supporters.png" style="background:none; border:none; box-shadow:none;" />

---

## 应用被[误](#/10)删除了

事实上是: CEO胡乱点击了某些链接。

```bash
$ cf delete -f -r stateful-app
```

令人欣慰的是，我们恢复了应用。

```
# 切换至training主目录:
$ cd 07-shared-state/stateful-app
$ cf push --no-start
$ cf set-env stateful-app SHOW_APP_SUPPORTERS true
$ cf start stateful-app
```

<img src="images/new-app-supporters.png" style="background:none; border:none; box-shadow:none;" />

注:
  URL不同是因为我们部署了一个新应用。

---

# <span style="color: #8FF541;">特性已传达</span>

```
身为一名CF高手
我需要一个新版应用来呈现相同的数据
这样我便可以对用户反馈进行迭代了
```

---

## [答](#/12) 疑?

> 提问必须正经严肃，解答可以风趣幽默。

---

# CF 高手进阶

  * 使用[SendGrid](https://sendgrid.com/)发送e-mails
  * 使用[IronWorker](https://www.iron.io/worker/)执行异步任务
  * 学习[security groups](https://docs.cloudfoundry.org/adminguide/app-sec-groups.html)
  * 使用[手动创建](https://docs.pivotal.io/pivotalcf/devguide/services/user-provided.html) 的IBM [Cloudant instance](https://cloudant.com/)

<p style="font-size: 50%; opacity: 0.2;">
  本文版权归CloudCredo所有。 &copy; CloudCredo 2015. 保留一切权利。
</p>
