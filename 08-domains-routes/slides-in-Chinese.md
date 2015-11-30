## Cloud Foundry <br />从入门到精通
### [08 如何使用自定义域名?](#/0)

<p style="font-size: 50%; opacity: 0.2;">
  本文版权归CloudCredo所有。&copy; CloudCredo 2015. 保留一切权利。
</p>

注:
  把最好的留到最后...

---

## [Cloud Foundry高手](#/2) 技能

> Cloud Foundry高级知识

注:
  首先...

---


# [正片](#/2)

```nohighlight
作为一个CF高手
我希望我的应用可以通过我自己的域名访问
这样所有人都能看到我的Cloud Fondry高手技能
```

---

## [如何使用](#/3) 自定义域名[?](#/3)

```bash
$ cf create-domain cf-hero-YOUR-NAME YOUR-NAME.cf-hero.cloudcredo.io
```

```bash
# 从培训文档的home目录:
$ cd 08-domains-routes/cf-hero
# 去掉域名注释并替换为你自己的域
$ vim manifest.yml
```

```bash
$ cf push
```

<img src="images/cf-hero.png" style="background:none; border:none; box-shadow:none;" />

注:
  我们可以给CF的org添加自定义域名。

---

## [我希望](#/4) 多个路由<br />指向同一个应用

```bash
$ cf map-route cf-hero gerhard.cf-hero.cloudcredo.io -n www
```

```bash
$ cf apps

name     ..  urls
cf-hero  ..  gerhard.cf-hero.cloudcredo.io, www.gerhard.cloudcredo..
```

<img src="images/www-cf-hero.png" style="background:none; border:none; box-shadow:none;" />

---

## [我还希望](#/5) 多个域名<br />指向同一个应用

```bash
$ cf domains

name                            status
cfapps.io                       shared
gerhard.cf-hero.cloudcredo.io   owned
```

```bash
$ cf map-route cf-hero cfapps.io -n gerhard-cf-hero
```

```bash
$ cf routes

space         host              domain                          apps
development                     gerhard.cf-hero.cloudcredo.io   ...
development   www               gerhard.cf-hero.cloudcredo.io   ...
development   gerhard-cf-hero   cfapps.io                       ...
```

<img src="images/cfapps-cf-hero.png" style="background:none; border:none; box-shadow:none;" />

注:
  每一个完整的Cloud Foundry环境都至少有一个共享的域名

  所有部署在Cloud Foundry上的应用共享这个域名

  尽管我们自定义的域名没有对应主机，共享域名不能没有主机

  共享域名有保留的主机，如api

---

## [如何实现](#/6) 多 <br />域名 &amp; 路由[?](#/6)

```bash
$ cat imaginary-manifest.yml

name: cf-hero
domains:
  - cloudcredo.io
  - cloudcredo.com
hosts:
  - gerhard
  - gerhard-lazu
```

```bash
$ cf routes

space          host            domain            apps
development    gerhard         cloudcredo.io     cf-hero
development    gerhard-lazu    cloudcredo.io     cf-hero
development    gerhard         cloudcredo.com    cf-hero
development    gerhard-lazu    cloudcredo.com    cf-hero
```

---

## [尝试](#/7) 应用版本替换

```bash
$ cf push -f manifest-superhero.yml
```

当这个应用完成了它的工作...

```bash
$ cf delete -f -r cf-superhero
```

<img src="images/cf-superhero.png" style="background:none; border:none; box-shadow:none;" />

---

## [当你发现](#/8) <br />应用的一个更好的版本

```bash
# 从培训文档的home目录:
$ cd 08-domains-routes/cf-hero-static
# 替换成你自己的应用名
$ vim public/index.html
# 去掉域名注释并替换为你自己的域名
$ vim manifest.yml
```

```bash
$ cf push
$ cf app cf-hero-static

     state     since        cpu    memory        disk
#0   running   2015-11-02   0.0%   6.8M of 16M   33.6M of 64M
...
#3   running   2015-11-02   0.0%   6.7M of 16M   33.5M of 64M
```

```bash
$ cf delete -f cf-hero
```

> 使用蓝绿部署软件升级模式

---

## [你现在已经是一个](#/9) <br />Cloud Foundry高手了

<br />

<img src="images/cf-hero-static.png" style="background:none; border:none; box-shadow:none;" />

---

# <span style="color: #8FF541;">布道完成</span>

```nohighlight
作为一个CF高手
我希望通过自己的域名访问我的应用
这样每个人都能看到我的Cloud Fondry高手技能
```

---

## [Any](#/11) 提问?

> 问题必须正经严肃，但回答可以幽默诙谐。

---

# CF大牛进阶

  * 设置自定义 [SSL 认证](http://www.selfsignedcertificate.com/)
  * 使用 [特性标志位](https://docs.cloudfoundry.org/adminguide/listing-feature-flags.html) 代替环境变量
  * 删除所有不再使用的路由

```bash
$ cf delete-orphaned-routes
```

<p style="font-size: 50%; opacity: 0.2;">
  本文版权归CloudCredo所有。 &copy; CloudCredo 2015. 保留一切权利。
</p>
