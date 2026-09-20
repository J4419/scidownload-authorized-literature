# SCIDownload Authorized Literature Skill

这是一个用于 **Agent 的学术文献获取 Skill**，基于 [SCIDownload](https://github.com/J4419/SCIDownload)。

它可以帮助 Agent 根据 DOI 或文献标题，在用户**已有合法访问权限**的前提下，调用本地浏览器和配套脚本完成文献获取、DOI 查询、断点续传以及常见访问问题诊断。

## 功能

- 根据 DOI 清单批量获取论文正文 PDF
- 可选下载 Supplementary Information（SI，补充材料）
- 根据论文标题查询 DOI
- 支持中断后继续运行，自动跳过已经完整下载的文献
- 辅助诊断校园网、代理、VPN、CARSI、EZproxy 等访问环境
- 支持 ScienceDirect 及部分其他出版商
- 对 PDF 和补充材料进行完整性检查
- 使用独立浏览器 Profile 保存机构登录状态

## 使用边界

本 Skill **只自动化用户本身已经拥有的访问权限**，例如：

- 开放获取（Open Access）文章
- 学校或科研机构已订阅的资源
- 已授权的校园网访问
- CARSI、EZproxy、学校 VPN 或机构登录会话

本 Skill：

- 不提供机构订阅
- 不绕过付费墙
- 不绕过访问控制
- 不破解账号或登录系统
- 不能让原本无权限访问的文章变成可下载

使用时请遵守出版商协议、学校图书馆政策及适用法律。

## 目录结构

```text
scidownload-authorized-literature/
├─ SKILL.md
├─ scripts/
│  ├─ SCIDownload.py
│  ├─ SCIDownload_cdp.py
│  ├─ FindDOI.py
│  ├─ ClearCache.py
│  └─ check_exit_ip.py
└─ references/
   ├─ troubleshooting.md
   ├─ manifest-and-si.md
   ├─ multi-publisher.md
   ├─ implementation-notes.md
   └─ release-maintenance.md
