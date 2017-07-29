---
layout: post
title:  Nutch Map/Reduce
category: jekyll
description: The process in Nutch using Map/Reduce.
---

<br />

# Process
1. 插入url列表到Crawl DB，引导下面的抓取程序
2. 循环:
- 从Crawl DB生成一些url列表;
- 抓取内容;
- 分析处理抓取的内容;
- 更新Crawl DB库.
3. 转化每个页面中外部对它的链接
4. 建立索引

<br />

## Ⅰ.Inject(插入url列表)
1. MapReduce程序1: 
- 目标:转换input输入为CrawlDatum格式. 
- 输入: url文件 
- Map(line) → <url, CrawlDatum> 
- Reduce()合并多重的Url.
- 输出:临时的CrawlDatum文件. 
2. MapReduce2: 
- 目标:合并上一步产生的临时文件到新的DB 
- 输入: 上次MapReduce输出的CrawlDatum 
- Map()过滤重复的url. 
- Reduce: 合并两个CrawlDatum到一个新的DB 
- 输出:CrawlDatum
<br /><br />

## Ⅱ.Generate(生成抓取列表)
1. MapReduce程序1: 
- 目标:选择抓取列表 
- 输入: Crawl DB 文件 
- Map() → 如果抓取当前时间大于现在时间 ,抓换成 <CrawlDatum, url>格式. 
- 分发器(Partition) :用url的host保证同一个站点分发到同一个Reduce程序上. 
- Reduce:取最顶部的N个链接. 
2. MapReduce程序2: 
- 目标:准备抓取 
- Map() 抓换成 <url,CrawlDatum,>格式 
- 分发器(Partition) :用url的host 
- 输出:<url,CrawlDatum>文件
<br /><br />

## Ⅲ.Fetch(抓取内容)
1. MapReduce: 
- 目标:抓取内容 
- 输入: <url,CrawlDatum>, 按host划分, 按hash排序 
- Map(url,CrawlDatum) → 输出<url, FetcherOutput> 
- 多线程, 调用Nutch的抓取协议插件,抓取输出<CrawlDatum, Content> 
- 输出: <url,CrawlDatum>, <url,Content>两个文件
<br /><br />

## Ⅳ.Parse(分析处理内容)
1. MapReduce: 
- 目标:处理抓取的能容 
- 输入: 抓取的<url, Content> 
- Map(url, Content) → <url, Parse> 
- 调用Nutch的解析插件,输出处理完的格式是<ParseText, ParseData> 
- 输出: <url,ParseText>, <url,ParseData><url,CrawlDatum>.
<br /><br />

## Ⅴ.Update(更新Crawl DB库)
1. MapReduce: 
- 目标: 整合 fetch和parse到DB中 
- 输入:<url,CrawlDatum> 现有的db加上fetch和parse的输出,合并上面3个DB为一个新的DB 
- 输出: 新的抓取DB
<br /><br />

## Ⅵ.Invert Links(转化链接)
1. MapReduce: 
- 目标:统计外部页面对本页面链接 
- 输入: <url,ParseData>, 包含页面往外的链接 
- Map(srcUrl, ParseData> → <destUrl, Inlinks> 
- 搜集外部对本页面的链接Inlinks格式:<srcUrl, anchorText> 
- Reduce() 添加inlinks 
- 输出: <url, Inlinks>
<br /><br />

## Ⅶ.Index(建立索引)
1. MapReduce:
- 目标:生成Lucene索引
- 输入: 多种文件格式
- parse处理完的<url, ParseData> 提取title, metadata信息等
- parse处理完的<url, ParseText> 提取text内容
- 转换链接处理完的<url, Inlinks> 提取anchors
- 抓取内容处理完的<url, CrawlDatum> 提取抓取时间
- Map() 用ObjectWritable包裹上面的内容
- Reduce() 调用Nutch的索引插件,生成Lucene Document文档
- 输出: 输出Lucene索引
<br /><br />