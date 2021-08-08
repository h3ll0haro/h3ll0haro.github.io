---
layout: post
title: 目录扫描
categories: web安全
tags: 信息收集

---



## dirsearch使用

dirsearch作为一款优秀的目录扫描工具，可自定义字典、后缀，并可根据目标情况排除返回状态码，返回大小等，帮助渗透测试者快速找到网站的敏感信息。安装地址：`git clone https://github.com/maurosoria/dirsearch.git`

### 命令参数

| 参数                      | 描述                                                         | 示例                                       |
| ------------------------- | ------------------------------------------------------------ | ------------------------------------------ |
| --version                 | 显示程序版本号                                               |                                            |
| -h,--help                 | 显示帮助消息                                                 |                                            |
| -u,--url                  | 目标URL                                                      | -u https://www.baidu.com                   |
| -l,--url-list             | URL列表                                                      | --url-list domain.txt                      |
| --stdin                   | 来自STDIN的URL列表                                           |                                            |
| --cidr                    | CIDR目标                                                     |                                            |
| --raw                     | 原始请求包含的文件（使用--scheme标志）                       |                                            |
| -e,--extensions           | 扩展名，跨站名列表用逗号分割                                 | -e php,asp                                 |
| -x,--exclude-extensions   | 排除拓展名,以逗号分隔的排除扩展名列表                        | -x php, asp                                |
| -f,--force-extensions     | 将扩展名添加到每个单词列表条目的末尾。将扩展名添加到每个单词列表条目的末尾。默认情况下，dirsearch仅将％EXT％关键字替换为扩展名 | -f asp,php                                 |
| -w,--wordlists            | 自定义单词列表                                               | -w sub.txt                                 |
| --prefixes                | 向所有条目添加自定义的前缀（以逗号分隔）                     | --prefixes /admin/                         |
| --suffixes                | 向所有条目添加自定义后缀，忽略目录(以逗号分隔)               | --suffixes  .bak                           |
| --only-selected           | 仅具有选定扩展名或不具有扩展名的条目                         |                                            |
| --remove-extensions       | 删除所有单词列表条目中的扩展名                               | --remove-extensions php(admin.php-> admin) |
| -U,--uppercase            | 大写字典                                                     |                                            |
| -L, --lowercase           | 小写字点                                                     |                                            |
| -C, --capital             | 首字母大写字典                                               |                                            |
| -r, --recursive           | 递归                                                         |                                            |
| -R, --recursion-depth     | 深度递归                                                     |                                            |
| -t,--threads              | 线程                                                         | -t 10                                      |
| --subdirs                 | 扫描给定URL的子目录（以逗号分隔）                            |                                            |
| --exclude-subdirs         | 递归扫描期间排除以下子目录（以逗号分隔）                     |                                            |
| -i,--include-status       | 状态代码，以逗号分隔，支持范围（例如：200,300-399）          |                                            |
| -x,--exclude-status       | 排除状态代码，以逗号分隔，支持范围（例如：301,500-599）      |                                            |
| --exclude-sizes           | 按大小排除响应，以逗号分隔（范例：123B，4KB）                |                                            |
| --exclude-texts           | 排除文字回应，并以逗号分隔（例如："Not found"，"error"）     |                                            |
| --exclude-regexps         | 用正则表达式排除响应，并用逗号分隔(Example: 'Not foun[a-z]{1}', '^Error$') |                                            |
| --exclude-redirects       | 通过重定向正则表达式或文本（以逗号分隔）排除响应（例如："https://okta.com/*"） |                                            |
| --calibration             | 测试校准的路径                                               |                                            |
| --random-agent            | 为每个请求选择一个随机的User-Agent                           |                                            |
| --minimal                 | 最小响应长度                                                 |                                            |
| --maximal                 | 最大响应长度                                                 |                                            |
| -q, --quiet-mode          | 安静模式                                                     |                                            |
| --full-url                | 在输出中打印完整的URL                                        |                                            |
| --no-color                | 无颜色输出                                                   |                                            |
| -m, --http-method         | 指定method方法                                               | -m post（默认get）                         |
| -d,--data                 | 请求包数据                                                   |                                            |
| -H,--header               | http请求头                                                   | -H 'Referer: example.com' -H 'Accept: */*' |
| --header-list             | 请求头文件                                                   | --header-list hearder.txt                  |
| -F, --follow-redirects    | 跟随重定向                                                   |                                            |
| --user-agent              | 自定义user-agent                                             |                                            |
| --cookie                  | 自定义cookie                                                 |                                            |
| --timeout                 | 连接超时                                                     |                                            |
| --ip                      | 服务器IP地址                                                 |                                            |
| -s,--delay                | 请求之间的延迟                                               |                                            |
| --proxy                   | 代理URL，支持http和socks代理                                 |                                            |
| --proxy-list              | 代理服务器列表                                               |                                            |
| --matches-proxy           | 用找到的路径重播的代理                                       |                                            |
| --scheme=SCHEME           | 默认方案（用于原始请求或URL中没有任何方案）                  |                                            |
| --max-retries             | 重试次数                                                     |                                            |
| -b, --request-by-hostname | 默认情况下，dirsearch通过IP请求速度。这将通过主机名强制请求  |                                            |
| --exit-on-error           | 发生错误时退出                                               |                                            |
| --debug                   | 调试模式                                                     |                                            |
| --simple-report           | 输出简单报告                                                 |                                            |
| --plain-text-report       | 输出文本报告                                                 |                                            |
| --json-report             | 输出json报告                                                 |                                            |
| --xml-report              | 输出xml报告                                                  |                                            |
| --markdown-report         | 输出markdown报告                                             |                                            |
| --csv-report              | 输出csv报告                                                  |                                            |

### 常用命令

```bash
# 指定后缀扫描
python3 dirsearch.py --url http://x.x.x.x/ -e txt,html,asp,aspx

# 排除状态码及大小
python3 dirsearch.py --url http://x.x.x.x/ -e txt,html,asp,aspx -x 401 --exclude-sizes 1000

# 扫描多个域名
python3 dirsearch.py -l domain.txt --plain-text-report result.txt
```

