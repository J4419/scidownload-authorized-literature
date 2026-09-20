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
```
其中：

SKILL.md：Agent 的主要操作规则
scripts/：SCIDownload 实际执行脚本
references/：故障排查、多出版商、断点续传和维护说明
环境要求
Python 3.10 或更高版本
Chrome、Edge 或 Chromium
对目标文献具有合法访问权限

脚本主要使用 Python 标准库，不要求额外安装复杂依赖。

基本工作流程

Agent 通常会：

检查输入中是否已经包含 DOI
如果只有标题，先使用 FindDOI.py 查询 DOI
对少量文献进行试跑
确认访问权限和 PDF 是否能够正常打开
再进行较大的批量任务
根据 manifest.jsonl 和磁盘文件进行断点续传

对于登录、网络、出版商兼容性或下载失败等情况，会按 references/ 中的说明进一步诊断。

隐私与安全

浏览器 Profile 可能包含：

Cookie
机构登录状态
会话数据

因此不要上传或公开：

browser-profile
Cookie
登录数据库
run.log
manifest.jsonl
包含个人研究兴趣的 DOI 清单
带有机构信息或本地路径的调试截图

CDP 调试接口仅应绑定到本机 127.0.0.1。

相关项目

SCIDownload 主项目：

https://github.com/J4419/SCIDownload

License

请参照 SCIDownload 主项目及本仓库所采用的许可证。

我建议就用这版，不要把 README 写得太长。真正给 Agent 执行的细节已经在 `SKILL.md` 和 `references/` 里，GitHub 首页主要是让人**一眼看懂这是干什么的、有什么边界、怎么组成**。:contentReference[oaicite:1]{index=1}
