---
layout: post
title: "一周格致随想（第7期）"
description: "一周格致随想（第7期）"
author: "魏仁言"
date:  2021-11-07 20:11:02 +0800
categories: life notes
toc: true
---

> 思不出其位。非其位不谋其政，非其事不虑其计。

## 1.蚂蚁旋涡
在自然界中存在着非常罕见的怪现象，**“蚂蚁死亡漩涡”（ant mill）**，由没有视力的行军蚁组成，多由一只“领头蚁”带领，它会分泌踪迹费洛蒙（trail pheromone），让其他行军蚁用嗅觉跟随；一但领头蚁失去方向，导致踪迹费洛蒙出现混乱，就会令整团行军蚁迷路，甚至不断原地绕圈的循环，形成死循环陷入**“死亡漩涡”**，持续转圈最后因体力耗尽而死。

蚂蚁旋涡现像，不仅在自然界中存在，同时日常生活中也频频出现。其阐释了在方向目标不明确坚定时，没有外力增长牵引情况下，我们日常活动也会处于原地踏步不停地绕圈循环，增长停滞陷入死循环漩涡。

复杂系统，告诉我们面对这种情况，依据开放系统的耗散结构特性，需要从不断地从外界取得能量和负熵(把熵给予外界)，才得以维持和发展，一旦与外界的交流断绝了，便趋于停滞和死亡，最后变为无序的废墟。

因此不仅要低头拉车，也要仰望星空，抬头看路。

## 2.认识业务
这个需求的业务逻辑没有确认，存在卡点产品方案没有办法完成；这个产品功能的逻辑不明确，系统设计不确定没有办法进行开发；这些业务问题没有解决影响到项目排期，导致项目最终交付延期等等。我们工作中是不是经常遇到这个景象呢？

那到底什么是业务？什么是业务问题？

举例介绍，最近做一个需求案例，用户在下单时现有支付方式（PayType1）不满足业务需求，需增加一种新的支付方式（PayType2），需要A系统进行改造支持，完成用户支付并记录实际支付金额。A系统功能原字段aPayAmount 记录原支付方式（PayType1）的实付金额，其给出的方案尽量减少影响范围，新增bPayAmount字段记录新支付（PayType2）的金额。产品方案沟通过程中，双方因A系统表示不能实现用户实付功能支持，出现业务认识偏差，争执不下。

一切脱离于用户的业务的架构设计都是耍流氓。

用户新增支付方式的业务场景中，主要是实现解决用户多种实付金额业务问题。无论何种支付方式（PayType1和PayType2），系统产品功能中aPayAmount实付金额和bPayAmount实付金额，表示的业务意义都是用户实付金额。

业务，具体一点就是人们客观现实中一系列活动操作，解决具体问题，并创造相应的价值。

业务是人的业务，产品是人的产品，系统是人的系统。其首要是服务于人，实现效能生产力的提升。

因此架构的前提是对业务的认知，产品的认知，系统的认知，三观认知必须一致，否则架构设计的产出物就不符合具体客观实际，满足业务的发展。更具体点就是架构是规划设计出来的，其演进升级过程是业务认知升级的过程。


> 非常抱歉，这大半年一直荒废，没有新的产出。同时最近忙于一个复杂更大的项目，对业务的理解感悟更进一步，今天先简单写一点，后续梳理完成再详细写出来。

九思堂/格致录/一周格致随想