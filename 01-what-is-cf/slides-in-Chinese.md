## Cloud Foundry <br />从入门到精通
### [01 什么是Cloud Foundry?](#/0)

<p style="font-size: 50%; opacity: 0.2;">
  本文版权由CloudCredo所有。 &copy; CloudCredo 2015.保留一切权利。
</p>

---

## [我们为何相聚于此?](#/1)

  1. 去发掘Cloud Foundry
  1. 团队协作，亲身实践
  1. 成为[玩转Cloud Foundry的高手](#/1)

---

## [你将会学到:](#/2)

  1. 什么是Cloud Foundry?
  1. 如何与CF交互?
  1. 怎样将应用部署到CF上?
  1. 什么是buildpacks?
  ---

  午餐&午休

  ---
  1. CF如何使我的应用快速恢复?
  1. 如何调试CF应用?
  1. CF应用间的共享状态是如何管理的?
  1. 如何使用自定义域名?

---

## [好学者有疑](#/3) <br /> 但问无妨

> 问题必须正经严肃，但回答可以幽默诙谐。

---

## [我们的每个](#/4)主题 [<br />都有各自的](#/4)特色

---

## [为什么本文要由](#/5) 一整个团队的人 [来讲授呢？](#/5)

---

## U盘里的[文件](#/6)

  * 本次课程的幻灯片
  * 示例应用
  * 其他课程的资料

---

## [在线](#/7)幻灯片

http://slides.cf-hero.cloudcredo.io

---

## [想要](#/8)挑战自我吗？

### [那就试着成为一名](#/8) CF 大牛吧！

---

## [什么是](#/9) Cloud Foundry[?](#/9)

  * 一个开放的 _Platform as a Service_ (_PaaS_)
  * 能够快速便捷地构建、测试、部署、伸缩应用
  * 支持众多*语言及框架

> 了解更多 [docs.cloudfoundry.org](https://docs.cloudfoundry.org/concepts/overview.html)

注:
  托管于GitHub，使用Apache开源许可。

  开发者使用cf命令行工具与CF实例交互。cf命令行工具提供Windows，Mac及Linux的预构建版本。

  CF通过buildpacks来支持各种语言及框架。后续课程会作详细介绍。

---

## [什么是](#/10) Platform as a Service[?](#/10)

  * <span style="color: #8FF541;">云计算平台</span>按其特征可划分为  **IaaS**，**PaaS**，以及 **SaaS**
  * 应用类似于 **PaaS** 的货币单位
  * Heroku开辟了**PaaS**的先河

注:
  IaaS - 服务器， SaaS - 用户。

  Heroku基于其风靡一时的所谓“12要素”经验创造了这种模式，“buildpack”也深受影响。
  Heroku created patterns based on their experiences called "12 Factors" that have been heavily influencial. Also drove "buildpack" model

---

## [为什么要用](#/11) Platform as a Service[?](#/11)

  * 快速传递商业价值
  * 只需专注于业务
  * 快速试错、成本低廉，促进学习

注:
  直接将用户所需呈现其眼前，通过直击痛点不断提高。
  
  不用等待IT部门筹备服务器。
  
  允许开发者选择他认为最适合这样工作的语言及服务。

---

## [Cloud Foundry](#/12) 基金会

  * EMC
  * HP
  * IBM
  * Intel
  * Pivotal
  * SAP
  * Swisscom
  * VMware
  * ... 45个公司和组织，并不断壮大中！


---

## CF[有哪些](#/13)不同的类别[?](#/13)

  1. 开源软件
  1. 供应商发行版
  1. 私有云服务
  1. 共有云服务

---

## [1.](#/14) 开源 [软件](#/14)

  * [github.com/cloudfoundry](https://github.com/cloudfoundry)
  * **cf-release** 需要有 [BOSH](https://bosh.io/docs) 基础
  * **bosh-lite** 适用于本地CF实例部署

> 千载难逢的学习良机

注:
  良机千载难逢，仍须身体力行，冰冻三尺非一日之寒，入木三分非一日之功。
  BOSH有非常陡峭的学习曲线，上手难度大。
  通过bosh-lite部署CF要求 10GB 可用内存。
  Cloud Credo等公司专业提供CF部署、优化及定制服务。
  如果您对这项工作怀抱兴趣，欢迎联系我们。

---

## [2.](#/15) 供应商 [发行版](#/15)

  * Pivotal Cloud Foundry
  * HP Helion Development Platform

注:
  Pivotal Cloud Foundry分发版即熟知的Pivotal弹性运行时。
  Pivotal最早介入CF，经验最为丰富。
  Pivotal对开源CF贡献巨大。
  Pivotal构建并开源了一系列有状态的服务，诸如MySQL、Cassandra、Redis、RabbitMQ等，这些服务与CF结合使用效果非常好。

  HP Helion开发平台基于Stackato Cloud Foundry发行版。
  HP已在2015年夏天收购Stackato。

---

## [3.](#/16) 私有云 [服务](#/16)

  * CloudCredo Platform
  * IBM Bluemix Dedicated
  * CenturyLink Private Cloud
  * Canopy Enterprise Private Cloud
  * HP Helion Development Platform

注:
  CloudCredo Platform允许你通过一个简单的界面部署一个CF实例，以及诸如Kubernates等一系列服务。您只需支付标准的基础设施费用，完全是免费使用。
  
  IBM Bluemix包括CF及一些IBM硬件上的服务，很受银行青睐。 

  托管于世纪互联全球范围内的各个数据中心。

  Canopy Cloud，也称作Atos Cloud.

  HP Helion Rack基于Stackato CF，部署在运行于HP硬件上的OpenStack之上。 

---

## [4.](#/17) 公有云 [服务](#/17)

  * Anynines
  * Swisscom（瑞士电信）
  * IBM Bluemix
  * Pivotal Web Services

注:
  Anynines总部位于德国萨尔布吕肯，专业提供诸如MongoDB、Redis、ElasticSearch及MySQL等有状态服务的优质计划。

  瑞士电信开发者门户于2015年十月二日开放，官网宣称其为百分百的瑞士云解决方案。
  
  IBM Bluemix运行在Softlayer位于德克萨斯及伦敦的主机之上。IBM Bluemix集成了许多IBM特有的服务，比如Watson、Cloudant、物联网等，并且提供了非常实惠的免费套餐计划。另外，您还可以通过同一个管理界面访问IBM提供的容器及虚拟机。
  
  Pivotal Web Services历史最为悠久，服务运行于AWS之上。

---

# [正片](#/18)

```nohighlight
身为一名CF高手
我得有一个CF账号
这样我就不用再顾虑基础设施这类问题了
```

---

https://run.pivotal.io

<a href="https://run.pivotal.io"><img src="images/pws-index.png" style="background:none; border:none; box-shadow:none;" /></a>

---

<a href="https://console.run.pivotal.io"><img src="images/pws-console.png" style="background:none; border:none; box-shadow:none;" /></a>

---

# <span style="color: #8FF541;">布道完成</span>

```nohighlight
身为一名CF高手
我得有一个CF账号
这样我就不用再顾虑基础设施这类问题了
```

---

## [有什么](#/22)疑问吗 ?

> 还是那句话，问题必须正经严肃，但解答可以幽默诙谐

---

# CF大牛进阶

CF公有云服务比较:
  * [Anynines](http://www.anynines.com/pricing)
  * [Swisscom](https://developer.swisscom.com/pricing)
  * [IBM Bluemix](https://console.ng.bluemix.net/pricing/)
  * [Pivotal Web Services](https://run.pivotal.io/pricing)

CF架构比较:
  * [DEA v2](https://docs.cloudfoundry.org/concepts/architecture/)
  * [Diego v3](https://docs.cloudfoundry.org/concepts/diego/diego-architecture.html)

<p style="font-size: 50%; opacity: 0.2;">
  本文版权归CloudCredo所有。 &copy; CloudCredo 2015，保留一切权利。
</p>
