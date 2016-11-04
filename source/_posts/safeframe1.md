---
title: SafeFrames v1.1
date: 2016-08-24 00:37:00
tags: [SafeFrame,iab,iframe]
categories: 
 - 译文
---

# **iab.(Interactive Advertising Bureau)** 
### **安全框架**
### 版本1.1 草案
#### 2014年8月发布


----------

**这个文档是IAB广告技术委员会开发的**

SafeFrame规范是由来自21个IAB成员公司的志愿者组成的工作小组开发的。

<!--more-->
SafeFrame工作小组的领导者是：

 - 肖恩·斯奈德，雅虎公司（Sean Snider, Yahoo!）
 - 普拉巴卡尔·戈亚尔，微软公司（Prabhakar Goyal, Microsoft）

为这个文档作出贡献的IAB成员公司有：

 - Adobe Systems Inc.
 - AOL & ADTECH
 - Auditude
 - C3 Metrics
 - CBS Interactive
 - Disney Interactive Media Group
 - Dotomi
 - Editorial Projects in Education
 - FDG
 - FreeWheel
 - Google
 - HealthiNation
 - Media Rating Council - MRC
 - Microsoft
 - NBC Universal Digital Media
 - Network Advertising Initiative - NAI
 - OpenX Limited
 - Time Inc.
 - Turner Broadcasting System, Inc./CNN.com
 - Undertone
 - Yahoo!

**IAB中这项倡议的带领者是克里斯·梅希亚（Chris Mejia）和凯蒂·斯特劳德（Katie Stroud ）**

对本文档进行评论请联系adtechnology@iab.net。请一定要提及这个文件的版本号（你可以在此页的右下角找到，v1.1）。

知识产权事项：参与SafeFrame1.0工作小组的公司在生产SafeFrame1.0版的过程中没有作出专利的说明。SafeFrame的未来版本将在即将发布的IAB知识产权政策的主持下进行生产。

SafeFrame倡议的更多细则和资源可以在http://www.iab.net/safeframe上被找到

**关于IAB广告技术委员会**

广告技术委员会由超过70个在广告技术有主要的业务的IAB成员公司组成。它与IAB广告经营理事会合作，为数字广告行业制定重要技术标准和实践最佳做法。一群精选的主要成员企业也参与到广告技术领导委员会里，建议IAB的管理层和董事会顶级广告技术优先。


广告技术委员会的使命是开发那些将降低成本，开拓新的市场机会，并确保数字广告行业的长期增长的技术规范和要求，并促进其被采用。

要查看委员会成员公司的完整列表，请移步到：
http://www.iab.net/advertising_technology_council

----------
## 目录
　执行摘要
　目标受众
　规格更新

**1. 概况**
　　1.1　SafeFrames组件
　　　1.1.1　主站(The Host)
　　　1.1.2　第三方(The External Party)
　　　1.1.3　接口
　　　1.1.4　二级域名(The Secondary Host Domain)
　　1.2　优点
　　　1.2.1　第三方和主站之间的透明度
　　　1.2.2　统一规范的API
　　　1.2.3　"沙盒”(Sandboxing)第三方内容
　　　1.2.4　主站定制&控制
　　1.3　SafeFrames和广告可见展视
　　　1.3.1　支持3MS广告可见度
　　　1.3.2　可见度特性对第三方可选
　　1.4　SafeFrames和视频插播广告
　　1.5　SafeFrames和移动端
　　1.6　报告SafeFrame的数据
　　1.7　区别于其他规范
　　　1.7.1　IAB友好的iframe
　　　1.7.2　跨源资源共享（CORS）
　　1.8　超出范围的项(Out of Scope)
　　1.9　操作注意事项
**2. 主站实施(Host Implementations)**
　　2.1　SafeFrames是如何运作的
　　　2.1.1　传递模式A：主站转换第三方内容
　　　2.1.2　传递模式B：第三方内容直接传递
　　　2.1.3　渲染SafeFrame
　　　2.1.4　通过API进行页内通讯
　　2.2　要求
　　　2.2.1　JavaScript的host库和API
　　　2.2.2　二级域名
　　　2.2.3　资源公约
　　　2.2.4　SafeFrame的URI公约
　　2.3　实现注意事项(Implementation Notes)
　　2.4　SafeFrame的渲染细节
　　2.5　通信机制的细节
**3. SafeFrame标签**
　　3.1　SafeFrame标签结构&要求
　　　3.1.1　SCRIPT标签
　　　3.1.2　使用JavaScript处理数据标签
　　　　3.1.2.1　例子：一次性处理所有标签
　　　　3.1.2.2　例子：在数据标签之前先定义SafeFrame host库
　　　　3.1.2.3　例子：有兄弟标签的SafeFrame数据标签自动引导(SafeFrame Data Tag with Sibling Auto-Bootstrapping)
**4. 主站API实施细则**
　　4.1　命名空间 `$sf.host`
　　4.2　命名空间 `$sf.host.conf`
　　4.3　命名空间 `$sf.info`
　　4.4　类 `$sf.host.Config`
　　4.5　类 `$sf.host.PosConfig`
　　4.6　类 `$sf.host.Position`
　　4.7　类 `$sf.host.PosMeta`
　　4.8　函数 `$sf.host.boot`
　　4.9　函数 `$sf.host.status`
　　4.10　函数 `$sf.host.nuke`
　　4.11　函数 `$sf.host.get`
　　4.12　函数 `$sf.host.render`
**5. 第三方API实现**
　　5.1　命名空间 `$sf.ext`
　　5.2　函数 `$sf.ext.register`
　　5.3　函数 `$sf.ext.supports`
　　5.4　函数 `$sf.ext.geom`
　　5.5　函数 `$sf.ext.expand`
　　5.6　函数 `$sf.ext.collapse`
　　5.7　函数 `$sf.ext.status`
　　5.8　函数 `$sf.ext.meta`
　　5.9　函数 `$sf.ext.cookie`
　　5.10　函数 `$sf.ext.inViewPercentage`
　　5.11　函数 `$sf.ext.winHasFocus`

----------
### 执行摘要
SafeFrame1.0技术是一种支持API的iframe，它开辟了发布页面内容和包含iframe的第三方内容（如广告）之间沟通的线路。因为有了这条沟通线路，投放到SafeFrame的内容能够收集数据和有丰富的互动，如广告扩展。这是标准的ifram所不具备的e。

为了避免有破坏性的广告行为和网页内嵌投放广告的潜在安全风险，发布商可以选择将广告内容投放到iframe中。

iframe是一种发布商网页中的微型HTML页面。使用这种iframe，广告内容以iframe为边界被隔离且它无法访问任何关于页面的信息。因为无法访问页面内容，是一样iframe内的广告内容就无法扩展，无法动态的与主站访客进行互动，并且无法收集任何必要的能够借此判断广告效果的数据。

这种方法保护了发布商，但同时也限制了广告的能力，并因为iframes的局限性而降低了媒体库的价值。

而SafeFrame的支持API的iframe以一种受控和透明的方式，开辟了网页代码和广告内容之间的通信线路。这种通信允许丰富的互动，同时保护发布商的页面以防未知的变化，避免了损坏页面完整性的风险。

SafeFrame用于数字广告的一些主要优势包括：

 - **消费者保护**
SafeFrames和广告内容共享信息的方式是提供它具有API的iframe，因此发布商可以选择什么内容允许共享并且能够保护敏感的消费者信息，比如个人email地址，密码甚至银行信息。

 - **发布商控制权**
发布商代码和广告代码之间的隔离，使发布者能够保持对页面布局的控制，限制来自广告的干扰，同时还允许丰富的互动和有限的数据收集。使用的SafeFrame API，发布商还有能力决定哪些主站信息（如果有的话）应该暴露给广告主和厂商

 - **发布商效率**
通过实施SafeFrame, 发布者可以允许广告在一个iframe中提供丰富的交互，同时保持控制，防止广告代码破坏页面功能。允许富媒体存在于SafeFrame能够提高潜在收入同时控制运营成本。

 - **标准化的广告布局**
广告技术提供商可以规范自己的富媒体广告代码，以便它可以在任何依附SafeFrame API协议的发布商网络上运行，同时降低运营成本。

 - **支持广告可见度与其他行业计划**
SafeFrame1.0提供机制，支持3MS发展下的可见展视以及DAA的AdChoices和其他隐私的举措。事实上，SafeFrame提供了更高的隐私控制，这是以往的标准iFrame做不到的。此外，通过SafeFrames实现的透明通信为支持其他行业举措的奠定了基础。

当一些发布商已经在自己的网页上实现了这种技术，而且广告开发人员和技术供应商已经作出必要的修改（如果有要修改的话）以支持投放广告到发布商实现SafeFrames时，SafeFrames提供的这些好处就能完全实现了。

SafeFrame的工作小组已经建立了一个开源的参考实例，以鼓励在市场上被迅速采用。但是，在发布商过渡到SafeFrame1.0的过程中，广告开发者和技术供应商必须给予足够的耐心。

### 目标受众
本规范中的技术细则主要适用于想实现SafeFrames的主站所有者，和将使用SafeFrame协议的富媒体广告的广告开发者。具体来说，主站和广告内容开发人员可以使用本文件中的规范，去制定SafeFrame协议，实现主站内容和任何外部投放的广告或其他内容之间的通信。

Web技术的厂商也应该熟悉SafeFrame的规范，来决定他们是否需要进行任何修改，以支持在市场上的SafeFrame技术。

此外，SafeFrame不仅限于在数字广告中使用，它也可以被任何客户端/服务器关系使用。

### 规格更新

| 版本号 |    日期    | 概要         |
| :---- | :-------- | :----       |
| 1.0   | 3/18/2013 |  Original    |
| 1.0.1 | 4/16/2013 | Minor name corrections  |
| 1.1   | 3/14/2014 | Support for communicating whether top browser window is “in focus” (changes in 5.1 and added 5.11) |


----------
## 1. 概况
SafeFrame说明了一个创建一个包含投放到网页的HTML内容的容器的框架，并建立了一个使网页和提供的内容之间沟通的API。有了SafeFrame1.0，指定的对象和函数被用于操纵和与创建的SafeFrame容器互动，并允许丰富的互动。SafeFrames的主要用途是用SCRIPT标签或者其他标记封装外部HTML内容，同时保护主页面，以防那些会意想不到地无意或故意影响到主站的内容。

作为一种对于投放到iframe的内容的解决方案，SafeFrame为广告内容提供了广告可见度和功能性，这是以往的标准的iframe做不到的。

### 1.1 SafeFrames组件
SafeFrame管理两方之间的互动：主站和第三方。主站拥有一个域名，在其中显示第三方提供的内容比如广告。此外，SafeFrame与一个二级域名进行交互。第三方内容投放到这个域名，并从那里通过受控的SafeFrame API功能与主站进行交互。

#### 1.1.1 主站(The Host)
本文档中，主站是一个主站拥有的某个域（或一组域），通常在这上面会显示内容给通过Web浏览器方式访问页面的终端用户。在网络广告中，主站是"发布商"的代名词，但在其他行业它可能有差异。主站负责部署SafeFrame框架，包括API和一系列用于SafeFrame的静态资源（JavaScript的™和HTML文件）。主站还负责渲染投放到SafeFrame容器的第三方内容。

#### 1.1.2 第三方(The External Party)
本文档中，第三方是一个来源于主域名之外的内容提供者。它提供内容，或者重定向到内容的数据。在网络广告中，第三方可能是广告服务器，广告交换平台(ad exchange)，广告联盟(ad network)或任何推动广告到主站的技术组织。

在许多情况下，第三方内容提供商可能完全不需要修改内容代码，但是当内容需要以某种方式与主站进行交互如内容扩展的时候，第三方就需要用一个JavaScript格式的标签把SafeFrame API的细节包括在内。

> **通用注意事项** 
> 如果第三方内容需要与它投放的主站进行互动，SafeFrame1.0所提供的容器管理技术只需要对第三方内容修改代码。例如，任何扩展或浮动的行为都需要修改一些代码，并且必须在JavaScript中进行。而限制于SafeFrame容器里的任何丰富的交互不需要修改。
> 

#### 1.1.3 API
SafeFrame规范了一个提供主站和第三方内容之间的通信协议的API。使用该API，主机主站可以在必要时给第三方内容提供信息，第三方内容可以向主站请求服务（即扩展）。

随着实施和行业应用，我们希望其他行业规范和举措将扩展SafeFrame API的功能。例如，为了支持由3MS举措正在开发的广告可见展视，当前的规范提供了构建模块。而且，该规范也许会扩展，以支持DAA的广告选择方案。

#### 1.1.4 二级域名
SafeFrame在一个由主站(the host site)提供的二级域名中运作。这个二级域名最好建立在内容分发网络（CDN）上，以提高性能和可用性。这个二级域名作为主站与第三方之间的未知空间（就是作为主站与第三方内容的隔离区）。第三方需要了解主站域(the Host domain)的信息可以通过请求访问到，通过使用SafeFrame API获得。

### 1.2 优点
SafeFrame给拥有者和主站或者第三方内容的运营商提供益处。

#### 1.2.1 第三方和主站之间的透明度
SafeFrame提供主站和第三方内容之间共享信息的机制。可以被共享的信息的一些可能性包括：主站指定的元数据，SafeFrame容器的几何位置，以及SafeFrame容器是否在视图里等等。

### 1.2.2 统一规范的API
随着行业应用，让所有广告商和其他第三方内容提供商可以与主站进行通信的公共API，是让两者之间的互动更清晰的基础。

### 1.2.3 "沙箱化”第三方内容
SafeFrame把内容渲染到一个实现第三方内容和主站内容之间的明确划分的容器。这个障碍形成了一个“沙箱”，它可以自动给主站一些安全感，并提供以下功能：

 - **基础安全**
实质上，SafeFrame是一个有API的iframe，该API实现主站和第三方内容之间通信，并与iframe一样提供相同的基础安全保证。虽然API开启了双方的通信，但主站会控制什么样的信息（如果有的话）能被访问或共享，以及分享给谁。

 - **稳定性**
