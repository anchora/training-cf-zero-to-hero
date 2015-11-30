## Cloud Foundry <br />从入门到精通
### [05 如何使我的应用恢复运行状态?](#/0)

<p style="font-size: 50%; opacity: 0.2;">
  本文版权归CloudCredo所有。 &copy; CloudCredo 2015. 保留一切权利。
</p>

---

# [特性](#/1)

```nohighlight
身为一名CF高手
我希望我的应用能够自动恢复运行状态
这样意外的错误就不会使其永远离线了
```

---

## [先一起来](#/2) 部署一个应用

```bash
# 切换至training主目录:
$ cd 05-resilience/imperfect-app
$ cf push

...

urls: imperfect-app-votive-seeress.cfapps.io

...
```

<img src="images/index.png" style="background:none; border:none; box-shadow:none;" />

---

## 你已经部署好了一个 [新应用!](#/3)

该静态网站有极好的流量处理能力，

大家都想访问下你的应用，然而...

---

## [它](#/4) 崩溃了

> _版本 1 很糟糕，但无论如何先部署上去再说_

<img src="images/crash.png" style="background:none; border:none; box-shadow:none;" />


注:
  [Coding Horror wisdom](http://blog.codinghorror.com/version-1-sucks-but-ship-it-anyway/)

---

## [如何](#/5) 使应用恢复运行状态[?](#/5)

允许出错 &amp; 同一应用运行多个实例

```bash
$ cf scale imperfect-app -i 3
$ cf apps

name            state     instances   memory   disk   urls
imperfect-app   started   2/3         64M      256M   imperfect..
```

注:
  由于是CF在运行您的应用，因此运行多个实例不过是一条命令的事情。

  注意应用不同实例运行IP的区别...

  尽管每个DEA都运行于不同的主机上，多个应用实例仍有可能运行在同一个主机上。

  应用实例越多，其运行于同一主机的几率越小。

  下一代CF运行时-即Diego-对应用实例的均匀分布做了更好的优化。

---

## [即便多个实例崩溃](#/6) <br />应用仍旧可以访问

```bash
$ cf app imperfect-app

     state     since       cpu    memory         disk
#0   running   2015-11-02  0.0%   25.3M of 32M   66.9M of 128M
#1   down      2015-11-02  0.0%   0 of 0         0 of 0
#2   down      2015-11-02  0.0%   0 of 0         0 of 0
```

```bash
$ watch cf apps # 实时查看应用实例重启
```

<img src="images/index.png" style="background:none; border:none; box-shadow:none;" />

---

## [是谁在]重启崩溃的应用(#/7) [?](#/7)

DEA v2的Health Manager (即[HM9K](https://docs.cloudfoundry.org/concepts/architecture/#hm9k))

Diego v3的Health Check

注:
  当一个应用实例崩溃时，我们称作“HM9000”的Health Manager将会察觉并重启这个应用实例。

---

## [我的应用需要](#/8) 更多内存

```bash
$ cf events imperfect-app

description
index: 1, reason: CRASHED... Exited with status 255 (out of memory)
```

```bash
$ cf scale imperfect-app -m 256M
```

<img src="images/fill-memory.png" style="background:none; border:none; box-shadow:none;" />

---

## [我的应用需要](#/9) 更多磁盘空间

<span style="color: #FF4D4D;">请勿使用应用磁盘空间做持久化存储</span>

```bash
$ cf app imperfect-app

     state     since        cpu    memory           disk
#0   running   2015-11-02   0.0%   24.4M  of 256M   95.2M  of 256M
#1   running   2015-11-02   0.0%   112.4M of 256M   224.2M of 256M
#0   running   2015-11-02   0.0%   24.3M  of 256M   95.2M  of 256M
```

```bash
$ cf logs imperfect-app --recent

Errno::EDQUOT - Disk quota exceeded @ io_write - infinite-file:
```

```bash
$ cf scale imperfect-app -k 1G
```

<img src="images/fill-disk.png" style="background:none; border:none; box-shadow:none;" />

注:
  使用服务来做持久化存储，不要将文件存储在运行应用的容器中。

---

## 扩容 [实例个数，磁盘及内存](#/10)

在单条命令中使用多个选项

```bash
$ cf help scale

NAME:
   scale - Change or view the instance count, disk space limit...

USAGE:
   cf scale APP_NAME [-i INSTANCES] [-k DISK] [-m MEMORY] [-f]

OPTIONS:
   -i       Number of instances
   -k       Disk limit (e.g. 256M, 1024M, 1G)
   -m       Memory limit (e.g. 256M, 1024M, 1G)
   -f       Force restart of app without prompt
```

---

## 12-要素 [应用](#/11)

<a href="http://12factor.net"><img src="images/12factor.png" style="background:none; border:none; box-shadow:none;" /></a>

---

# <span style="color: #8FF541;">特性已传达</span>

```nohighlight
身为一名CF高手
我希望我的应用能够自动恢复运行状态
这样意外的错误就不会使其永远离线了
```

---

## [答](#/13) 疑?

> 提问必须正经严肃，解答可以风趣幽默。

---

# CF 高手进阶

  * 阅读[12-要素应用](http://12factor.net/)
  * 对应用进行[负荷冲击](https://loadimpact.com/) 测试(1-2个实例崩溃)
  * 阅读[在CF上实现Blue-Green部署](http://garage.mybluemix.net/posts/blue-green-deployment/)
  * 使用[cf-blue-green-deploy](https://github.com/bluemixgaragelondon/cf-blue-green-deploy) cli插件

<p style="font-size: 50%; opacity: 0.2;">
  本文版权归CloudCredo所有。 &copy; CloudCredo 2015. 保留一切权利。
</p>
