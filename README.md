# NightFall 友链仓库

本仓库是 [NightFall-Blog](https://github.com/NightNight-30/NightFall-Blog) 的动态友链数据源。每个 GitHub Issue 记录一条友链，GitHub Actions 自动抓取生成 `data.json` 供博客前端调用。

## 工作原理

参考 [xaoxuu 博客 2025-06-02](https://xaoxuu.com/blog/20250602/) 的动态友链方案：

1. 申请人在 Issues 提交友链（用「友链申请」模板，包含一段 JSON）
2. 仓库 owner 审核通过后去掉 `审核中` 标签
3. GitHub Actions（`xaoxuu/links-checker` + `xaoxuu/issues2json`）自动：
   - 检查每条友链的可达性，挂掉打 `失联` 标签
   - 把每条 Issue 的 JSON 合并成 `/v2/data.json`，push 到 `output` 分支
4. 每天凌晨 5 点（UTC+8）`feed-posts-parser` 自动抓每个友链 RSS 的最新 3 篇文章
5. 博客前端用 stellar `{% friends posts:true api:... %}` 拉取 `data.json` 渲染卡片 + 最新文章

## 想申请友链？

[点这里提 Issue](../../issues/new?assignees=&labels=%E5%AE%A1%E6%A0%B8%E4%B8%AD&projects=&template=friend-link.yml&title=友链申请：) ，按模板填好等审核就行。

## 排序规则

`data.json` 默认按 `posts-desc`（最新文章时间降序）排序，没有文章的链接自动靠后。

## 排除标签

带这些标签的 Issue 不会进入 `data.json`：
- `审核中` 未通过审核
- `缺少互动` 长期无互动
- `缺少文章` 长期无新文章
- `风险网站` 有木马/违法等风险

`白名单` 标签的链接会进入 data.json 但不显示该标签本身。