就标准的内部框架来说，SafeFrame里主站和第三方内容之间的明确划分，降低了在渲染内容时发生错误的可能，以及对主站的JavaScript和HTML代码内容干扰的机会。由于第三方内容是由它自己的HTML文档，自己的一套CSS规则，以及JavaScript渲染的，它不能直接交互或覆盖主站的JavaScript，HTML，或者CSS。
另外，第三方内容不会在不适当的位置渲染。例如，传送的原始数据，第三方内容可以在主站页面内的任何地方渲染自己，覆盖主站的内容和功能，或在主站内容之下呈现和超出视图。
由于双方之间的代码干扰被包含在SafeFrame里，使用SafeFrame API，双方可以以可控和透明的方式进行交互。

 - **性能测量**
明确定义允许主站衡量第三方内容需要多长时间来加载，这对渲染到iframe的内容来说是可能的了。但是在第三方内容与网页代码内嵌投放的情况下，不能测量第三方内容的加载时间，因为它不被包含和可以在网页内多个地方呈现。随着SafeFrame的API让其交互能力成为可能，一个可行的办法是，一旦被内嵌投放到一个SafeFrame，就移动第三方内容。

 - **两全其美：富媒体和数据收集**
主站所有者面临着是否允许富媒体在他们的主站上内嵌到网页代码，允许无限制地访问页面数据，或者是否要在iframe中展现内容，这些做法会阻止大多数的富媒体互动和限制大部分或所有的数据收集。
展现内容到SafeFrame让富媒体交互和受控制的数据采集两全其美。规范实现了iframe投放的内容的新功能，并提供一个可行的方案，即投放富媒体到一个交互式的SafeFrame而不是内嵌到网页。

#### 1.2.4 主站自定义&控制
主站可以定制SafeFrame API，那些允许的功能和丰富的交互。主站也可以呈现新的内容，或在任何时候卸载内容。

### 1.3 SafeFrames和广告可见展视
SafeFrame创建一个包围广告内容的容器，防止直接访问有关主站网页或应用程序的信息内容。但是，该内容可以请求信息和通过使用SafeFrame API发送和接收消息与主站交互。这些消息使主站能够与第三方内容共享精选的信息，其中包括，让第三方内容能够确定其是否在视图中的几何信息。

