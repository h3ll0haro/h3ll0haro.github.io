---
layout: post
title: 情报分析-TheHarvester
categories: web安全
tags: 信息收集
---

## TheHarvester使用

`TheHarvester`能够收集电子邮件账号、用户名、主机名和子域名等信息。它通过Google、Bing、PGP、LinkedIn、Baidu、Yandex、People123、Jigsaw、Shodan等公开资源整理收集这些信息。安装地址：`https://github.com/laramies/theHarvester`

### 0x01 命令参数

| 参数                                   | 描述                                                         |
| -------------------------------------- | ------------------------------------------------------------ |
| -h,--help                              | 显示此帮助信息并退出                                         |
| -d DOMAIN,--domain DOMAIN              | 要搜索的公司名称或域名                                       |
| -l LIMIT, --limit LIMIT                | 限制搜索结果的数量，默认=500                                 |
| -S START, --start START                | 从结果号 X 开始，默认值 = 0                                  |
| -g, --google-dork                      | 使用 Google Dorks 进行 Google 搜索                           |
| -p, --port-scan                        | 扫描检测到的主机并检查接管 (21,22,80,443,8080)               |
| -s, --shodan                           | 使用 Shodan 查询发现的主机                                   |
| -v, --virtual-host                     | 通过 DNS 解析验证主机名并搜索虚拟主机                        |
| -e DNS_SERVER, --dns-server DNS_SERVER | 用于查找的 DNS 服务器                                        |
| -t DNS_TLD, --dns-tld DNS_TLD          | 启用 DNS 服务器查找，默认为 False                            |
| -n, --dns-lookup                       | 启用 DNS 服务器查找，默认为 False                            |
| -c, --dns-brute                        | 对域执行 DNS 爆破                                            |
| -f FILENAME, --filename FILENAME       | 将结果保存到 HTML 和/或 XML 文件                             |
| -b SOURCE, --source SOURCE             | baidu、bing、bingapi、certspotter、crtsh、dnsdumpster、dogpile、duckduckgo、github-code、google、hunter、intelx、linkedin、linkedin_links、netcraft、otx、securityTrails、spyse（暂时禁用）、threatcrowd、trello、twitter、vhost ,virustotal, yahoo, all |

### 0x02 常用命令

```bash
# 通过百度查找域名xxx.com的相关信息
theHarvester -d xxx.com -l 500 -b baidu
```

