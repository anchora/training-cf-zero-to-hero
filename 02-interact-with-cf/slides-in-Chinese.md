## Cloud Foundry <br />From Zero To Hero
### [02 如何与CF交互?](#/0)

<p style="font-size: 50%; opacity: 0.2;">
  本文版权归CloudCredo所有。 &copy; CloudCredo 2015.保留一切权利。
</p>

---

# [正片](#/1)

```nohighlight
身为一名CF高手
我得有命令行工具这把利剑在手
这样我便可以把应用部署到我的CF账户下了
```

---

## [如何](#/2)与 CF 进行交互[?](#/2)

通过带有CF CLI（命令行接口）工具的终端/命令提示符


注:
  CF CLI与名为Cloud Controller的组件通信，这个组件对外暴露了一组RESTful API。

  好物当如是: 使用CF CLI，从终端操作。

  某些流行的IDE附带有CF插件，您可以通过IDE将应用直接部署到CF之上。

---

## [如何](#/3)  安装CF CLI[?](#/3)

[github.com/cloudfoundry/cli](https://github.com/cloudfoundry/cli#downloads)

<img src="images/cf-cli-downloads.png" style="background:none; border:none; box-shadow:none;" />


注:
  CLI v6用Go语言重写，并且包装成一个独立的二进制文件。

  推荐使用Installer安装CLI，更简单且不易出错。

  点击GitHub页面第一栏的 Stable Installers.
---

## [如何确认](#/4) CF CLI [已正确安装?](#/4)

```bash
$ cf
NAME:
   cf - A command line tool to interact with Cloud Foundry

USAGE:
   [environment variables] cf [global options] command [arguments...

VERSION:
   6.13.0-e68ce0f-2015-10-15T22:53:29+00:00

BUILD TIME:
   2015-10-25 01:04:16.22807612 +0100 BST
...
```

注:
  Windows系统下打开命令提示符方法：按下Windows键（或者同时按下Ctrl+R），输入“cmd”，回车确认即可。
  
  输入“cf”，不带任何命令或参数，即可查看所有支持的命令列表。

  切勿输入“$”符号！

---

## [如何](#/5) 登录 CF[?](#/5)

```bash
$ cf login -a api.run.pivotal.io -u YOUR-EMAIL-ADDRESS
```

```bash
$ cf help login
$ cf login -h
NAME:
   login - Log user in

USAGE:
   cf login [-a URL] [-u USERNAME] [-p PASSWORD] [-o ORG] [-s SPACE]
...
```

注:
  我们已知输入“cf”会列出所有支持的命令。
  
  cf help login (或者其他支持的命令) 则会显示这条特定命令的用法等详细信息。

  cf login -h 效果同上。

  CF API endpoint & username from Pivotal CF

---

## [什么是](#/6) CF space[?](#/6)

每个应用及服务都隶属于一个space。

```bash
$ cf spaces

slides
training
```

```bash
$ cf space training

development
    Org:               cf-hero
    Apps:              
    Domains:           
    Services:          
    Security Groups:   public_networks, dns, ssh-logging, p-mysql...
    Space Quota:
```

注:

  如果还未创建空间，则无法创建应用或服务。

  space为一组用户提供了一个共享的应用开发环境。

  每个org必须包含至少一个space。

  如果某个org仅有一个space，则用户登录后会自动定位到这个space。

  如果某个org有多个space，则需要用户显式选择指定的space。

  查看你的CF space信息。

---

## [什么是](#/7) CF org[?](#/7)

由一个或多个用户共享的账户，共享内容包括资源配额（resource quotas）、应用（applications）、服务(services)、自定义域名(custom domains)。

```bash
$ cf orgs

name
cf-hero
```

```bash
$ cf org cf-hero

cf-hero:
    domains:        
    quota:          100GB:50free (102400M memory limit, Unlimited ..
    spaces:         slides, training
    space quotas:
```

---

## [什么是](#/8) 资源配额(resource quotas)[?](#/8)


```bash
$ cf quotas

name           total mem   instance mem   routes   service instances
free           0           unlimited      1000     0                
trial          2G          unlimited      1000     10               
paid           10G         unlimited      1000     unlimited        
25GB           25G         unlimited      1000     unlimited        
50GB           50G         unlimited      1000     unlimited        
75GB           75G         unlimited      1000     unlimited        
100GB          100G        unlimited      1000     unlimited        
...
```

---

## orgs, spaces 及 apps[之间的关系](#/9) [?](#/9)

```nohighlight

              . Org                   . cf-hero
              |                       |
              \-- Space               \-- training
                 |                       |
                 |-- App 1               |-- web-app
                 \-- App 2               \-- worker-app
.
```

---

## [我当前正](#/10) 定位[在哪个org，哪个space?](#/10)

```bash
$ cf target

API endpoint:   https://api.run.pivotal.io (API version: 2.36.0)
User:           gerhard@cloudcredo.com
Org:            cf-hero
Space:          training
```

---

## [如何从当前org或space](#/11) 切换 [至其他的org或space?](#/11)

```bash
$ cf target -o ORG -s SPACE
```

注:
  如果您想挑战自我，请参阅《使用“多重定位”高手进阶》。
---

# <span style="color: #8FF541;">DELIVERED</span>

```nohighlight
身为一名CF高手
我得有命令行工具这把利剑在手
这样我便可以把应用部署到我的CF账户下了
```

---

## [答](#/13) 疑?

> 提问必须正经严肃，解答可以风趣幽默。

---

# CF 高手进阶

  * 将坐在您身旁的小伙伴添加到您的PWS space。
  * 在[IBM Bluemix](https://console.ng.bluemix.net/registration/)上新建一个账户。
  * 在PWS及Bluemix使用[Targets cf cli 插件](https://github.com/guidowb/cf-targets-plugin)。
  * 向[cf cli](https://github.com/cloudfoundry/cli/blob/master/cf/i18n/README-i18n.md)提交一个本地化拉取请求。

<p style="font-size: 50%; opacity: 0.2;">
  本文版权归CloudCredo所有。 &copy; CloudCredo 2015. 保留一切权利。
</p>
