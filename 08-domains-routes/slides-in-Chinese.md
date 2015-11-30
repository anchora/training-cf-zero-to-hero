## Cloud Foundry <br />从入门到精通
### [08 如何使用自定义域名?](#/0)

<p style="font-size: 50%; opacity: 0.2;">
  本文版权归CloudCredo所有。 &copy; CloudCredo 2015.保留一切权利。
</p>

注:
  好东西总要留到最后...

---

## [Cloud Foundry英雄](#/2)徽章

> 出众的Cloud Foundry专业知识

注:
  但是首先...

---


# [特性](#/2)

```nohighlight
身为一名CF高手As a CF hero
我希望我的应用可以通过自己提供的域名访问I want my app to be accessible at my domain
这样便能让所有人见识下我的Cloud Foundry应用徽章了So that everyone can see my Cloud Foundry Hero Badge
```

---

## [如何使用](#/3)自定义域名[?](#/3)

```bash
$ cf create-domain cf-hero-YOUR-NAME YOUR-NAME.cf-hero.cloudcredo.io
```

```bash
# From the training home directory:
$ cd 08-domains-routes/cf-hero
# Uncomment domain & replace YOUR-NAME
$ vim manifest.yml
```

```bash
$ cf push
```

<img src="images/cf-hero.png" style="background:none; border:none; box-shadow:none;" />

注:
  我们可以在CF org中添加自定义域名。

---

## [我想让](#/4) 多个路由(routes) <br />指向同一个应用

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

## [我还想让](#/5) 多个域名(domains) <br />指向同一个应用

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
  每个Cloud Foundry环境至少拥有一个共享域名(shared domain)。

  此域名是由部署于该Cloud Foundry之上的所有应用共享使用的。

  尽管我们在使用自定义域名时可以不指定host名，但使用共享域名时必须指定host名。

  共享域名有一些保留的host名，比如api等。

---

## [应用绑定]多个 <br />domains &amp; routes[会怎样?](#/6)

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

## 应用版本[实验](#/7)

```bash
$ cf push -f manifest-superhero.yml
```

完成这个步骤后...

```bash
$ cf delete -f -r cf-superhero
```

<img src="images/cf-superhero.png" style="background:none; border:none; box-shadow:none;" />

---

## [当你发现了](#/8) <br />应用的最优版本

```bash
# 切换至training主目录:
$ cd 08-domains-routes/cf-hero-static
# 替换 YOUR-NAME
$ vim public/index.html
# 取消 domain & replace YOUR-NAME 这一行的注解
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

> 意外的 blue/green 部署

---

## [现在，你已经是一个](#/9) <br />Cloud Foundry英雄了

<br />

<img src="images/cf-hero-static.png" style="background:none; border:none; box-shadow:none;" />

---

# <span style="color: #8FF541;">特性已传达</span>

```nohighlight
身为一名CF高手As a CF hero
我希望我的应用可以通过自己提供的域名访问I want my app to be accessible at my domain
这样便能让所有人见识下我的Cloud Foundry应用徽章了So that everyone can see my Cloud Foundry Hero Badge
```

---

## [答](#/11) 疑?

> 提问必须正经严肃，解答可以风趣幽默。

---

# CF 高手进阶

  * 设置一个自定义的[SSL certificate](http://www.selfsignedcertificate.com/)
  * 使用[feature flags](https://docs.cloudfoundry.org/adminguide/listing-feature-flags.html)来替代ENV vars
  * 删除所有不再使用的路由

```bash
$ cf delete-orphaned-routes
```

<p style="font-size: 50%; opacity: 0.2;">
  本文版权归CloudCredo所有。 &copy; CloudCredo 2015. 保留一切权利。
</p>
