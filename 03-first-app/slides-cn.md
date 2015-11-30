## Cloud Foundry <br />从无名，到英雄
### [03 我是如何CF push我的应用的?](#/0)

<p style="font-size: 50%; opacity: 0.2;">
   本文版权归CloudCredo所有。 &copy; CloudCredo 2015.保留一切权利。
</p>

---

# [特性](#/1)

```nohighlight
我希望能像CF英雄那样
使自己的应用可以通过URL被访问
这样便能让全世界大开眼界了
```

---

## [我是如何将](#/2)应用部署[至CF的?](#/2)

```bash
# 切换至training主目录:
$ cd 03-first-app/web-app
$ cf push

...

urls: web-app-unpassionate-eighteen.cfapps.io

...
```

<img src="images/first-app.png" style="background:none; border:none; box-shadow:none;" />

注:
  既然我们已经登录了，那就赶紧在CF上部署一个示例应用吧。

---

## [CF space中的](#/3) apps [是怎样的?](#/3)

```bash
$ cf apps

name      state     instances   memory   disk   urls
web-app   started   1/1         32M      256M   web-app-unpass...
```

---

## [我想查看](#/4) 更多关于app的细节

```bash
$ cf app web-app

requested state: started
instances: 1/1
usage: 32M x 1 instances
urls: web-app-unpassionate-eighteen.cfapps.io
last uploaded: Mon Nov 2 10:18:05 UTC 2015
stack: cflinuxfs2
buildpack: ruby 1.6.7

     state     since        cpu    memory         disk
#0   running   2015-11-02   0.0%   25.7M of 32M   95.1M of 256M
```

---

## 应用名(app name)及配额限制(quotas) [从何而来?](#/5)

```bash
$ cat 03-first-app/web/manifest.yml

name: web-app
memory: 32M
disk_quota: 256M
random-route: true
buildpack: ruby_buildpack
```

---

## app的默认配额限制[是怎样的?](#/6)

  * Memory: **1G**
  * Disk: **1G**

---

## 随机路由(random route) [是做什么用的?](#/7)

`cfapps.io` 是由所有 [run.pivotal.io](https://run.pivotal.io/) 应用共享使用的。

```bash
$ cf app web-app

...

urls: web-app-unpassionate-eighteen.cfapps.io

...
```

---

## [app](#/8) 运行[在何处?](#/8)

```nohighlight
                           CONSUMER

                              |

                            ROUTER

                            /    \
                           /      \
                          /        \
                         /          \

                      HOST 1       HOST 2

                        |            |
      APP A INSTANCE 1 -|            |- APP A INSTANCE 2
      APP B INSTANCE 2 -|            |- APP B INSTANCE 1
```

注:
  应用运行于应用宿主机，即熟知的DEA（CF v2）或CELL（CF v3）之上。

  router将访问流量转发至与Host header匹配的IP & port组合。

  多应用实例(instances)意味着访问流量会被随机分配。

  Cloud Foundry生产环境通常部署多个routers来确保弹性可恢复，部署多个应用宿主机（app hosts）来优化应用请求分发。

---

## cf push[命令背后的执行流程?](#/9)

1. App文件上传至CF
1. 生成可运行的app加工品(Runnable app artefact)(droplet)
1. App在应用宿主机上启动

<hr />

App 接收web请求(在已绑定TCP端口的前提下)

---

## [1.](#/10) App 文件 [上传至CF](#/10)

使用`cf` cli即可，无需其他依赖工具。

定义在[`.cfignore`](https://docs.cloudfoundry.org/devguide/deploy-apps/prepare-to-deploy.html#exclude) 中的文件将不会被上传。

注:
  cf cli 将所有app文件发送至 Cloud Controller。

  Cloud Controller将所接收到的所有文件存储至Blob Store。

  Cloud Controller同时会在数据库中存储该app相关的元数据(metadata)。

---

## [2. 可运行的](#/11) app 加工品

App 文件 + 运行时依赖 = App 加工品

注:
  这项工作由Buildpack完成，属于staging阶段的一部分。
  
  执行cf push命令时控制台的输出主要是staging阶段的日志。

  在后续的课程中您将学习到到更多关于Buildpacks的知识。

---

## [3. 在应用宿主机上](#/12) 启动 App

如果这是个web程序，将会与TCP端口绑定。

---

## [App接收](#/13) web请求

请求在多个应用示例（app instances）间随机分发。

---

## [如何](#/14) 部署worker app[?](#/14)

```bash
# 切换至training主目录:
$ cd 03-first-app/worker-app
$ cf push
```

```bash
$ cf app worker-app

requested state: started
instances: 1/1
usage: 16M x 1 instances
urls:
last uploaded: Mon Nov 2 13:56:39 UTC 2015
stack: cflinuxfs2
buildpack: binary_buildpack

     state     since        cpu    memory         disk
#0   running   2015-11-02   0.0%   10.7M of 16M   27.3M of 64M
```

---

## [如何](#/15) 查看这个worker app是做什么用的[?](#/15)

```bash
$ cf logs worker-app --recent

...
2015... [App/0] ERR + main
2015... [App/0] ERR + find_my_public_ip
2015... [App/0] ERR + which curl
2015... [App/0] ERR + curl -sL https://api.ipify.org?format=json
2015... [App/0] OUT {"ip":"54.236.219.204"}
2015... [App/0] ERR + suspend_myself
2015... [App/0] ERR + kill -STOP 11
```

---

# <span style="color: #8FF541;">DELIVERED</span>

```nohighlight
我希望能像CF英雄那样
使自己的应用可以通过URL被访问
这样便能让全世界大开眼界了
```

---

## 删除 [应用](#/17)

```bash
$ cf delete worker-app
```

```bash
$ cf delete -f -r web-app
```

---

## [答](#/18) 疑?

> 提问必须正经严肃，解答可以风趣幽默。

---

# CF超级英雄

  * 使用[`CF_TRACE=true`](https://docs.cloudfoundry.org/devguide/deploy-apps/troubleshoot-app-health.html#trace)调试`cf`命令。
  * 创建你自己的[`manifest.yml`](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html)。
  * 学习[部署 WAR 文件](https://docs.cloudfoundry.org/buildpacks/java/java-tips.html)。

<p style="font-size: 50%; opacity: 0.2;">
  本文版权归CloudCredo所有。 &copy; CloudCredo 2015. 保留一切权利。
</p>
