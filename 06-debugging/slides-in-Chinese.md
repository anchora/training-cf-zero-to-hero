## Cloud Foundry <br />从入门到精通
### [06 如何调试应用?](#/0)

<p style="font-size: 50%; opacity: 0.2;">
  本文版权归CloudCredo所有。 &copy; CloudCredo 2015. 保留一切权利。
</p>

---

# [特性](#/1)

```nohighlight
身为一名CF高手
我得知道我的应用究竟在干什么
这样我便可以进行调试了
```

---

## [部署](#/2)一个带bug的应用

```bash
# 切换至training主目录:
$ cd 06-debugging/debug-app
$ cf push

...

urls: debug-app-unerring-muddlehead.cfapps.io

...
```

<img src="images/500-index.png" style="background:none; border:none; box-shadow:none;" />

---

## [如何](#/3)调试我的应用[?](#/3)

1. App logs
1. App events
1. App instrumentation
1. SSH access*

注:
  Diego SSH在PWS上并未完全集成

---

## [1. 应用](#/4)logs

```bash
$ cf logs debug-app --recent

...

... [App/0] ERR ...  - RuntimeError - I am a bug, fix me:
... [App/0] ERR /home/vcap/app/config.ru:20:in `block in <class:Web>
```

```bash
$ cf logs debug-app # Tails app logs, CTRL + C to exit
```

---

## [一起来](#/5)修复这个bug

```bash
$ cf set-env debug-app FIXED true
$ cf restart debug-app
```

<img src="images/200-index.png" style="background:none; border:none; box-shadow:none;" />

---

## [2. 应用](#/6)events

```bash
$ cf events debug-app

... description
... index: 0, reason: CRASHED, exit_description: 2 error(s) ...
...
```

注:
  注意最近的事件位于顶部

---

## [3. 应用](#/7)instrumentation

* New Relic
* AppDynamics

> 包含在Java buildpack中

---

## [New Relic](#/8) instrumentation

```bash
$ cf create-service newrelic standard newrelic
$ cf bind-service debug-app newrelic
$ cf env debug-app
# Find your New Relic license key
```

```bash
# From the training home directory:
$ cd 06-debugging/debug-app
# Replace YOUR-LICENSE-KEY
$ vim newrelic.yml
```

```bash
$ cf push
```

```bash
$ cf service newrelic
```

注:
  新建一个New Relic服务实例

  为应用提供New Relic许可密钥

  访问New Relic Dashboard页面

  访问应用，产生些应用负载以查看效果

---

## [4.](#/9) SSH [访问](#/9)*

```bash
$ cf logout
$ cf login -a api.run.pivotal.io -u YOUR-EMAIL-ADDRESS --sso
# Use temp auth code from https://login.run.pivotal.io/passcode
$ cf ssh debug-app
```

<img src="images/cf-ssh.png" style="background:none; border:none; box-shadow:none;" />

注:
  PWS上暂未公开宣布

---

# <span style="color: #8FF541;">特性已传到</span>

```nohighlight
身为一名CF高手
我得知道我的应用究竟在干什么
这样我便可以进行调试了
```

---

## [答](#/11) 疑?

> 提问必须正经严肃，解答可以风趣幽默。

---

# CF 高手进阶

  * 为应用设置[Skylight](https://www.skylight.io/)
  * 为应用设置[Opbeat](https://opbeat.com/)
  * 学习[CF日志与计量](http://www.cfsummit.com/sites/cfs2015/files/pages/files/cfsummit15_king.pdf)
  * 将应用日志发送到[Papertrail](https://papertrailapp.com/)

```bash
$ cf cups logdrain -l syslog://YOUR-PAPERTRAIL-LOG-DESTINATION
$ cf bind-service debug-app logdrain
# Check your Papertrail Events, no need to restart the app
```

<p style="font-size: 50%; opacity: 0.2;">
  本文版权归CloudCredo所有。 &copy; CloudCredo 2015. 保留一切权利。
</p>