下图说明了SafeFrame的内容如何显示，并表明了SafeFrame内投放的广告内容有25％在视图中。
![图1-1](http://119.29.142.213/static/201608/sf1.png)

使用SafeFrame的第三方接口`$sf.ext.geom`，提供广告内容的第三方可以确定SafeFrame相对于可视区域和总主站内容区域的位置。使用提供的尺寸，第三方可以确定有多少内容在视图中。

其他的SafeFrame调用让第三方内容可以扩展，超越SafeFrame的界限。`$sf.ext.geom`调用也使第三方可以确定它能扩展多远以及扩展内容中有多少是在视图中。

下图的例子说明了SafeFrame的内容怎么在网页内扩展和有多少是在可视区域内的。
![图1-2](http://119.29.142.213/static/201608/sf2.png)
想了解更多信息，请查阅第5.4节，它涵盖了详细的$sf.ext.geom调用。

#### 1.3.1 支持3MS广告可见度
本SafeFrames规范用于与其它行业举措周围的广告能见度对齐和提高广告度量的质量，即使测量有意义（3MS，Making Measurements Make Sense）的倡议。自从SafeFrame1.0发布，广告能见度仍在测试，而且正式的推荐标准尚未建立。正如同行业中建立和采用SafeFrames和3MS能见度推荐标准，更新的SafeFrame可以更无缝地支持3MS的推荐标准。

在这之前，SafeFrame1.0提供几何值，让主站和第三方可以用于确定有多少内容在视图中。主站与外部各方应合作，并且为了声明一个"可见度"展视，就应该有多少内容在视图中达成协议。

#### 1.3.2 可见度特性对第三方可选
SafeFrames框架允许第三方内容通过调用`$sf.ext.inVewPercentage`方法确定它是否在视图中。此特性，与所有的SafeFrame特性，对投放内容到主站的第三方是可选的。但是，无论何时第三方请求主站提供能见度数据，它都要提供。

### 1.4 SafeFrames和视频插播广告
SafeFrame的不是为了插播视频广告而设计的，但在一个VAST标签投放的插播横幅广告可以投放到SafeFrames。IAB的视频套件将需要更新以完全支持SafeFrame渲染，但视频发布商已经可以使用SafeFrame来代替他们的视频网页上使用的任何内部框架。

任何指定为一个iframe资源的VAST横幅广告可以渲染在SafeFrame内。为了确保VAST插播横幅广告可以投放到SafeFrame，第三方必须指定创造性的资源作为一个iframe。任何包含在VAST插播横幅广告内容的SafeFrame协议可以被任何支持SafeFrame1.0的视频发布商使用。

一个SafeFrame内渲染VPAID（Video Player-Ad Interface Definition）内容的可能性存在，但是实施解决方案需要来自视频主站与第三方的修改。直到IAB VSuite被更新以更容易支持SafeFrame，使用SafeFrame渲染VPAID内容的技术操作必须由愿意执行解决方案的各方加以解决。

### 1.5 SafeFrames和移动端
在网页呈现的任何的SafeFrame内容也可以像网页一样在移动设备呈现。基于浏览器的Web应用，包括那些专为移动端设计的，也能充分地从SafeFrame实施中获益。然而，虽然SafeFrame可以在非浏览器应用程序中运作，如那些专门为移动设备（本地应用程序）开发的，但是SafeFrame1.0没有非浏览器支持的详细信息，这些信息将被考虑进未来的SafeFrame发布中。

### 1.6 报告SafeFrame的数据
SafeFrame1.0为您提供可以在报表中使用的数据，但是使用SafeFrame报告数据的机制还没完成。报告系统必须配置为收集和以收件人期望的格式报告SafeFrame数据。

### 1.7 区别于其他规范
IAB的SafeFrame解决其他IAB规范或指南没有解决的问题。下面的章节帮助从行业里其他解决不同的问题的规范中区分SafeFrame。

#### 1.7.1 IAB友好的iframe
IAB的SafeFrame规范跟IAB那些记录了"在异步广告环境中富媒体广告的最佳示例"的友好的iframe不同。

IAB的友好iframe的最佳示例是用来支持使用JavaScript调用的富媒体广告，不像AJAX那样使用动态编码框架。使用易用的iframe，来自广告服务器的内容像主站内容一样渲染到一个来源于同一服务器的frame。

虽然友好的iframe解决方案解决了跨平台的障碍，以支持某些富媒体格式，但是它没有把第三方内容从主站内容中隔离。在易用的iframe里的富媒体内容像主站的一样，直接由同一个服务器提供。

相反地，SafeFrame实现了主站和第三方内容之间的隔离，并且提供了一个API来让控制和透明相互作用成为可能，同时为主站提供安全性和稳定性控制的最小层。使用SafeFrame时，广告内容是由一个中性域提供，而不是与主站内容一样来自于相同的来源。只允许通过SafeFrame指南中指定的API访问该广告内容被最终渲染的主站域。

SafeFrame指南删除了许多与投放富媒体到来源于主站的iframe相关的安全风险。SafeFrame API也允许主机和广告内容之间更加透明，以及对丰富的互动有更多的控制和监控工具。该新增的控制也保证了富媒体的更好的渲染能力。

#### 1.7.2 跨源资源共享（CORS）
跨源资源共享（CORS，Cross Origin Resource Sharing）是一种用于实现跨域的HTTP请求的机制。如果没有CORS，同源策略（the Same Origin Policy）就会阻止一个域的嵌入代码请求来自另一个域的潜在的破坏性的内容。对于良性的内容如图像存在例外。

不幸的是，同源策略阻止内容，比如富媒体广告，请求脚本文件以支持正确的渲染和互动。利用CORS，内容可以让浏览器对信任域进行跨域请求。虽然这个解决方案是有效的，但是它也赋予第三方内容对主站的完全访问权限，使第三方内容有权不需主站知晓就访问和更改主站内容。

CORS只能管理一个域名是否可以请求另一个;它不能管理这两个之间的相互作用。

另一种解决方案是把内容投放到iframe中。在此解决方案中，内容是在提供内容的外部服务器中进行处理。但有了这个解决方案，所有对主站的访问都被拒绝，禁止与第三方内容的任何丰富的互动。

综上所述，CORS不能控制被请求的内容可以访问什么，而一个标准的iframe又禁止任何的访问。

同时使用iframe和API，SafeFrame实现了主站和外部提供的内容之间丰富的互动，同时允许控制对主站信息的访问。

### 1.8 超出范围的项(Out of Scope)
在本规范中，下列项是超出范围的：

 - **读取/检索第三方内容**
SafeFrame不指定如何检索第三方内容。一旦检索，第三方内容就作为一个字符串或URI和主站选择使用的任何元数据，被放入SafeFrame`$sf.host.Position`对象中。

 - **为了特定实现的附加功能**
对比起其他的创建，操纵和管理一个容器，SafeFrame没有定义任何对象，方法，或者其他属性。进一步的功能，例如额外的安全性，UI元素等可能会被加入到未来的版本中，但不会作为SafeFrame1.0的一部分。

 - **基于非浏览器的实现**
SafeFrame的这个版本仅限于JavaScript的格式，基于浏览器的实现。SafeFrame1.0可以在应用内发挥作用，如那些在移动设备（本机），但是对于非基于浏览器实现的细则未包括在SafeFrames的这个版本中。它支持基于浏览器的应用程序，移动设备和任何其他设备。

### 1.9 操作注意事项

使用SafeFrames可能会改变某些熟悉操作。在实施SafeFrames之前，请考虑以下的一些操作效果，并确定你的技术和过程是否需要做出任何修改，以利用SafeFrame的优势。

以下的考虑情况旨在提供一个用于实现分析的起点。可能需要进一步的评估和测试，以确保顺利实施和过渡。

 - **上下文展示广告**
以编程方式投放基于网页内容的广告的广告代码，访问网页数据，以识别它显示的内容类型，然后投放适合于内容的广告。使用SafeFrames，因为广告代码不能直接访问主机页面，主站API必须传递这个数据给广告代码。
该广告代码提供者应与主站页面所有者（发布者）洽谈，为了广告代码提供者投放上下文展示广告，应该传递哪些信息。
欲了解更多信息，请参阅第5.8节有关`$sf.ext.meta`功能的详细信息。

 - **访问主站URL**
当第三方内容使用iframe最初加载到一个主站站点时，HTTP头一般标识URL为主站站点，但不是很准确。由于SafeFrame是一个iframe，这种URL不准确的情况也发生在SafeFrame。为了获取准确的主机URL，在JavaScript中使用document.referrer属性，正如你在一个标准的iframe中会做的一样。

 - **设置Cookies**
使用SafeFrame，第三方内容在SafeFrame域内被渲染，这与主站域不同。Cookies可以在这个二级域名中被设置和读取，但主站必须声明是否支持`$sf.ext.cookie`功能。此外，即使支持cookie的读写能力，主站控制何时及向谁共享Cookie数据。如果第三方内容需要设置和直接在主站域读取cookie，这个访问必须与主站页面所有者进行谈判协商。

 - **第三方数据**
使用SafeFrames的API，从主站（host）域共享的任何数据是由主站提供的。在商业模式，一个第三方代表另一方从主站收集数据，在一个SafeFrame实施的第三方必须依赖由主站（第一方）提供的数据，而不是直接收集。
当第一方提供的数据可能会提高人们对数据完整性的关注，被共享的数据被用于正确呈现提供的内容。共享不正确的数据违背了主机的最高利益，因为这可能会导致不正确的渲染和互动，这与会干扰主站的页面内容。此外，实施SafeFrames的主站所有者可以被审核，以确保整个的SafeFrame API共享数据的完整性。

 - **支持未知尺寸的广告内容**
有些广告投放模式涉及，在不知道哪个广告会被投放和什么时候调用的详细信息的情况下，对于特定的一套标准的广告分配广告空间。在这些情况下，在调用该广告的时候，宽度和高度是是未知的。SafeFrame1.0不直接支持这种模式，但可以使用现有的SafeFrame功能与对“推送”扩展技术的支持相结合来支持。
如果主机声明对推送扩展技术的支持，通过使用`$sf.ext.expand`功能以扩展SafeFrame容器来提供可以调整为更大实际尺寸的原始尺寸，未知的尺寸的广告内容可以被接受。当在这种情况下扩张，但使用覆盖扩展方法是一种选择的时候，推送方法，如果支持的话，是最佳的方法。

其他的技术和工艺，可能需要修改，以适应SafeFrame的操作。SafeFrame上线之前，请考虑运行一个深入的分析和进行产品测试。

## 2. 主站实施(Host Implementations)
在一个基于SafeFrame实施的浏览器，API的主站端用JavaScript编写，并且必须提供所定义的在章到中列出的函数和命名空间列表。基于实现的浏览器的机制是使用一个iframe标签来创建第三方内容的容器，伴随着额外的JavaScript代码，以促进功能和与第三方内容的通信。

浏览器根据他们所支持的功能等级分级。A级浏览器是已知的，有能力的，现代的，和常见的。所有使用JavaScript激活A级浏览器应予支持。C-＆X级浏览器是较为少见，能力较差，和过时的。主站方可能在斟酌下决定支持这些浏览器。SafeFrame的主要依靠HTML5“postMessage”函数作为低级别的iframe和主站之间的通信手段。而postMessage函数提供了最佳的性能，其它机制可以用于促进两方之间的通信，特别是在C-＆X分级浏览器的情况中。

有关HTML5 PostMessage函数的更多信息，请访问：
[http://dev.w3.org/html5/postmsg/#posting-messages](http://dev.w3.org/html5/postmsg/#posting-messages)

### 2.1 SafeFrames是如何运作的
SafeFrame的目标是提供将内容从外部源（第三方内容）传递到一个SafeFrame容器和渲染到主站站点上。第三方内容可以通过以下两种方式之一传递：

**传递模式A：主站代码转换第三方内容，并渲染在SafeFrame中**
当浏览器联系主站网络服务器，主站可以从自己的后台系统检索第三方内容。在这种传递方式，主站可以使用由SafeFrame实例渲染的SafeFrame标签，转换第三方内容到内联的JavaScript结构。

**传递模式B：第三方内容被直接传递到SafeFrame**
主站可能没有机制去在自己的网络服务器上转换第三方内容。在这种传送模式，它们仍然提供相同类型的内联JavaScript结构与SafeFrame标记，但是它们给内容提供一个URL，而不是直接放置第三方内容到该结构。在这种情况下，使用SCRIPT标签和指定的URL把第三方内容直接输送到SafeFrame容器中。

下面的章节详细描述了这两个过程。

#### 2.1.1 传递模式A：主站转换第三方内容
下图说明了使用传递模式A传递第三方内容到SafeFrame容器的过程

![图2-1](http://119.29.142.213/static/201608/sf3.png)

 1. **内容请求：**当最终用户访问主站时，浏览器向主站服务器请求内容。
 2. **第三方内容请求：**主站向外部服务器请求内容数据。
 3. **第三方传递：**第三方将HTML内容作为数据传递
 4. **SafeFrame标签：**主站主站转换第三方内容数据以使用SafeFrame标签将其投放到SafeFrame容器
 5. **内容隔离：**主站内容与第三方内容隔离
 6. **内容传递：**主站内容，伴随着包裹在一个SafeFrame容器的第三方内容，被投放到浏览器。
 7. **浏览器处理SafeFrame：**浏览器使用来自被传递的主站内容的SafeFrame指令，来处理主站内容和第三方内容之间的互动。

#### 2.1.2 传递模式B：第三方内容直接传递
下图展示了使用传递模式B传递第三方内容到SafeFrame容器的过程

![图2-2](http://119.29.142.213/static/201608/sf4.png)

 1. **内容请求：**当最终用户访问主站时，浏览器向主站服务器请求内容。
 2. **主站内容传递：**主站传递它的站点的HTML内容，伴随着SafeFrame的指令和指向第三方内容的URL。
 3. **浏览器处理SafeFrame：**浏览器使用来自被传递的主站内容的SafeFrame指令，来处理主站内容和第三方内容之间的互动。
 4. **外部请求：**浏览器使用主站提供的URL向外部服务器请求内容。
 5. **第三方内容请求：**被请求的第三方内容直接传递到SafeFrame iframe中。

 
#### 2.1.3 渲染SafeFrame
 在部分2.1.1和2.1.2的图片分别说明传递模式A和传递模式B之间的差异。下面的图描述在一个高层次，浏览器如何使用主站服务器发送来的SafeFrame指令初始化SafeFrame API和在其中渲染第三方内容。

![图2-3](http://119.29.142.213/static/201608/sf5.png)

 1. **获取SafeFrame：**浏览器从主站服务器接收指令后，浏览器从一个二级域名请求和接收SafeFrame。
 2. **配置SafeFrame：**浏览器初始化从host库访问的SafeFrame代码。使用SafeFrame类`$sf.host.Position`，确定传递模式，因为要么包括HTML第三方内容（传递模式A）要么引用一个URL（传递模式B）。SafeFrame函数`$sf.host.render()`随后用于渲染iframe。
 3. **创建IFRAME：**：iframe随后在二级域名被创建。如果第三方内容是使用传递模式A传递的，内容数据将与iframe被载入。
 4. **加载iframe：**iframe被加载（如果使用传递模式B传递，则伴随着第三方内容）到host库。
 5. **API初始化：**SafeFrame API被初始化，而且如果使用传递模式A传递，则第三方内容被渲染。
 6. **第三方内容请求（模式B）：**如果第三方内容使用传递模式B传递，则SafeFrame内提供的URL被用于向外部服务器请求内容。
 7. **第三方内容渲染（模式B）：**在SafeFrame iframe中第三方内容被传递和渲染。

#### 2.1.4通过API进行页内通讯
 下图说明了主站与第三方内容之间的通信是怎么启动的。

 ![图2-4](http://119.29.142.213/static/201608/sf6.png)

 1. **接收第三方内容（作为数据）：**浏览器接收第三方数据，作为渲染在主站页面的内容。
 2. **API初始化：**使用SafeFrame的类`$sf.host.Position`，网页创建一个接收的外部数据的容器，然后配置的SafeFrame主站API。
 3. **第三方内容渲染：**：第三方数据在SafeFrame中渲染为内容。
 4. **通信：**通讯：一旦第三方内容被渲染，如果付诸实施，它可以使用SafeFrame外部API代码，以调用主站API。

### 2.2要求
 下面的章节描述实施SafeFrame的要求。

#### 2.2.1 JavaScript host库和API
 JavaScript的host库和API是用来控制和渲染SafeFrame容器的。该库提供了命名空间，类和函数，这些将在第4章中进一步描述。

> **主站实现注意事项** 
> 一个SafeFrame可能包括：一个网页视图（移动端），一个嵌入式浏览器，一个HTA（微软HTML应用程序），或原用户的Web浏览器。
>

#### 2.2.2 二级域名

通过创建一个比起主站页面具有不同的来源，主机名和域名的iframe，SafeFrame 主站 API创建，渲染并管理第三方HTML内容。这种二级域名创建了一个"跨域屏障"，防止第三方的HTML和JavaScript直接访问主站页面的任何东西。

Web浏览器遵循"同源"政策，即来自2个不同来源的代码不允许进行通信（有一些例外）。因此，主站*必须*有一个二级域名，从那里可以投放SafeFrame资源。使用CDN（内容分发网络）可以提供二级域名。

#### 2.2.3 资源公约

对于主站，SafeFrame定义两种类型的资源：基本HTML文件和JavaScript文件。

**基本HTML文件（静态HTML文件）**

HTML文件是用来提供一个第三方HTML内容渲染成的基础级的HTML文档。在不支持HTML5报信的情况下，HTML文件还可以被用来充当代理，以促进在主站和第三方之间发送消息。

与SafeFrame一起使用的HTML文件必须遵循以下约定：

 - 浏览器和代理也必须能够公开缓存HTML文件
 - 一个以上的HTML文件可以被用于渲染，以提供额外的功能。
 - 所有的渲染HTML文件必须包括对外方API功能的支持，以渲染第三方内容。
 - 用于渲染的HTML文件必须首先包含SafeFrame第三方JavaScript库。

**JavaScript文件**
主站的SafeFrame API用JavaScript实现，而且必须遵守以下公约：

 - SafeFrame API JavaScript文件必须是静态的。
 - 第三方API总是用JavaScript实现。
 - 也必须允许浏览器和代理公开缓存所有提供的文件。

#### 2.2.4 SafeFrame的URI公约

因为浏览器和代理必须能够缓存JavaScript和HTML资源文件，这些文件应该只因新版本的发行而改变，而且用于访问文件的URI不能包含查询字符串参数或任何会让浏览器或代理不缓存文件的东西。

版本一致性必须得到维护。例如，对于版本2-2-0的HTML资源不应该被提供给一个版本2-3-5的JavaScript实例。版本一致性必须得到维护。

以下SafeFrame的访问主站资源的的URI格式，实现了静态URI的使用和资源范围内的相对路径。

![图2-5](http://119.29.142.213/static/201608/sf7.png)

用于访问SafeFrame资源的URI的部分完全是由主站定义的，但必须按指定的顺序提供并按下面的细节描述：

 1. 协议 (例如http, https等等) 
 2. 二级域名（和端口，如果适用的话）
 3. 通向SafeFrame资源的根路径（允许多个目录，用/分隔）
 4. SafeFrame的版本号，格式为“N-N-N”
 5. 小写的“html”表示HTML文件或小写的“JS”表示JavaScript文件（其他资源类型可能包含在未来发布的SafeFrame）
 6. 文件名称

### 2.3 实现注意事项

下面的说明对于主站实现SafeFrame来说是重要的。

**SafeFrame和iframe嵌套**
SafeFrame容器总是在顶层HTML文档中呈现。一个SafeFrame的容器不能在另一个容器的SafeFrame内呈现。如果SafeFrame 主站 API创建的iframe是作为浏览器执行的主站顶层HTML的一部分呈现的话，它只能与外部HTML内容通信。SafeFrame的JavaScript代码必须能够检测不正确的嵌套。

鉴于以下不当嵌套的情况，SafeFrame的JavaScript代码应该采取下面对应的行动。

 1. SafeFrame主站的JavaScript代码加载到一个iframe：
a. SafeFrame主站的JavaScript的命名空间是无效的。
b. 一个且只有一个调用函数`$sf.host.boot`是允许的，这样类`$sf.host.Position`对象的内容就可以呈现了。
c. SafeFrame的JavaScript代码将不会遵守且不会处理配置调用（请参阅类`$sf.host.Config`和类`$sf.host.PosConfig`）。
d. SafeFrame的JavaScript代码将不会遵守且不会处理一个iframe中的SafeFrame标签中定义的元数据。
 2. SafeFrame标签（见第3章）放置在iframe里面：
a. SafeFrame的JavaScript代码将解析和读取SafeFrame标签以检索内容。
b. SafeFrame的JavaScript代码将渲染内容
c. SafeFrame的JavaScript代码将不会遵守且不会处理配置调用（请参阅类`$sf.host.Config`和类`$sf.host.PosConfig`）。
d. SafeFrame的JavaScript代码将不会遵守且不会处理一个iframe中的SafeFrame标签中定义的元数据。
e. 如果提供的iframe是一个SafeFrame容器，那么内容伴随着自己的JavaScript代码被渲染，并可以访问第三方的SafeFrame API。

**SafeFrame JavaScript库**
SafeFrame容器总是包含一个JavaScript库，它响应SafeFrame主站JavaScript库，并且总是包含在HTML渲染文件的第一个JavaScript文件。

**SafeFrame HTML管理**
SafeFrame管理所有它创建的HTML元素。无论是主站还是第三方都不应该试图操纵该SafeFrame创建和管理的HTML节点。这样做会意料不到地损坏接口。

SafeFrame可能会为它控制的HTML元素增加一个CSS类，但主站和第三方应避免为以下的CSS类名添加CSS规则或选择器：

 - **sf_data: **用于表示包含要渲染的数据的内嵌SafeFram元素
 - **sf_position: **用于表示一个被渲染的SafeFrame容器的起始HTML元素树
 - **sf_lib: **用于表示包含SafeFrame JavaScript代码的SCRIPT HTML元素。
 - **sf_el: **一般用来表示由SafeFrame的JavaScript代码保持的其他HTML元素

**递交内容到的SafeFrame主站API以渲染**
除了配置递送到SafeFrame容器的第三方内容，主站应确保相关内容以一个可以接受的形式给API去处理。主站页面接收第三方内容作为原始的HTML或一个SafeFrame必须获取和渲染的URL。

在这两种情况下，SafeFrame都使用JavaScript来处理信息。原始HTML内容可能需要在发送到SafeFrame之前进行编码。虽然不需要调整URL，第三方应该知道，渲染和添加内容到SafeFrame容器必定需要一个JavaScript响应(response)。

下面的实现注意事项提供更多的细节，且与第2.1节讨论的两个传递模式相对应。
 - **作为JavaScript字符串的原始的HTML（传递模式A）**
 由于JavaScript处理某些字符和SCRIPT标签时不同于HTML，主站可能必须在发送给SafeFrame API处理之前，修改任何原始的HTML内容。
 例如，下面的HTML字符串被当做JavaScript处理时，会报一个语法错误：
 
 ``` html
<script type="text/javascript">
 	document.write('Hello "Dave"');
</script> 
 ```
 为了使上述HTML字符串在SafeFrame API中正常被处理，必须重写格式如下：
 
 ``` js
 var html = "<scr"+"ipt type=\'text\/javascript\'>";
 document.write('Hello \"Dave\"'); </scr"+"ipt>";
 ```
 以上的JavaScript格式的HTML字符串可以在SafeFrame`$sf.host.Position`对象中正常运行。
 - **使用URL来获取第三方内容（传递模式B）**
 主机可以提供一个URL到第三方内容，而不是提供所述HTML内容本身。在此情况下，主站页面不能直接把第三方内容渲染到的SafeFrame`$sf.host.Position`对象，给SafeFrame去获取和渲染在容器中的内容。
 由于网页不能编译SafeFrame中处理的第三方内容，SafeFrame请求得到的响应应该是JavaScript格式的。一旦生成SafeFrame容器，第三方内容的URL响应的SCRIPT标签产生，并且可以传递附加内容。使用一个SCRIPT标签，所以从被提供的URL传递的内容可以在SafeFrame的容器内访问外部的API，而不是使用一个嵌套的iframe标签。

 > **第三方实现注意事项**
 > 虽然第三方HTML内容可能使用其他的iframe标签，因为"同源"政策，在额外的iframe标记内的任何内容被禁止访问API。欲知更多信息，请参阅第3章的SafeFrame标签
 >

### 2.4 SafeFrame的渲染细节
主站JavaScript库插入一个HTML iframe元素到主站页面。主站API控制渲染过程，为了也控制第三方内容在iframe中渲染。

使用以下的指南创建iframe：

**`src`属性**
iframe的"src"属性是指向一个静态的，公开缓存的HTML文件的URL。所提供的URL必须来自一个不同于作为典型的内容分发网络（CDN）的主站域。

**`name`属性**
iframe的"name"属性必须包含数据的序列化字符串，包含这些的配置属性：一个特定的SafeFrame位置的配置，元数据和渲染的内容。

在name属性中使用数据字符串使单向的，同步的消息传递，使得HTML文件中的JavaScript代码可以：读取相应的`window.name`属性，反序列化数据的字符串，设置第三方内容的环境，最后渲染内容。

这种技术允许包含JavaScript的`document.write`命令的HTML `SCRIPT`标签能够正常工作。

**`Width`和`Height`属性**
iframe的宽度和高度需要被设置为与`$sf.host.PosConfig`对象的w和h字段相同的值。通常，iframe的宽度和高度应与渲染的内容的已知宽度和高度匹配。

**`SCRIPT`标签**
一个`SCRIPT`标签应在创建的iframe中的HTML文档中存在。这个`SCRIPT`标签是第一个，最初的在HTML文档中以数据形式读取以进行处理并渲染的JavaScript。一个访问JavaScirpt文件的相关的URL可以备用，只要版本保持一致性（欲知更多关于URL在SafeFrame中的公约，请查阅第2.2.4节）。

此JavaScript文件必须（按顺序）执行以下操作：

 1. 请检查HTML文档是否直接在顶级HTML文档的（是它的一个孩子）。如果不是，应报一个错误，并且不渲染任何内容。
 2. 读取和反序列化在`window.name`属性中传递的数据。
 3. 验证传递到`window.name`属性的数据，它通常是通过，确保反序列化对象包含所有必需的信息，包括GUID，来完成验证。如果验证失败，应产出一个错误，并且不渲染任何内容。
 4. 设置`window.name`属性为空字符串（""），使第三方不能在这上读取数据。
 5. 初始化发送跨域邮件到主站的SafeFrame的JavaScript的能力。
 6. 附加任何额外的标记和`name`属性中传递的元数据。
 7. 附加任何专有的逻辑，事件处理程序或其他细节到HTML文档（如`onload`事件）。
 8. 渲染第三方内容。

HTML文件应该包含以下内容：

 - CSS规则：设置文档中的`BODY`元素的margin和padding为0px。
 - 一个单一的，绝对定位的`DIV`元素，作为`BODY`元素的直系子结点，最初位于top 0，left 0。这个元素在第三方内容希望在给定方向扩展的情况下被使用，以便能够保持正确的对齐。
 - 一个单一的，`SCRIPT`元素，放置在给定的`DIV`元素，其中包含上面第4节的逻辑。该`SCRIPT`可能来自第三方，或者可能定义成内联。
 例子：
 ``` html
 <html> 
    <head> 
    <style type="text/css"> 
      BODY { margin:0px; padding:0px } 
    </style> 
    </head> 
    <body scroll="no"> 
        <div id="sf_align" style="position:absolute;top:0px;left:0px;" class="sf_el"> 
       		<script type="text/javascript" src="../js/ext.js" class="sf_lib"></script> 
        </div> 
    </body> 
 </html> 
 ```

### 2.5 通信机制的细节

为了促进一个SafeFrame容器内的主站和第三方之间高性能，安全的通信，HTML5的"postMessage"函数是一个主要的方法。此方法允许实施者，从一个HTML文档发送一个字符串到另一个来源于另一个地方的HTML文档，从而绕过"同源"政策。第三方内容被阻止调用此相同的方法去意图欺骗主站API因为消息（发送的字符串）被验证成功而做些错误的或恶意的事情。

想知道有关W3C同源策略的更多信息，请访问：
[http://en.wikipedia.org/wiki/Same_origin_policy](http://en.wikipedia.org/wiki/Same_origin_policy)

每当从容器收到信息，采取以下的步骤来确保它是被允许的：
 1. **二级域名/检查来源**
 从外部HTML内容发来的一个消息的原始域，应该与用于创建SafeFrame容器的`$sf.host.Config`类的`renderFile`字段中传递的URL的域相吻合。如果起源不吻合，消息就会被忽略。
 2. 检查GUID 
 一个GUID被定义为，SafeFrame容器被渲染和应该与外部API传递的任何信息包含在一起的情况。当提供的GUID不存在或者未知时，则忽略该消息。
 3. 检查 HTML window对象的引用
 第三方对象的window引用源应该指向一个当SafeFrame被渲染时创建的iframe窗口引用。如果对象的窗口参考不匹配，已经呈现的任何已知的SafeFrame容器，则忽略该消息。
 4. 处理序列消息
 消息在先来的、先被发出的基础上被处理，而且应该总是尝试表示成功或失败的响应。

## 3. SafeFrame 标签
SafeFrame标签的高级别目标是封装第三方内容数据，以便主站可以管理内容的渲染和控制。以下各节描述SafeFrame标签，如何构建它们，以及如何处理它们。

### 3.1 SafeFrame 标签结构&要求
一个SafeFrame的标签是标准化的一组HTML标签。它必须由下列元素构成：
 - 与主机配置内联提供的，包含第三方内容的元数据的`SCRIPT`标签。
 - 在`SCRIPT`标签中处理数据标签的JavaScript
 - (可选)当不支持JavaScript的时候，HTML上的后退的NOSCRIPT标签。
 - (可选)指定SafeFrame容器将渲染在哪的DIV标签

只有主站应该在网页内容中插入SafeFrame标签。嵌套的SafeFrame标签不被支持。忽略任何包含在来自交换，中介，代理或任何其它二级出版发布伙伴或供应商的标签中的SafeFrame标签。

如果一个SafeFrame标签渲染在一个已经创建的SafeFrame容器内，渲染过程会假定容器已经创建并跳过以直接渲染第三方内容。直接在主站网页上只使用SafeFrame容器确保内容被正确地渲染，数据是共享的，并且该API可以被访问。

### 3.1.1 `SCRIPT`标签
`SCRIPT`标签必须专门构建成与主站内容内嵌，并包括渲染在SafeFrame中的第三方内容的数据标签。

`SCRIPT`标签必须包括以下内容：

 - 一个带有值为`iab_sf_data`的属性的类型
 - type属性设置为text/ X-的SafeFrame
 - 一个JavaScript或JSON样的数据结构，是`$sf.host.Position`和`$sf.host.PosConfig`的代表。此结构在字面JavaScript语法中定义，并且可能包括被转换成一个`$sf.host.PosMeta`对象的附加元数据值。

**例子：**
``` html
<script type='text/x-safeframe' class='iab_sf_data'>     
	{ 
		id: "LREC", // ID of position object        
		html: "<h1>Hello World</h1>", //3rd party HTML content 
		conf:  
		{ 
			size: "300x250" //The size conf is required and denotes the  
		} 
	 	meta: //optional shared meta information  
		{ 
			rmx:  
			 { 
			 	sectionID: "14800347",  
			 	siteID: "140509"   
			 }  
		}  
	}  
</script>  
```

 上述的`SCRIPT`标签被定义为这样的原因如下：
  - **type="text/x-safeframe" **
  由于该数据是一个内嵌的JavaScript结构，可能会发生在语法方面的问题，并且如果被当作JavaScript代码可能会损坏主站网页。设置脚本类型为“text/ X-SafeFrame”确保数据结构不被浏览器的JavaScript引擎拾起。
  - **class="sf_data"**
  在一个`SCRIPT`标签中，类属性不被处理;然而，指定sf_data类名使SafeFrame 主站API能标识那些它可以待会处理的数据的标签。

### 3.1.2 使用JavaScript处理数据标签
`SCRIPT`标签必须包含将处理（一个或多个）数据标签的JavaScript代码。第3.1.2.1节至第3.1.2.3节提供了以三种不同的方式来处理数据标签的例子。

 - 例子3.1.2.1：一次性处理所有标签
 - 例子3.1.2.2：先定义库
 - 例子3.1.2.3：一次一个地处理标签

#### 3.1.2.1 例子：一次性处理所有标签
当数据标签在伴随`$sf.host.boot`方法的SafeFrame host库之前，都包含在`SCRIPT`标签内，host库被加载，并且引导程序找到并渲染代码中引导程序之上列出的数据标签。

下面的例子提供了两个，加载SafeFrame host库和引导程序的SafeFrame的数据标签。
``` html
<table>  
	<tbody>  
		<tr>  
			<td valign='top'> 
			<!-- SafeFrame Inline Tag 1 -->  
			<div id='tgtLREC'>  
				<script type='text/x-safeframe' class='iab_sf_data'> 
				{  
					id: "LREC",  
					src:  
					"http://extserver.com/data-tag",  
					conf:  
					{  
						w: 300,  
						h: 250,  
						dest: "tgtLREC"  
					},  
					meta:  
					{  
						rmx:  
						{  
							sectionID: "14800347",  
							siteID:  "140509"   
						}  
					}  
				}  
				</script>  
				<!-- b/c a "dest" tag exists (the overall div container) -->
				<!-- container tags will be rendered here -->  
				<!-- optional noscript section for fall back -->  
				<noscript>  
					<img src=  "http://ext.server.com/img.gif"  />  
				</noscript>  
			</div> 
			</td>  
			<td valign='top'>  
			<!-- SafeFrame Inline Tag 2 -->  
				<script type='text/x-safeframe' 
				class='iab_sf_data'>  
				{  
					id: "LREC2",  
					src: "http://externalserver.com/data-
					tag",  
					conf:  
					{  
						w: 300,  
						h: 250  
					}  
				}  
				</script>  
			<!-- b/c a "dest" tag exists (the overall div container) -->
			<!-- container tags will be rendered here -->   
			<!-- optional noscript section for fall back -->  
				<noscript>  
				<img src= "http://ext.server.com/img.gif" />  
				</noscript>  
			</td>  
		</tr>  
	</tbody>  
</table>
```
``` html
<!-- SafeFrame Host library / API -->  
<script type='text/javascript' src='sf-api-boot.js'></script>  
<!-- script code in external file will automatically 'boot' and read data tags -->
```

#### 3.1.2.2 在数据标签之前先定义SafeFrame host库

实施一个SafeFrame标签的另一种方法是，先定义host库，然后提供数据标签，最后显式调用`$sf.host.boot`。
下面的例子演示了这种情况可能是如何编码的。

``` html
<html> 
<head> 
	<script type="text/javascript" src="http://cdn.example.org/v1/sf-host.js"></script>
	<script type='text/javascript'>  
	 
	<!-- SafeFrame Host API configuration -->  
	(function()  
		{  
		var pubAPI = $sf.hostpub, conf;   
		function handle_start_pos_render(id)  
		{   
		 
		}  
		function handle_end_pos_render(id)  
		{ 
		}  
		conf = new pubAPI.Config( 
		{  
			auto: true,  
			cdn: "http://l.yimg.com",  
			renderFile: "r.html",  
			root: "/SafeFrame/v1/html",  
			ver: "2-3-4",  
			positions:  
			{  
			"LREC":  
				{  
				dest:  "tgtLREC",  
				w: 300,  
				h: 250  
				}  
			},  
		onStartPosRender: 
		handle_start_pos_render,  
		onEndPosRender: handle_end_pos_render  
		});   
	})();  
	</script>  
</head>  
<body>  
	<div id='tgtLREC'>  
	<script type='text/x-safeframe' class='sf_data'>  
	{  
	id: "LREC",  
	src: 
	"http://externalserver.com/data-tag",  
	meta:  
		{  
		rmx:  
			{  
			sectionID: "14800347",  
			siteID: "140509"   
			}  
		}  
	}  
	</script>
	 <!-- b/c a "dest" tag exists (the overall div 
	container) container tags will be rendered here -->  
	<noscript>  
		<img src= "http://ext.server.com/img.gif"  
		/>  
	</noscript>  
	</div>  
	<script type='text/javascript'>  
	$sf.host.boot();  
	</script>  
</body>  
</html>  
```

#### 3.1.2.3 有兄弟标签的SafeFrame数据标签自动引导(SafeFrame Data Tag with Sibling Auto-Bootstrapping)

在下面的例子中，每个提交的数据变量伴随着一个调用`$sf.host.boot`以在代码中加载列出的标签的二级标签。
``` html
<table>  
<tbody>  
<tr>  
<td valign='top'>  
<!-- SafeFrame Inline Tag 1 -->  
<div id='tgtLREC'>  
	<script type='text/x-safeframe' 
	class='sf_data'>  
	{  
	id: "LREC",  
	src: 
	"http://externalserver.com/da
	ta-tag",  
	conf:  
		{  
		w: 300,  
		h: 250,  
		dest: "tgtLREC"  
		},  
	meta:  
		{  
		rmx:  
		{  
			sectionID: 
			"14800347",  
			siteID: "140509"   
		}  
		}  
	}  
	</script>  
	<!-- b/c a "dest" tag exists (the overall 
	div container) container tags will be 
	rendered here -->  
	<!-- optional noscript section for fall 
	back -->  
	<noscript>  
		<img src= "http://ext.server.com/img.gif" />   
	</noscript>  
	<script type='text/javascript'>  
	(function() {  
		var w = window, s = w["$sf"], 
		b = s && s.boot;  
		if (!s) s = w["$sf"] = {};  
		if (b && typeof b == 
		"function") {  
		try { b(); } catch (e) 
		{ }  
		} else {  
		document.write("<scr","ipt type='text/javascript' src='sf-host.js'></scr","ipt>")
		;  
		}  
	})();  
	</script>  
	<!-- Above script code will only load in 
	host library one time, call boot for each 
	tag -->  
</div>  
</td>  
<td valign='top'>  
	<!-- SafeFrame Inline Tag 2 -->  
	<script type='text/x-safeframe' 
	class='sf_data'>  
	{  
	id: "LREC2",  
	src: 
	"http://externalserver.com/data-tag",  
	conf:  
	{  
		w: 300,  
		h: 250  
	}  
	}  
	</script>  
	<!-- b/c a "dest" tag exists (the overall div 
	container) container tags will be rendered here 
	-->  
	<!-- optional noscript section for fall back -->  
	<noscript>  
	<img src= "http://ext.server.com/img.gif" />  
	</noscript>  
	<script type='text/javascript'>  
	(function() {  
		var w = window, s = w["$sf"], 
		b = s && s.boot;  
		if (!s) s = w["$sf"] = {};  
		if (b && typeof b == "function") {   
		try { b(); } catch (e) 
		{ }  
		} else {  
		document.write("<scr","ipt type='text/javascript' src='sf-host.js'></scr","ipt>");  
		}  
	})();  
	</script> 
	<!-- Above script code will only load in host library one time, call boot for each tag -->  
</td>  
</tr>  
</tbody>  
</table> 
```

## 4. 主站API实施细则
SafeFrame主站API使用第4.1节至第4.11节到定义的的命名空间，函数，和类。

### 4.1 命名空间`$sf.host`
此命名空间用来定义主站网页可以用于与SafeFrame容器交互的JavaScript类，对象和方法。

在`$sf.host`命名空间是SafeFrame配置，渲染，检查，并与SafeFrame容器互动的SafeFrame起始点。在这个空间中定义的一切都是公开，除非另有被点名的。

### 4.2 命名空间`$sf.host.conf`
指定`host.conf`命名空间内联将允许SafeFrame容器（包括SafeFrame标签）与你指定的配置选项被加载（或引导）。这个对象是`$sf.host.Config`对象的字面版本(literal version)。

**相关章节**
 - 3 SafeFrame标签
 - 4.4 类 `$sf.host.Config`

**例子1**
``` html
<script type='text/javascript'>    
 
//JavaScript inline host config, used mainly for SafeFrame tags which want to auto boot the SafeFrame host API and render 3rd party content.    
 
var w = window, sf = w["$sf"], pub = sf && sf.host;   
if (!sf) sf = w["$sf"] = {};  
if (!pub) pub = sf.host = {};   
 
host.conf  =  
{  
	debug:    true,  
	ver:    "2-3-4",  
	positions:  
{  
	LREC:  
	{  
		id:   "LREC",  
		dest:  "tgtLREC",  
		tgt:  "_self",  
		w:  300,  
		h:  250    
	}  
}  
};      
 
//Assuming a SafeFrame tag is placed below this configuration, it will read the config defined and use those values as the logic for the tag.  
</script>
```

**例子2**
``` html
<!-- SafeFrame Inline Tag -->   
<div id="tgtLREC">  
<script type='text/x-safeframe' class='sf_data'>  
{  
id:    "LREC",  
src:"http://ext.server.com/sf",   
conf:  
{  
	dest:  "tgtLREC",  
	size:  "300x250"  
},   
 
meta:  
{    
	rmx:  
	{  
		sectionID:  "14800347",  
		siteID:  "140509"   
	}  
}    
}    
</script>  
<script type='text/javascript'>  
try {  
	$sf.host.boot();  
} catch (e) {  }  
</script>  
</div> 
```

### 4.3 命名空间`$sf.info `
该信息命名空间被保留用于存储有关SafeFrame容器的信息。

 - `<static> {Array} $sf.info.errs `
 包含有关发生在SafeFrame API的主站端的任何错误信息;细节是**只读**的。
 - `<static> {Array} $sf.info.list `
 包含了关于每个SafeFrame的容器的信息，要么是要渲染的，要么是正在处理的;细节是**只读**的。

无论何时SafeFrame主站API创建一个容器，它会适当地更新这些命名空间字段，允许检查和/或调试当前状态。

**例子**
``` html
<div id='tgtLREC'></div>  
<script type='text/javascript'>   
 
(function() {   
 
	var w = window, sf = w["$sf"], pub = sf && sf.host, Config = pub 
	&& host.Config,    
	 
	CONF_CDN   = "http://l.yimg.com",  
	CONF_ROOT   = "/sf",  
	CONF_VER  = "2-3-4",  
	CONF_RFILE  = "/html/render.html",  
	CONF_TO  = 30;   
	 
	function on_endposrender(posID, success)  
	{  
	//a render action success  
	}   
	 
	function on_posmsg(posID, msg, data)  
	{  
	//listen for messages 
	}  
	 
	w.render_content  = function()  
	{  
	var conf, posConf, pos,confDesc;   
	 
	if (Config) {  
	conf = Config();  
	if (!conf) {  
	confDesc  =  
	{  
	debug:       true,  
	cdn:        CONF_CDN,  
	root:       CONF_ROOT,    
	ver:        CONF_VER,  
	renderFile:      CONF_RFILE,   
	to:        CONF_TO  
	onEndPosRender:    on_endposrender,  
	onPosMsg:      on_posmsg  
	};  
	conf = new Config(confDesc);  
	}  
	if (conf) {  
	posConf = new host.PosConfig("LREC","tgtLREC");  
	posConf.w  = 300;  
	posConf.h  = 250;  
	posConf.z  = 1000;  
	pos    = new host.Position("LREC","<h1>Hello World I'm an Ad<h1>",null,posConf);  
	host.render(pos);  
	}  
	}  
	}   
	w.remove_content  = function()  
	{  
	var skipID = "LREC",  // we want to skip the LREC position, 
	and leave it in the page  
	list   = $sf.info.list,  
	cnt     = list.length,  
	to_rem = [],  
	idx     = 0,  
	pos;   
	while (cnt--)  
	{  
	pos = list[idx++];  //$sf.host.Position  
	if (pos.id == skipID) continue;  
	to_rem.push(pos.id);  
	}  
	$host.nuke(to_rem); } //remove all but the LREC position; 
})();  
</script>
</div>
```

### 4.4 类`$sf.host.Config` 
`$sf.host.Config(conf) `
主站配置类是用来描述了SafeFrame 主站API的配置选项的。该类配置主站使用的整体功能和设置。

>  **主站实现注意事项**
>  主站类`$sf.host.Config`只应在的SafeFrame主站API中存在一次，并应在SafeFrame容器处于非活动状态的时候被构造来启动配置选项。
> 当构建时，如果不是先前定义的话，详细信息将写入内嵌`$sf.host.conf`命名空间中。
> 

如果没有指定初始参数，则返回现有配置。如果返回值为null，则不存在有效的配置。

当`$sf.host.Config`被构造，其他SafeFrame主站类从得到的配置读取，以确定容器的渲染方式。如果先前没有定义，任何所得的值会被添加到内联`$sf.host.conf`命名空间中。
**参数**

 - `{Object} conf`
 A list of key value pairs to use for the configuration. 

**字段**
以下字段可以在`conf`参数中返回：

 - `{String}` **conf.cdn**
 Host of the CDN used to fetch SafeFrame resources. This value should always be a different domain than your web page 
 　Sample value: `"http://l.yimg.com"`
 - `{String}` **conf.ver**
 The version number of the SafeFrame to be used, provided in the format [number]-[number]-[number]. 
 　Sample value: `"2-3-4"
 - `{String}` **conf.renderFile**
 The partial path and filename of the file from the cdn property that is used as the base document for external party content to be rendered using the SafeFrame. 
 - `{String}` **conf.hostFile** 
 The URL string to the Host-side JavaScript file to be used. 
 - `{String}` **extFile**
 The URL string to the External Party-side JavaScript file to be used. 
 - `{String}` **bootFile**
 The URL string to the External Party-side JaveScript file to be used for bootstrapping the SafeFrames library, processing SafeFrames tags, and rendering content. 
 - `{Number}` **conf.to** 
 The maximum amount of time (in seconds) that a render process can take before the operation can be aborted. 
 Rendering the external party content in a SafeFrame container is an asynchronous process, which is done by rendering an x-domain iframe tag. This number defines the maximum amount of time that the render operation can spend in the "loading" state before a time-out error is generated. 
　 Sample value: `30` 
 - `{Object}` **conf.positions** 
 An object defining literal representations of `$sf.host.PosConfig` objects, keyed by id, to be used to configure each position in the page  
 - `{Boolean}` **conf.auto** *(Optional)*
 Whether or not automatic bootstrapping and rendering of SafeFrame tags should occur. Default is true. If set to false, SafeFrame tags will just add to the `$sf.info object`. 
 - `{String}` **conf.msgFile** *(Optional)*
 The partial path and filename of the file from the cdn property that is used to as a proxy for x-domain communication. Only required for older browsers that do not support HTML 5. 
 - `{Boolean}` conf.debug** *(Optional)*
 Whether or not to run the SDK in debug mode, which will also use un-minified JS code, separated files, etc. 

**事件**

 - `onBeforePosMsg(id, msgName, data)` 
 A function that gets called each time a position sends a request for some functionality. Returning true cancels the command request. 
 **参数: **
 　`{String}` **id**
 　The id of the position that has started its render process 
 　`{String}` **msgName** 
 　The type of message being sent 
 　`{String}` **data** *(Optional)* 
 　Data that gets passed through 
 - `onEndPosRender(id)` 
 A  function which gets called each time a position has finished rendering  
 **参数: **
 　`{String}` **id**
　 The id of the position that has started its render process 
 - `onFailure(id)` 
 A  function which gets called anytime a render call has failed or timed out 
 **参数: **
　 `{String}` **id** 
　 The id of the position that has started its render process 
 - `onPosMsg(id, msgName, data)` 
 A callback function which gets called each time a position sends a message up to your web page 
 **参数: **
　 `{String}` **id**
　 The id of the position that has started its render process 
　 `{String}` **msgName** 
 　The name / type of message being sent 
 　`{String}` **data** *(Optional)*
 　Data that gets passed through 
 - `onStartPosRender(id)`
 A callback function which gets called each time a position is about to be rendered 
 **参数: **
　 `{String}` **id**
 　The id of the position that has started its render process 
 - `onSuccess(id) `
 A callback function which gets called anytime a render call has successfully completed. 
 **参数: **
 　`{String}` **id**
　 The id of the position that has started its render process

**相关章节**: 

 - 4.2 命名空间 `$sf.host.conf `
 - 4.5 类 `$sf.host.PosConfig` 

**例子**
``` html
<div id='tgtLREC'></div>  
<script type='text/javascript'>  
 
(function() {     
	var w = window, sf = w["$sf"], pub = sf && sf.host, Config = pub 
	&& host.Config,    
	 
	CONF_CDN   = "http://l.yimg.com",    
	CONF_ROOT   = "/sf",    
	CONF_VER  = "2-3-4",    
	CONF_RFILE  = "/html/render.html",    
	CONF_TO    = 30;     
	 
	function on_endposrender(posID, success)    
	{      
	//a render action success    
	}     
	 
	function on_posmsg(posID, msg, data)    
	{        
	//listen for messages    
	}     
	 
	w.init_SafeFrame  = function()    
	 
	{        
	var conf, confDesc;       
	 
	if (Config) {        
		conf = Config();       
		if (!conf) {          
			confDesc  =          
			{            
				debug:       true, 
				cdn:        CONF_CDN,  
				root:       CONF_ROOT,  
				ver:        CONF_VER,  
				renderFile:      CONF_RFILE,  
				to:        CONF_TO  
				onEndPosRender:    on_endposrender,  
				onPosMsg:      on_posmsg,  
				positions:            
				{  
					"LREC":  
					{  
					id:    "LREC",  
					w:    300,  
					h:    250,  
					z:    1000,  
					dest:  "tgtLREC"  
					}  
				}  
			};  
			conf = new Config(confDesc);  
			if (conf) {  
				alert("SafeFrame Host Config successful");  
			}  
		}  
	}    
	}   
})();  
</script>
</div>
```

### 4.5 类`$sf.host.PosConfig` 
**$sf.host.PosConfig**`(posIDorObj, destID, baseConf) `

该类描述了一个`$sf.host.Position`对象应该如何被渲染。每个唯一ID只能有一个`PosConfig`对象可以存在。如果多于一个`PosConfig`对象由相同的ID建造，原来的PosConfig的初始值将被覆盖。主站主机仍然可以支持具有相同特征的多个广告位（即两个唯一的LREC）;他们只是需要有不同的ID（即LREC1和LREC2）

类结构可被认为是一个工厂，其中，在内部，构成的所有实例都被监视，以便可以自动连接整体配置选项和数据。

**参数：**

 - `{String|Object}` **posIDorObj**
 If this value is provided as a string, then it is used as the id property of the instance. If the value is returned as an object, then it is a descriptor that populates the properties of the instance. 
 - `{String}` **destID**
 The HTML element ID attribute string into which the content is to be rendered. 
 - `{Object}` **baseConf**,*(Optional)*
 An optional object that defines a representation of an `$sf.host.Config` object and is used in cases where no initial Host configuration was pre-defined. This option enables a shortcut for automatic host configuration if necessary and is usually used in conjunction with SafeFrame tags. If specified when a Host configuration already exists, this parameter is ignored. 

**字段**

 - `bg`
 The background color to be used inside the safe frame. Default value is "transparent". 
 - `css`
 Style-sheet text or a URI to a CSS file that defines additional CSS to be used inside the SafeFrame iframe. Default value is "". 
 - `dest` 
 The HTML element ID into which the content is to be rendered. 
 - `H` 
 The height (in pixels) of the SafeFrame iframe to be created for the content specified.  
 - `id` 
 A unique identifier for the position or content. Used to link the `$sf.host.Position` object with a configuration. Specifying the id as "DEFAULT" means that this configuration will be used as the default values for other `$sf.Position` objects created. 
 - `size` 
 A string representing the width and height (in pixels) of the safe frame to be created for the content specified. Setting this value also sets the w and h properties respectively Example: `"300x250"` 
 - `tgt` 
 The target window name for where hyperlink clicks should be routed to unless otherwise specified. Default value is "_blank". If a URL is provided, it opens in a new window. The values "_self" and "_parent" are NOT allowed and if provided the value "_top" is used instead. 
 - `w` 
 The width (in pixels) of the SafeFrame iframe to be created for the content specified. 
 - `z`  
 The z-index value to be used for the SafeFrame iframe. 
 - `supports` 
 An object identifying the features that the host supports relative to the content specified.  

**方法**

 - `toString()` 
 A method that serializes the position into a string using query-string encoded syntax. 

**例子**
 `//See $sf.host.Config example`

### 4.6 类`$sf.host.Position`
`**$sf.host.Position**(posIDorObj, html, meta, config)`

一个用于描述在一个安全的框架中渲染的HTML内容的类。

**参数**

 - `{String|Object}` **posIDorObj**
 REQUIRED, if is a string, used as the id property of the instance. If is an object, it is used as a descriptor to fill out the properties of the instance. 
 - `{String}` **html**
 REQUIRED, the string content to be rendered into the safe frame described by this instance 
 - `{Object}` **meta Optional** 
 An object with key/value pairs defining customizable metadata about the position 
 - `{Object}` **config Optional** 
 An object representing position config overrides 

**字段**

 - `{Object}` **config** 
 Config information defines how SafeFrame renders a position. This object can override values already set in the associated config. 
 - `{String}` **html** 
 The HTML content to be rendered inside the safe frame, or a URL to HTML content returned that is returned using a SCRIPT tag. 
 - `{String}` **id**
 A unique identifier for the position. If present, this value is used to lookup a 
`$sf.host.PosConfig` object. 
 - `{Object}` **meta** 
 Metadata information in the form of an object of any number, combination key, or value pairs to store host or content-related metadata. 
 - `{String}` **src** 
 A URI to be used as a SCRIPT tag that renders the contents in the SafeFrame. Setting this value changes the value of the HTML property and is used mostly for short-hand purposes.  
 The purpose of this field is to enable content to be fetched when the HTML content is no readily available. Setting this property creates an HTTP request for content to the URI specified. Because the URI provided is in a SCRIPT context, content must be returned in JavaScript form. This process prevents the creation of other iframes that would otherwise damage the system because content within any created iframes is denied access to the external content API. 
 The URI provided may contain MACRO place holders that SafeFrame will populate. This feature can be used to gather information from a Web browser that can be passed in the HTTP request and is useful for cases when retrieved content requires information about the Web browser environment only available to the host.
 SafeFrame populates the following values: 

 　- `{String} ${sf_ver}`  
 　The string representation of the current version of SafeFrame 
　- `{Number} ${ck_on}`
 　Indicates whether cookies are enabled on the browser: 1 for true, 0 for false. 
　- `{String} ${flash_ver}`  
 　Identifies which version of Flash is enabled in the browser. If Flash is not detected, the value is set to 0.

**例子**
``` javascript
function define_content() 
{ 
var pub = $sf.host, PosConfig = host.PosConfig, PosMeta = host.PosMeta, 
Pos = host.Position, pos, posConf, posMeta; 
 
  posConf   = new PosConfig("LREC", "tgtLREC"); 
  posConf.w   = 300; 
  posConf.h  = 250; 
  posConf.z  = 1000; 
 
posMeta    = new PosMeta({"context":"Music"}); 
 
  //a shared meta object will now contain 
  //  context:   "Music" 
  //  sf_ver:   "1-0-1", 
  //  flash_ver:  11 
 
  pos     = new Pos("LREC", 
"http://getsomeads.com?pos=LREC&f=${flash_ver}&sf=${sf_ver}", posMeta, 
posConf); 
  //note that the ${flash_ver} and ${sf_ver} macros will get filled out 
automatically 
  // 
  //so if flash 11 is installed, and we are using SafeFrame version 1 
  //the URI for the script tag created will be 
  // 
  // "http://getsomeads.com?pos=LREC&f=11&sf=1-0-1" 
 
  host.render(pos);
```
**方法**

 - `toString()` 
 A method that serializes the position into a string using query-string encoded syntax 

**例子**
``` html
<div id='tgtLREC'></div>  
<script type='text/javascript'>   
 
(function() {  
 
	var w = window, sf = w["$sf"], pub = sf && sf.host, Config = pub 
	&& host.Config,   
	 
	CONF_CDN   = "http://l.yimg.com",  
	CONF_ROOT   = "/sf",  
	CONF_VER  = "2-3-4",  
	CONF_RFILE  = "/html/render.html",  
	CONF_TO    = 30;   
	 
	function on_endposrender(posID, success)  
	{  
	//a render action success  
	}   
	 
	function on_posmsg(posID, msg, data)  
	{  
	 //listen for messages  
	}     
	 
	w.init_render  = function() 
	{  
	 var conf, confDesc, posConf, pos;      
	 
	if (Config) {  
		conf = Config();  
		if (!conf) {  
			confDesc  =  
			{          
				debug:       true,  
				cdn:        CONF_CDN,  
				root:       CONF_ROOT,  
				ver:        CONF_VER,  
				renderFile:      CONF_RFILE,  
				to:        CONF_TO  
				onEndPosRender:    on_endposrender,  
				onPosMsg:      on_posmsg  
			};          
			conf = new Config(confDesc);  
			if (conf) {  
				posConf    = new 
				host.PosConfig("LREC","tgtLREC");  
				posConf.w  = 300;  
				posConf.h  = 250;  
				posConf.z  = 1000;    
				pos    = new 
				host.Position("LREC","<h1>Hello World, I'm an Ad</h1>");  
				//note that b/c you constructed a 
				PosConfig object already with an id of 
				"LREC", the configuration will be 
				grabbed   
				 
				host.render(pos);  
			}        
		}      
	}    
	}   
})();  
</script>
```

### 4.7 类`$sf.host.PosMeta`
**$sf.host.PosMeta**`(shared_obj, ownerKey, obj)`
此类定义一个特定的位置的元数据。元数据可以被共享，或键入，到特定的数据所有者（如果需要的话，允许隐藏）。此对象中存储的值，不能改变;被构造完时，他们才可以被设置，且是只读的。典型地存储在该对象的数据用于专有用途。

当一个SafeFrame容器被构造和渲染，这里存储的信息将提供给第三方API。在某些元数据需要被保护的情况下，共享和非共享内部对象被创建。例如，`ownerKey`属性可能是从一个服务器生成的签名。

在SafeFrame的容器内，函数用于访问元数据，使外部各方不能用迭代发现它。在这种情况下，用作`ownerKey`的签名可以在容器内部被用来访问它，且只允许向可信方访问。

每当`$sf.host.PosMeta`对象被创建了，以下信息会总是默认在"共享"部分中出现。

 - `{String} sf_ver`  
 The string representation of the current version of SafeFrame 
 - `{Number} ck_on`  
 Identified whether cookies are enabled on the browser: 1 for true, 0 for false. 
 - `{String} flash_ver`  
 Identifies which version of Flash is enabled in the browser. If Flash is not detected, the value is set to 0. 

 我们也看看`$sf.host.Position`的"src"属性。当`PosMeta`对象被构造，并可以为了作为宏观字段的"src"属性在URL上自动被传递，上述的值被定义。

**参数：**

 - `{Object}` **shared_obj** *(Optional)*
 An object containing key /value pairs for shared metadata 
 - `{String}` **ownerKey** *(Optional)* 
 A key name to identify the owner or a particular set of metadata. 
 - `{Object}` **obj** *(Optional)*
 An object containing the key value pairs of metadata 
 欲知关于传递元数据的详情，请参阅相关的函数`$sf.ext.meta`。 

**方法**

 - `{String|Number|Boolean}` **value**`(propKey, ownerKey)` 
 A method retrieves a metadata value from this object. 
 方法参数: 
 - `{String}` **propKey**
 The name of the value to retrieve 
 - `{String}` **ownerKey** (Optional) 
 The name of the owner key of the metadata value. By default, it is assumed to be shared, so nothing needs to be passed in unless looking for a specific proprietary value 

**返回:** 

 - `{String|Number|Boolean}` 

**例子** 
``` html
<!-- Host Side tags --> 
<div id='tgtLREC'></div> 
<script type='text/javascript'> 
 
var w = window, sf = w["$sf"], pub = sf && sf.host, Config = pub && 
host.Config, conf, posConf, posMeta, shared, non_shared, pos; 
 
if (Config) { 
 
  conf = Config(); 
if (!conf) conf = new 
Config({debug:true,cdn:"http://l.yimg.com",root:"/sf",
	ver:"2-3-4",renderFile:"/html/render.html",to:30}) 
  if (conf) { 
    posConf    = new host.PosConfig("LREC","tgtLREC"); 
    posConf.w  = 300; 
    posConf.h  = 250; 
    posConf.z  = 1000; 
    shared   = {"context": "Music"}; 
    non_shared = {spaceID: 90900909090, adID: 3423423432423}; 
	posMeta  = new host.PosMeta(shared,"y",non_shared); 
	//Use a signature for a key name (instead of "y"), 
	//if you don't want 3rd parties accessing this data  
	pos    = new host.Position("LREC","<Hello World I'm an Ad>",posMeta,posConf); 
    host.render(pos); 
  } 
} 
</script> 
 
 
<!-- External Party tag --> 
<script type='text/javascript'> 
 
var w = window, sf = w["$sf"], ext = sf && sf.ext, cntxt = ext && 
ext.meta("context"), yspaceID = ext && ext.meta("spaceID","y"); 
 
alert(cntxt); //will say Music; 
 
alert(yspaceID); //will say 90900909090 
</script>
```

### 4.8 函数 `$sf.host.boot` 
boot函数用于查找，处理和自动渲染数据的标签。它返回一个布尔值，响应是否已发现任何新的，未经加工的项。一旦进行处理，将所得的SafeFrame数据被添加到`$sf.info`。并且如果自动字段在`$sf.host.config`类中被设置为true，boot函数启动在数据定义的内容的渲染过程。

**返回值** 

 - `{Boolean}`  
 Indicates whether any new, unprocessed items have been found 

**相关章节**

 - 3   SafeFrame标签
 - 4.3 命名空间`$sf.info` 
 - 4.2 命名空间`$sf.host.conf` 

**例子**
``` html
<!-- SafeFrame Inline Tag -->   
<div id="tgtLREC">  
<script type='text/x-safeframe' class='sf_data'>  
{  
	id: "LREC",  
	src: "http://secondarydomain.com/safeframe",   
	conf:  
	{    
	dest: "tgtLREC",  
	size: "300x250"  
	}   
	meta:  
	{    
		rmx:  
		{  
			sectionID:"14800347",   
			siteID: "140509"   
		}  
	}  
}  
</script>  
<script type='text/javascript'>  
try {  
$sf.host.boot();  
} catch (e) {  }  
</script> 
 </div> 
```

### 4.9 函数`$sf.host.status`
status函数用于确定位置的状态。它返回一个指示页面中的是否有任何位置当前在所渲染的过程中，或者如果有一些其它的操作，诸如扩展，正在发生的布尔值响应。
**参数**
 - `{Object} positions`
 可选的对象参数提供了一个空的对象引用，它可以被代表每个SafeFrame正在管理的$sf.host.Position对象（使用其ID属性）的密钥列表中的一个填充。每个键的值包含一个对象，该对象有一个代表容器的当前状态的状态代码串。在此版本中，可能值如下：
 　•  ready: the container is available for rendering but has not yet been rendered 
 　•  loading: the container is currently in the process of being rendered 
 　•  expanding: the container is currently in the process of expanding 
 　•  expanded: the container is currently in expanded state 
 　•  collapsing: the container is currently in the process of collapsing 
 　•  error: the container has experienced an error that is preventing any interaction 

**返回值**
 - `{Boolean}`  
 Indicates whether or not the SafeFrame SDK is busy with an operation where the configuration cannot be updated 

**相关章节**

 - 5.1  命名空间`$sf.ext` 
 - 5.7  函数`$sf.ext.status` 

**例子**
``` html
<script type='text/javascript'>   
var posDetail = {};  
var isBusy    = $sf.host.status(posDetail);  
var posID    = "";  
var posInfo, posInfoStatus, posInfoDesc, posIDProc;    
 
if (isBusy) {  
//Cannot change configuration while operations are ongoing, 
inspect object to determine what is going on   
 
for (posID in posDetail)  
{  
	posInfo = posDetail[posID];  
	//object has "status", "id", and "desc" properties   
	 
	posInfoStatus = posInfo.status;  
	switch (posInfoStatus)  
	{  
		case "expanding":  
		posIDProc = posID;  
		break;  
		case "collapsing":  
		posIDProc = posID;  
		break;  
	}  
	if (posIDProc) break;  
}   
if (posIDProc) alert(posIDProc + ", is " + posInfoStatus);  
}  
</script> 
```

### 4.10 函数`$sf.host.nuke `
nuke函数用于从页面去除SafeFrame容器的位置。即使互动正在审理或正在发生，这个功能也可以调用，且将中止未完成的操作或渲染。

nuke函数被提供以迁就SafeFrame容器位置不能容易地在常规情况下除去的情况。例如，nuke函数可以被用来去除在不具有可以像它在Web浏览器那样被关闭的网页的本机应用中的SafeFrame容器的位置。

Nuke不需要把新的内容加载到现有的位置。渲染函数会处理设置新的内容位置。

**参数**

 - `{String|String[]}` **id** 
 The id of the position to be removed; use "*" to remove all positions. 

**例子**
``` html
<div id='tgtLREC'></div>  
<script type='text/javascript'>   
 
(function() {   
var w = window, sf = w["$sf"], pub = sf && sf.host, Config = pub 
&& host.Config,   
 
CONF_CDN   = "http://l.yimg.com",  
CONF_ROOT   = "/sf",  
CONF_VER  = "2-3-4",  
CONF_RFILE  = "/html/render.html",  
CONF_TO  = 30;   
 
function on_endposrender(posID, success)  
{  
//a render action total failure  
} 
 
function on_posmsg(posID, msg, data) 
{  
//listen for messages  
}   
w.render_content  = function()  
{  
	var conf, posConf, pos,confDesc;   
	 
	if (Config) {  
		conf = Config();  
		if (!conf) {  
		confDesc  =  
		{  
		debug: true,  
		cdn:    CONF_CDN,  
		root:    CONF_ROOT,  
		ver:    CONF_VER,  
		renderFile:  CONF_RFILE,  
		to:    CONF_TO  
		onEndPosRender:  on_endposrender,  
		onPosMsg:    on_posmsg  
		};  
		conf = new Config(confDesc);  
		}  
		if (conf) {  
			posConf    = new host.PosConfig("LREC","tgtLREC");  
			posConf.w  = 300;  
			posConf.h  = 250;  
			posConf.z  = 1000;  
			pos      = new host.Position("LREC",
				"<h1>Hello World I'm an Ad<h1>",null,posConf);  
			host.render(pos);  
		}  
	}  
}   
 
w.remove_content  = function()   
{  
	host.nuke("*"); //will remove all positions rendered or in 
	process of rendering.  
	//could also pass "LREC" in this case, or 
	"LREC","SKY" if "LREC" and "SKY" ads were 
	configured.  
}  
})();  
</script> 
```

### 4.11 函数`$sf.host.get `
get函数用于获取SafeFrame容器的位置配置的参考。当SafeFrame回调函数之一通知一个事件的主站代码，这个函数被用来获取与有疑问的位置关联的PosConfig对象的引用。

**参数**

 - `{String}` **id** 
 The id of the position to get. 

**例子**
``` html
<div id='tgtLREC'></div>  
<script type='text/javascript'>   
 
(function() {   
	var w = window, sf = w["$sf"], pub = sf && sf.host, Config = pub 
	&& host.Config,   
	 
	// Configuration omitted for brevity   
	 
	function on_endposrender(posID, success)  
	{  
		var adPos = host.get(posID); 
		if(!success) { 
		  host.nuke(posID); 
		} 
	}
})();
</script> 
```

### 4.12 函数`$sf.host.render`
render函数用于呈现一个或多个的SafeFrame位置。

你可以同时传递一个或多个`$sf.host.Position`对象（或对象的表现形式）以渲染一组容器。如果您传递回调函数给`$sf.host.Config`类，你会看到以以下顺序调用的回调函数：

 1. onStartPosRender 
 2. onEndPosRender (success / failure) 
 3. onBeforePosMsg (if ad sends commands such as for expansion etc, allows you to return true to reject the message) 
 4. onPosMsg (if ad sends commands such as for expansion, etc.) 

> **主站实现注意事项**
> 当`$sf.host.nuke`已被调用给当前渲染的位置，`onEndPosRender`回调不能初始化。

**参数**

 - `{Object|Object[]|$sf.host.Position|$sf.host.Position[]}` **data**
 A representation of a $sf.host.Position object to be rendered

**相关章节**

 - 4.4 类`$sf.host.Config` 
 - 4.5 类`$sf.host.PosConfig`
 - 4.6 类`$sf.host.Position` 

**例子**
``` html
<div id='tgtLREC'></div>  
<script type='text/javascript'>  
(function() {   
var w = window, sf = w["$sf"], pub = sf && sf.host, Config = pub 
&& host.Config,   
 
CONF_CDN   = "http://l.yimg.com",  
CONF_ROOT   = "/sf",  
CONF_VER  = "2-3-4",  
CONF_RFILE  = "/html/render.html",  
CONF_TO  = 30;   
 
function on_endposrender(posID, success)  
{  
//a render action success  
}  
 
function on_posmsg(posID, msg, data)  
{  
//listen for messages  
} 
w.render_content  = function()  
{  
	var conf, posConf, pos,confDesc;   
	 
	if (Config) {  
		conf = Config();  
		if (!conf) {  
		confDesc =  
		{    
			debug:       true,  
			cdn:        CONF_CDN,  
			root:        CONF_ROOT,  
			ver:        CONF_VER,  
			renderFile:      CONF_RFILE,  
			to:        CONF_TO  
			onEndPosRender:    on_endposrender,  
			onPosMsg:      on_posmsg  
		};  
		conf = new Config(confDesc);  
		}  
		if (conf) {  
			posConf    = new host.PosConfig("LREC","tgtLREC");  
			posConf.w  = 300;  
			posConf.h  = 250;  
			posConf.z  = 1000;  
			pos    = new host.Position("LREC",
				"<h1>Hello World I'm an Ad<h1>",null,posConf);  
			host.render(pos);        
		}  
	} 
}  
 
w.remove_content  = function()  
{  
	host.nuke("*"); //will remove all positions rendered or in 
	process of rendering.  
	//could also pass "LREC" in this case, or 
	"LREC","SKY" if "LREC" and "SKY" ads 
	were configured.  
}  
})();  
</script><div id='tgtLREC'></div>  
<script type='text/javascript'>  
(function() {   
	var w = window, sf = w["$sf"], pub = sf && sf.host, Config = pub 
	&& host.Config,   
	 
	CONF_CDN   = "http://l.yimg.com",  
	CONF_ROOT   = "/sf",  
	CONF_VER  = "2-3-4",  
	CONF_RFILE  = "/html/render.html",  
	CONF_TO  = 30;   
	 
	function on_endposrender(posID, success)  
	{  
	//a render action success  
	}  
	 
	function on_posmsg(posID, msg, data)  
	{  
	//listen for messages  
	}
})();  
</script> 
```

## 5.第三方API实施
SafeFrame第三方API使用的命名空间和函数在第5.1至5.10节中描述。

### 5.1 命名空间`$sf.ext`
`$sf.ext`

命名空间ext提供了一系列用于检索关于所述容器的各种类型的信息的方法。第三方使用此命名空间来定义JavaScript类和对象，第三方广告可以用于在一个SafeFrame的环境中与主站内容进行交互。

被用于执行交互作用的SafeFrame方法是异步的，使得只能使用来自API的回调来确定任何成功或失败。这些方法也可保持其状态，这意味着它们被保护以免重复调用。

例如：

 - `$sf.ext.expand`调用被初始化. 
 - 在后台，SafeFrame处理`$ sf.ext.expand`并发送消息到主站。 
 - 如果`$ sf.ext.expand`再次被调用，处理第一次调用之前，因为只可以同时处理一个命令，它会被认为是一个错误。 
 - 如果是使用`$ sf.ext.register`提供的`$ sd.ext.expand`回调函数，那么函数被调用，而且一旦处理，成功或失败的通知会发出。
 - 成功或失败的结果产生之后，`$ sf.ext.expand`可以再次调用。

**事件**
`<static>  $sf.ext.__status_update(status, data)`

此事件提供第三方广告内容的状态。事件是从第三方SDK发射的，以便您可以通过`$ sf.ext.register`注册一个回调。

> **实现注意事项** 
> `$ sf.ext.__ status_update`命名空间仅仅是隐式的，在JavaScript的层次结构不存在，但它在这里被调用，以记录当函数被调用，并提交给`$ sf.ext.register`的可能的参数。
> 

回调函数与至少两个参数被调用：第一，一个表示状态的变化的字符串；第二，一个表示生成状态更新事件的命令的字符串，这是由生成的状态更新事件的第三方API初始化的命令发出的。如果第二个参数是一个空字符串，其含义是主站已强制状态更新，而不是由第三方API正在发起的命令启动的。

**事件参数：**

 - `{String}` **status** 
 The status code string notifying external content of container updates. The following status codes are available: 
 　**expanded** 
 　The container has been expanded. 
 　**collapsed** 
 　The container is in the default collapsed state. 
 　**failed** 
 　A command initiated by the external party API did not succeed. 
 　**geom-update** 
 　The container geometry information has changed. Sent for events such when the  browser window is resized, parent container scrolls, or other geometric changes. 
 　***focus-change*** 
 　*The browser window / tab has become active (“focus”), or become in-active  (“blur”).* 
 - `{Object}` **data** *(Optional)* 
 Contains information about the original message or action requested of the Host or supplied by the host as a result of changes in the page. The following objects may be issued: 
 　**cmd**  
 　The original command sent with possible values such as: exp-ovr, exp-push, read-cookie, write-cookie, etc. 
 　**reason** 
 　Description information about whether the command succeeded or failed. 
 　**info** 
 　The information sent as part of the command echoed back to the caller, such as dimensions for expansion, the data to set for a cookie, etc. 

 **相关章节**

  - 5.2 函数`$sf.ext.register` 
  - 5.5 函数`$sf.ext.expand`

### 5.2 函数`$sf.ext.register`
`$sf.ext.register(initWidth, initHeight, cb) `

可用性：同步（可随时请求）

第三方注册函数注册SafeFrame平台，以接受SafeFrame第三方API调用。第三方广告声明初始的（折叠的）的宽度和高度。除了宽度和高度，此函数还可以定义一个回调函数，通知有关各种状态信息的第三方内容。

最初的宽度和高度参数是必需的，以便SafeFrame通知主站渲染第三方内容所需的呈现空间。回调函数是返回每一个命令处理的成功或错误代码，通知第三方发送的每一个命令的执行状态的方法。然后，第三方可做出相应的反应。在等待成功或失败通知的时候，命令应该只被调用一次。在成功或失败通知之前，任何后续的调用将被忽略。

**参数**

 - `{Number}` **initWidth**
 The initial / original width of the 3rd party content 
 - `{Number}` **initHeight** 
 The initial / original height of the 3rd party content 
 - `{Event}` **cb** 
 An optional callback function that will be called as a notification of event status

**返回：**

 - `void`

**相关章节**

 -  第5.1节里的事件细节

**例子**
``` html
<!-- External Party tag --> 
<script type='text/javascript'> 
 
var w = window, sf = w["$sf"], ext = sf && sf.ext; 
 
function status_update(status, data) 
{ 
 
} 
if (ext) { 
  try { 
    ext.register(300, 250, status_update); 
 
alert(ext.meta("context"));  
//read some metadata passed in from the host side 
  } catch (e) { 
    alert("no SafeFrame available"); 
  } 
} 
</script>
```

### 5.3 函数 `$sf.ext.supports` 
`$sf.ext.supports()`

可用性：同步（可随时请求）

返回一个有代表这个特定容器的什么功能已被打开或关闭的键的对象。

**返回**

 - `{Object}`  
 An object containing a list of SafeFrame container features that are available, defined as follows: 
 　`{Boolean} exp-ovr`  
Whether or not expansion is allowed in overlay mode. Default value is true. 
 　`{Boolean} exp-push`  
 　Whether or not expansion is allowed in push mode. Push expansion, a method of content expansion in which Host content is "pushed" instead of expanding over the content, is not yet supported in SafeFrame but may be supported separately by the Host. Default value is false.  
 　`{Boolean} read-cookie` 
 　Whether or not the host allows external party content to read host cookies. Default value is false. 
 　`{Boolean} write-cookie` 
 　Whether or not the host allows external party content to write cookies to the host domain. Despite value of true, the host may reject cookie values when offered if deemed appropriate. Default value is false. 

**例子**
``` javascript
//Sample JavaScript implementation 
//Let's say that a 300x250 ad has been declared to fully expand to 400 
pixels to the left and 200 pixels to the top. 
 
 
function feature_check(which) 
{ 
  var o = $sf.ext.supports(); 
 
  return (o && o[which]); 
} 
 
 
function expand() 
{ 
  if (feature_check("exp_push")) { 
    $sf.ext.expand({l:400,t:200,push:true}); 
  } 
} 
```

### 5.4 函数` $sf.ext.geom`
` $sf.ext.geom()`

可用性：同步（可随时请求）

所述的geom函数使SafeFrame容器的几何尺寸和位置，与它的与浏览器或应用程序窗口和其中主站内容正在被观看的设备的屏幕边界相关的内容交换。

> **主站实现注意事项** 
> 如果调用，主站需要返回所请求的值。
> 

该信息可用于：

 - 决定内容扩展的可用的方向和尺寸
 - 确定SafeFrame容器是否“在视图中”

> **广告可见度注** 
> SafeFrame提供了可在根据接受的工业建议的可用性方面报告的信息；然而，SafeFrame不直接报告可见度指标。一个对于报告可见度是必要的指标是持续时间，它必须通过注册状态更新监听器来得到，以确定为`self.iv`被注册为`true`要用多久的持续时间。如果要了解关于`$ sf.ext.register`功能的详细信息，请查阅第5.2节。
> 

**返回**

 - `{Object} g`  
 An object containing sub objects with geometric information about the container. Geometric information may be returned as described in the following lists
 
 **win** 
 Identifies the location, width, and height (in pixels) of the browser or application window boundaries relative to the device screen. 
 　•  {Number} t 
 　The y coordinate (in pixels) of the top boundary of the browser or application window relative to the screen 
 　•  {Number} b  
 　The y coordinate (in pixels) of the bottom boundary of the browser or application window relative to the screen 
 　•  {Number} l  
 　The x coordinate (in pixels) of the left boundary of the browser or application window relative to the screen 
 　•  {Number} r  
 　The x coordinate (in pixels) of the right boundary of the browser or application window relative to the screen 
 　•  {Number} w  
 　The width (in pixels) of the browser or application window (win.r – win.l) 
 　•  {Number} h  
 　•  The height (in pixels) of the browser or application window (win.b – win.t) 
 　
 **self** 
 Identifies the z-index and location, width, and height (in pixels) of the SafeFrame container relative to the browser or application window (win). In addition, width, height, and area percentage of SafeFrame content in view is provided, based on how much of the container is located within the boundaries of the browser or application window (win). 
 　•  {Number} t  
 　The y coordinate (in pixels) of the top boundary of the SafeFrame container 
 　•  {Number} l  
 　The x coordinate (in pixels) of the left side boundary of the SafeFrame container  
 　•  {Number} r  
 　The x coordinate (in pixels) of the right side boundary of the SafeFrame container (self.l + width of container) 
 　•  {Number} b  
 　The y coordinate (in pixels) of the bottom boundary of the SafeFrame container (self.t + height of container) 
 　•  {Number} xiv  
 　The percentage (%) of width for the SafeFrame container that is in view (formatted as "0.14" or "1") 
 　•  {Number} yiv  
 　•  The percentage (%) of height for the SafeFrame container that is in view (formatted as "0.14" or "1") 
 　•  {Number} iv  
 　The percentage (%) of area for the SafeFrame container that is in view (formatted as "0.14" or "1") 
 　•  {Number} z  
 　The Z-index of the SafeFrame container 
 　•  {Number} w  
 　The width (in pixels) of the SafeFrame container  
 　•  {Number} h  
 　The height (in pixels) of the SafeFrame container 
 　
 **exp** 
 Identifies the expected distance available for expansion within the host content along with information about whether controls allow the end user to scroll the page. If “scrollable,” the SafeFrame content can expand to dimensions greater than those provided.  
 　•  {Number} t  
 　The number of pixels that can be expanded upwards 
 　•  {Number} l  
 　The number of pixels that can be expanded left 
 　•  {Number} r  
 　The number of pixels that can be expanded right 
 　•  {Number} b  
 　The number of pixels that can be expanded down 
 　•  {Number/Boolean} xs  
 　A response that indicates whether the host content is scrollable along the x-axis (1 = scrollable; 0 = not scrollable)  
 　•  {Number/Boolean} yx  
 　A response that indicates whether the host content is scrollable along the y-axis (1 = scrollable; 0 = not scrollable)
 　
 由于计算几何信息和交换消息会影响性能，几何信息应只在以下时间段更新：
 
 　**SafeFrame容器的首次渲染**
 　当SafeFrame容器首次被渲染时，`$sf.ext.geom`应被处理，并与要渲染的第三方内容一起发送结果。
 　
 　**当改变SafeFrame容器的大小或位置时**
 　当使用下列功能之一改变容器尺寸或位置时，`$ sf.ext.geom`函数应该被处理：
 　　o  `$sf.ext.expand` 
 　　o  `$sf.ext.collapse`
 　　
 　**当来源于主站的外部更新被接收时** 
 　　o  收到来自从其中该容器的几何形状已经被主站自己更新的主站端的信息，例如强制内容折叠。查看注册回调消息。 
 　　o  在所有可视面积的滚动，但是只允许每秒一个更新（节流）。
 　　o  在所有可视面积的大小调整，但是只允许每秒一个更新（节流）。

> **主站实现注意事项** 
> 对于滚动或调整事件，SafeFrames主站实施应该只侦听要么是裁剪或滚动上面的SafeFrame容器上的第一个父HTML元素的事件。
> 

**例子**
``` javascript
//Sample JavaScript implementation 
//Let's say that a 300x250 ad has been declared to fully expand to 400 pixels 
to the left and 200 pixels to the top. 
 
function expand() 
{ 
    var w = window, sf = w["$sf"], ext = sf && sf.ext, g, ex; 
 
    if (ext) { 
      try { 
        g  = ext.geom(); 
        ex  = g && g.exp; 
        if (Math.abs(ex.l) >= 400 && Math.abs(ex.t) >= 200) { 
            ext.expand({l:400,t:200}); 
        } 
      } catch (e) { 
        //do not expand, not enough room 
      } 
    } else { 
      //api expansion not supported 
    } 
  } 
 
 function status_update_handler(status) 
  { 
   if (status == "expanded") { 
      // The ad has finished expanding 
    } 
  } 
```

### 5.5 函数`$sf.ext.expand`
`$sf.ext.expand(obj)`

可用性：异步（仅第一个请求被接受;另外的请求被拒绝，直到最初的请求被处理）

此方法扩展SafeFrame容器到指定的几何位置，允许媒介扩张。每个方向上的像素是相对于由init寄存器方法声明的原始偏移值的绝对位置。如果没有初始化方法就调用此方法，一个错误可能会被抛出，而且它会被忽略。扩展方法只能从初始大小调用，以保持性能。

SafeFrame中不支持补间，所以每当它需要扩大到其最大尺寸时，任何动画必须由第三方在容器内进行处理和调用此方法。

至少所述偏移参数之一是强制性的。如果所有的参数丢失，调用将被忽略，而且可能会抛出一个错误。在这个方法结束时，第三方注册执行的状态。如果SafeFrame的iframe是已经在最大尺寸，调用将被忽略。

**参数：**

 - `{Object}` **obj** 
 A descriptor object that defines the top, left, bottom, right coordinates for expansion. At minimum, 1 value must be specified. 
 - `{Number}` **obj.t** 
 The new top coordinate (y) relative to the current top coordinate. 
 - `{Number}` **obj.l** 
 The new left coordinate (x) relative to the current left coordinate. 
 - `{Number}` **obj.r**  
 The new right coordinate (x+width) relative to the current right coordinate (x+width). 
 - `{Number}` **obj.b** 
 The new bottom coordinate (y+height) relative to the current top coordinate (y+height). 
 - `{Boolean}` **obj.push**  
 Whether or not expansion should push the host content, rather than overlay. 

 > **实现注意事项** 
 > “推”的功能是一种在在第三方内容扩展方向（或多个）上“推动”主站内容的拓展功能。对于支持推动扩大功能的技术不是直接由SafeFrame1.0规定的。主站必须明确声明Push是否在`$ sf.host.posConfig`对象的`supports`属性中被允许。如果允许，主站必须能够在技术上支持该功能。
 > 

**返回：**

 - ***void***

**例子**
``` javascript
//Sample JavaScript implementation 
//Let's say that a 300x250 ad has been declared to fully expand to 400 
pixels to the left and 200 pixels to the top. 
 
var expansionPending = false; 
var expanded      = false; 
 
function expand() 
{ 
    var w = window, sf = w["$sf"], ext = sf && sf.ext; 
 
    if (ext) { 
      ext.expand({l:400,t:200}); 
    } else { 
      //api expansion not supported 
    } 
  } 
 
 function status_update_handler(status) 
  { 
  if (status == "expanded") { 
      // The ad has finished expanding 
} 
}
```

### 5.6 函数`$sf.ext.collapse`
`$sf.ext.collapse()`

可用性：异步（仅第一个请求被接受;另外的请求被拒绝，直到最初的请求被处理）

这种方法折叠SafeFrame容器到原来的几何位置。这个初始大小应已在在调用此方法之前的初始化注册方法中被声明。如果此方法没有伴随着初始化注册方法被调用，它可能会抛出一个错误，并且将被忽略。如果已经在初始大小了，调用将被忽略。

**返回**

 - `Void`

**例子**
``` javascript
//Sample JavaScript implementation 
 
function collapse() 
{ 
    var w = window, sf = w["$sf"], ext = sf && sf.ext; 
 
    if (ext) { 
      ext.collapse(); 
    } else { 
      //api expansion not supported 
    } 
  } 
 
 function status_update_handler(status) 
  { 
   if (status == "expanded") { 
      // Expanded 
    } else if (status == "collapsed") { 
      //we called collapse 
    } 
  }
```

### 5.7 函数`$sf.ext.status`
`$sf.ext.status()`

可用性：同步（可随时请求）

返回关于容器的当前状态的信息，例如，扩展命令是否正在等待处理等等。以下的是可被返回（更可能在后续版本中添加）的状态代码串的列表。一些字符串类似于在您调用`$ sf.ext.register`提供的的函数中收到的状态更新。

**返回**

 - `{String}` One of the following strings may be returned 
 **expanded** 
 Denotes that the container has been expanded. 
 **expanding** 
 Denotes that an expansion command is pending. 
 **collapsed** 
 Denotes that the container is in the default collapsed state. 
 **collapsing** 
 Denotes that a collapse command is pending. 

**相关章节**

 - 5.2 函数`$sf.ext.register`

### 5.8 函数`$sf.ext.meta`
`$sf.ext.meta(propName, ownerKey)`

用于检索关于由主站指定的SafeFrame的位置的元数据。主站可以指定有关第三方内容的其他元数据。主站指定使用`$sf.host.PosMeta`类此的元数据。

因为主机可能想使用一些这方面的数据为自己的目的，而不是与外方共享，第三方内容必须使用此功能来访问元数据信息。这样，第三方内容无法扫描任何主站不希望分享的值。

**参数**

 - `{String} propName` 
 The name of the metadata value you want to read 
 - `{String} ownerKey` *(Optional)*
 The name of the owner object from which to read the property. By default this value is "shared" meaning look in common data. 

**返回：**

 - `{String|Number|Boolean}`

**例子：1 -检索共享的元数据值**
``` javascript
//External Party JavaScript code (inside SafeFrame container) 
 
  var posID  = $sf.ext.meta("pos"); 
```

**例子：2 -检索非共享的元数据值**
``` javascript
//External Party JavaScript code (inside SafeFrame container) 
//"rmx" == owner of metadata blob, "sectionID" is key to retrieve 
 
  var sectionID  = $sf.ext.meta("sectionID", "rmx"); 
```

**相关章节**

 - 5.8 函数 `$sf.ext.meta`

### 5.9 函数`$sf.ext.cookie`
`$sf.ext.cookie(cookieName, cookieData)`

可用性：异步（读/写要求传递函数到`$sf.ext.register`）

将消息发送到主站以在主站域名读或写cookie。请注意，如果主站支持此功能，cookie数据是不能直接从该函数返回，因为它是异步的。你必须传递一个函数到`$sf.ext.register`，然后这将在Cookie数据设置或检索时被调用。

> **主站实现注意事项** 
> 允许一个第三方来读取或设置cookies，会带来某些安全页面，如登录页面，的安全风险。在允许之前，考虑允许cookie的读取或设置对于网页是否安全。
>

**参数**

 - `{String} cookieName`   
 The name of the cookie to set or read.   
 - `{Object} cookieData`  *(Optional)* 
 An object that contains the value, and potentially an expiration date, of a cookie to be set.  If not set, the Host assumes that External Party content is only interested in reading the Host cookie value. If set but no expiration date is given, the Host assumes that any cookie written to the Host domain is intended to remain indefinitely. 
 如果提供了以下参数: 
 　`{String} cookieData.info` *(Required)*   
 　A string value for the cookie. 
 　`{Date} cookieData.expires` *(Optional)*  
 　A date for when the cookie should expire. 

**例子1：读取一个主站cookie**
``` javascript
//Sample JavaScript implementation 
var w = window, sf = w[“$sf”], sfAPI = sf && sf.ext, myPubCookieName = 
“foo”, myPubCookieValue = “”, fetchingCookie = false; 
 
function register_content() 
{ 
  var e; 
  try { 
    if (sfAPI) sfAPI.register(300,250,status_update_handler); 
  } catch (e) { 
    //console.log(“no sfAPI -- > “ + e.message); 
       sfAPI = null; 
    } 
} 
 
function get_host_cookie() 
{ 
  var e; 
 
       try { 
      if (sfAPI && sfAPI.supports(“read-cookie”)) { 
fetchingCookie = sfAPI.cookie(“foo”); 
      } 
    } catch (e) { 
      fetchingCookie = false; 
    } 
} 
 
function status_update_handler(status, data) 
{ 
   if (status == "read-cookie") {  
    myPubCookieValue = data; 
    //now do whatever here since you have the cookie data 
  }  
} 
```

**例子2：写一个主站cookie**
``` javascript
//Sample JavaScript implementation 
var w = window, sf = w[“$sf”], sfAPI = sf && sf.ext, myPubCookieName = 
“foo”, myPubCookieValue = “”, settingCookie = false; 
 
function register_content() 
{ 
  var e; 
  try { 
    if (sfAPI) sfAPI.register(300,250,status_update_handler); 
  } catch (e) { 
    //console.log(“no sfAPI -- > “ + e.message); 
       sfAPI = null; 
    } 
} 
 
function set_host_cookie(newVal) 
{ 
  var e, cookieData = {value:newVal,expires:new Date(2020, 11, 1)}; 
 
       try { 
      if (sfAPI && sfAPI.supports(“write-cookie”)) { 
settingCookie = sfAPI.cookie(“foo”, cookieData); 
      } 
    } catch (e) { 
      settingCookie = false; 
    } 
} 
 
function status_update_handler(status, data) 
{ 
   if (status == "write-cookie") { 
    myPubCookieValue = data.info; 
    //now do whatever here since the write was successful 
  } else if (status == “failed” && data.cmd == “write-cookie”) { 
    //data.cmd contains original command sent 
       //data.reason contains a description of failure 
    //data.info contains the object of information sent to host 
    settingCookie = false; 
    //cookie not allowed to be set 
  } 
} 
```

### 5.10 函数`$sf.ext.inViewPercentage`
`$sf.ext.inViewPercentage()`

可用性：同步（可随时请求）

返回其中容器是在屏幕上视图内的区域的百分比，作为0到100之间的整数。

> **实现注意事项** 
> 在这个函数提供的信息在`$ sf.ext.geom`函数内是可用的，作为`self.iv`值返回。此附加函数被提供作为更方便访问该信息的便利。
>

**返回：**

 - `{Number}` The percentage of area that a container is in view on the screen 

**行业标准可广告见度**
业界公认的可见度指标可能需要报告的可见曝光的持续时间组件。持续时间可以通过计算`$ sf.ext.inViewPercentage`值多久达到或超过一个可见曝光的最小百分比来决定。

下面的代码示例演示了注册的"监听者"是怎样可能会确定持续时间（粗体值指被业界公认的广告能见度值代替）：
``` javascript
var viewableTimerId = 0; 
var viewableFired = false; 
 
function nodifyViewablePassed() 
{ 
if(viewableFired) return; // fire beacon 
viewableFired = true; 
viewableTimerId = 0; 
}  
 
function status_update(status, data)  
{  
// notify if 50% in view for 1 second  
if($sf.ext.inViewPercentage() > 50) 
{   
if(viewableTimerId == 0){ 
viewableTimerId = setTimeout(function() 
{notifyViewablePassed(); }, 1000);} 
}  
else{ 
clearTimeout(viewableTimerId); 
}  
} 
 
$sf.ext.register(160, 650, status_update) 
```

### 5.11 函数`$sf.ext.winHasFocus`
`$sf.ext.winHasFocus()`

可用性：同步（可随时请求）

返回是否浏览器窗口或包含SafeFrame聚焦(in focus)，或当前活动(active)的标签。

**返回：**

 - `{Boolean} True if the browser window / tab has focus, otherwise false`

**版本要求**

 - “1.1” Requires specVersion 1.1 as opposed to original functionality in “1.0”

**与广告可见度的关系**
除了几何坐标，一个SafeFrame内的内容可能想知道主窗口是当前活动，或聚焦。该函数提供了信息，而且在报告可见度指标时可以被考虑。

> **广告可见度注** 
> `winHasFocus`函数提供了可被认为是可见度指标的部分的信息。这个函数报告的信息并不能决定或报告可见度。可见度指标是由行业和参与报告可见度的媒体的各方确定。
>

下面的代码示例演示了一个注册的监听器会如何确定主浏览器窗口或选项卡是否聚焦。
``` javascript
var win_has_focus = false; 
 
function status_update(status, data) 
{ 
// notify if 50% in view for 1 second 
if(status == “focus-change”) { 
    win_has_focus = $sf.ext.winHasFocus(); 
} 
} 
 
$sf.ext.register(160, 650, status_update) 
```


----------
end