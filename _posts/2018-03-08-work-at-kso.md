---
layout: post
title: 在金山WPS珠海工作是怎样的体验
date: 2018-03-08
banner_image: kso_office.jpg
tags: [Work, WPS]
---

说来在金山WPS从事企业云后端开发也有三个月了，经历了公司从吉大搬家到唐家湾，见证了WPS第三十个年头的年会，借用一下知乎体，在这里写写我眼中的珠海WPS以及企业云后端开发。

<!--more-->

#### 关于珠海及珠海IT行业
珠海是一座小而美的城市。记得拿Offer后来珠海考察，顺便去拜访一下公司办公室，那是一个工作日的周一，中午完事儿之后从公司出来，骑着摩拜在附近闲逛，街道上居然没有什么人，除了一些穿着校服的中学生，很少能看到上班族。要知道，当时金山所在的区域是所谓的“珠海CBD”, 基本上是珠海最繁华的地段。街道不宽，两边绿树成荫鲜花繁盛，地面很干净，天空通透，空气新鲜舒适，时不时有桂花香气扑来，那种感觉很容易让人不自觉地放松。大概这就是珠海给我的最初印象。

在珠海，IT公司相对于北上广深杭等一线少很多，但比其他二线是不差的，比较有名的IT公司就是金山、魅族、YY了，还有一些垂直行业比较大的公司，比如做电力行业的远光软件，还有一些创业公司比如小源、心游、优特、佳米等等。在高新区唐家湾，有个南方软件园，里面也有一些IT公司。总体来说，IT业在珠海还是比较受到政府鼓励发展的，也有越来越多的创业公司选择来珠海。

#### 关于金山和WPS云办公
金山软件在珠海有三个子公司：金山办公、西山居、猎豹，三个公司独立运营，共享集团公司资源。我所在的金山办公软件有限公司，主要产品就是WPS, 有着30年历史的骨灰级国产办公软件，高开低走，甚是曲折，最近一些年借着移动端发力以及云计算的东风，又火起来了，去年已经在A股提交IPO, 大概率今年要上市。

WPS云办公分为个人云和企业云，主要是给用户提供一站式在线文档管理平台，随时随地的文档查看、协同编辑、存储等。我的理解就是一个文档领域的SaaS服务，类似于Google Doc. 国内也有类似SaaS服务，比如石墨文档和一起写等。

我所在的WPS云服务后端开发，主要采用Golang语言，有些服务也用Java或C++, 但主要还是Golang. 整个后端采用微服务架构，Docker部署，数据存储有MySQL, Redis, ElasticSearch等，文档存储对接金山云存储，或者是企业用户私有的存储。随着业务发展，用户量飙升，后端架构也在不断调整重构，还是有比较多的技术挑战。

#### 关于企业文化和福利
中午休息两小时，12点到2点。刚来上班的时候惊呆了，1点钟办公室准时熄灯，有人从座位底下拉出折叠床直接盖被睡觉！不得不说，这一点还是很好的，因为我离家比较近，中午经常回家睡一觉，有点儿回到80年代上班族的感觉。

食堂必须给赞，一日三餐免费自助。师傅变着法做各种新菜式，海边餐厅就餐环境一流，想象一下，一帮同事围坐有说有笑，然后抬头就是一片蓝色海域，就像住在海边酒店享受自助餐一样。哈哈，有点儿夸张，但作为一个服务两三千人的员工食堂，能做到这个份上真的已经国内罕见了，最重要的是免费哦。
{% include image_caption.html imageurl="/images/posts/kso_breakfast.jpg" title="Breakfast" caption="今天在食堂吃的早餐" %}

公司年轻人居多，氛围比较轻松活跃，各种活动多姿多彩；公司运转效率也比较高，有什么问题反馈渠道畅通，整改也很快，大企业病不严重哈；上班不打卡，但一般要求工作时间8个小时，比较忙的时候会加班，不是很夸张，我来的三个月加班时间平均下来一周一天左右吧。

#### 内推

说了那么多，重点来了，感兴趣的朋友们简历砸过来：<archer.wq@gmail.com>，标题记得注明职位哦，职位列表可以参考这里：<http://www.wps.cn/jobs/>






