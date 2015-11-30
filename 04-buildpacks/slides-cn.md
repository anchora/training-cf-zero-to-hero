## Cloud Foundry <br />从无名，到英雄
### [04 什么是 buildpacks?](#/0)

<p style="font-size: 50%; opacity: 0.2;">
  本文版权归CloudCredo所有。 &copy; CloudCredo 2015. 保留一切权利。
</p>

---

# [特色](#/1)

```nohighlight
身为一名CF高手
我需要一个简单的静态网站
这样我便可以专注于构建我的产品了
```

---

## [什么是](#/2) buildpacks[?](#/2)

Cloud Foundry组件之一， <br />用于解析app的运行时依赖。

---

## [为什么使用](#/3) buildpacks[?](#/3)

  * 简化应用部署-只需专注于代码
  * 更少文件，应用部署更快捷
  * 生成可独立运行的app加工品(droplets)

---

## buildpack [做了什么?](#/4)

  * 输入为应用代码
  * 检查应用并满足依赖
  * 输出是droplet
  * 元数据输出(Metadata output)定义了环境变量(ENV vars)及启动命令(start command)

> 每个buildpack都会参与甄选

---

## buildpack [如何工作?](#/5)

  1. `bin/detect`
  1. `bin/compile`
  1. `bin/release`

---

## buildpack [运行于何处?](#/6)

  * 使用带有rootfs的主机内核(jeos)
  * 默认的rootfs是<span style="color: #8FF541;">cflinuxfs2</span> (基于Ubuntu 14.04 Trusty)
  * Buildpack执行及app运行时位于容器内
  * Cloud Foundry使用[Garden](https://github.com/cloudfoundry-incubator/garden)实现容器化

---

## [Cloud Foundry](#/7) <br />部署流程 <br />[回顾](#/7)

  1. 开发者push应用
  1. buildpacks按序检查对应用的兼容性
  1. <span style="color: #8FF541;">获胜的</span> buildpack执行compile及release (staging)
  1. 生成的droplet存储于blobstore中
  1. 应用运行即将droplets部署到容器中

---

## buildpacks[有哪些分类?](#/8)

  1. 默认buildpacks
  1. 社区buildpacks
  1. Heroku buildpacks
  1. 自定义buildpacks

> 在线(Online)与离线(Offline)

---

## [1.](#/9) 默认 [buildpacks](#/9)

```bash
$ cf buildpacks

buildpack              position   filename
ruby_buildpack         1          ruby_buildpack-cached-v1.6.7
nodejs_buildpack       2          nodejs_buildpack-cached-v1.5.0
java_buildpack         3          java-buildpack-v3.2
go_buildpack           4          go_buildpack-cached-v1.6.2
liberty_buildpack      5          liberty_buildpack
python_buildpack       6          python_buildpack-cached-v1.5.1
php_buildpack          7          php_buildpack-cached-v4.1.4
staticfile_buildpack   8          staticfile_buildpack-cached-v1.2..
binary_buildpack       9          binary_buildpack-cached-v1.0.1
```

---

## [2.](#/10) 社区 [buildpacks](#/10)

[github.com/cloudfoundry-community](https://github.com/cloudfoundry-community/cf-docs-contrib/wiki/Buildpacks)

---

## [3.](#/11) Heroku [buildpacks](#/11)

  * CF buildpacks基于Heroku buildpacks
  * 它们可以相互替换 (大多数)

---

## [4.](#/12) 自定义 [buildpacks](#/12)

  * 若您要使用某种特定的编程语言，则需要相应的buildpack
  * 繁简由您决定

---

## 静态 [buildpack](#/13)

```bash
# 切换至training主目录:
$ cd 04-buildpacks/static-app
$ cf push
```

```bash
$ cf app static-app

     state     since        cpu    memory        disk
#0   running   2015-11-02   0.0%   6.5M of 16M   33.6M of 64M
```

<img src="images/index.png" style="background:none; border:none; box-shadow:none;" />

---

## [轻松]扩容app(#/14)

```bash
$ cf scale static-app -i 32
```

```bash
$ cf app static-app

     state      since        cpu    memory        disk
#0   running    2015-11-02   0.0%   6.5M of 16M   33.6M of 64M
#1   starting   2015-11-02   0.0%   0 of  16M     0 of 64M
#2   running    2015-11-02   0.0%   6.9M of 16M   33.5M of 64M
...
#30   running   2015-11-02   0.0%   6.8M of 16M   33.5M of 64M
#31   running   2015-11-02   0.0%   7M of 16M     33.6M of 64M
```

---

# <span style="color: #8FF541;">特性已传达</span>

```nohighlight
身为一名CF高手
我需要一个简单的静态网站
这样我便可以专注于构建我的产品了
```

---

## [答](#/16) 疑?

> 提问必须正经严肃，解答可以风趣幽默。

---

# CF 高手进阶

  * 编写一个[caddy HTTP/2 server](https://caddyserver.com/)的[自定义 buildpack](https://docs.cloudfoundry.org/buildpacks/custom.html)
  * 使用上述自定义的caddy buildpack部署static-app

<p style="font-size: 50%; opacity: 0.2;">
  本文版权归CloudCredo所有。 &copy; CloudCredo 2015. 保留一切权利。
</p>
