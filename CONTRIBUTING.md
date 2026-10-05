# 使用与贡献指南

本仓库是 JNU-CST 指南的 GitBook 内容源。本文只说明如何在本地构建网站、如何修改内容，以及提交修改时需要注意的少量 Git 事项。

## 1. 构建与预览

### 1.1 环境要求

请先安装：

- Node.js LTS；
- pnpm。

确认命令可以正常使用：

```bash
node --version
pnpm --version
```

如果终端提示找不到命令，请先重新打开终端或编辑器，使最新的环境变量生效。

### 1.2 安装依赖

在仓库根目录执行：

```bash
pnpm install --frozen-lockfile
```

依赖版本由 `pnpm-lock.yaml` 锁定。除非需要升级构建工具，否则不要随意更新锁文件。

### 1.3 本地预览

启动 HonKit 开发服务器：

```bash
pnpm dev
```

然后访问：

```text
http://localhost:4000
```

修改 Markdown 文件后，页面会自动重新构建。按 `Ctrl+C` 可以停止服务。

### 1.4 生成静态网站

执行：

```bash
pnpm build
```

生成的网站位于 `_book/`。该目录是本地构建产物，已经被 Git 忽略，不应提交到仓库。

提交修改前，应至少确认 `pnpm build` 能够成功完成。

## 2. 修改仓库内容

### 2.1 目录作用

| 路径 | 作用 |
| --- | --- |
| `README.md` | 指南首页 |
| `SUMMARY.md` | GitBook 导航目录及页面顺序 |
| `intro/` | 专业介绍、入学和设备准备 |
| `survival/` | 军训、社团和校园生活 |
| `study/` | 学习建议及各年级课程评价 |
| `tail/` | 其他补充内容 |
| `.gitbook/assets/` | 图片等静态资源 |

### 2.2 修改已有页面

找到对应的 `.md` 文件后直接编辑。页面通常采用以下结构：

```markdown
---
description: 页面的一句话简介
---

# 页面标题

正文内容。

## 小节标题

小节内容。
```

写作时请遵循以下规则：

- 一个页面只使用一个一级标题 `#`；
- 正文使用 `##`、`###` 组织层次；
- 保留已有页面的 YAML 头部；
- 尽量延续原有语言和排版风格；
- 保证内容真实、清晰，不加入个人隐私或未经授权的材料；
- 课程、教师和考核方式等易变化的信息，应注明适用年级或更新时间。

### 2.3 新增页面

1. 根据内容类型，在 `intro/`、`survival/`、`study/` 或 `tail/` 中创建 Markdown 文件。
2. 文件名使用简短的小写英文或拼音，单词之间使用短横线，例如 `bi-ye-she-ji.md`。
3. 添加 YAML 头部、一级标题和正文。
4. 在 `SUMMARY.md` 中添加页面链接。

示例：

```markdown
* [一级页面](path/README.md)
  * [新增页面](path/new-page.md)
```

`SUMMARY.md` 中的缩进决定页面的导航层级。仅创建 Markdown 文件而不更新 `SUMMARY.md`，页面通常不会出现在网站导航中。

### 2.4 调整或删除页面

移动、重命名或删除页面时，需要同时更新：

- `SUMMARY.md` 中的对应路径；
- 其他 Markdown 文件中指向该页面的链接；
- 页面引用图片时使用的相对路径。

可以使用以下命令搜索旧路径或文件名：

```bash
rg "旧文件名或路径" .
```

### 2.5 添加图片

图片统一放入 `.gitbook/assets/`，文件名建议使用英文、数字和短横线。

在页面中使用相对路径引用，例如位于二级目录中的页面可以写为：

```markdown
![图片说明](../../.gitbook/assets/example.png)
```

需要控制图片宽度或添加图注时，可以沿用仓库中已有的 `<figure>` 格式：

```html
<figure>
  <img src="../../.gitbook/assets/example.png" alt="图片说明" width="563">
  <figcaption><p>图片说明</p></figcaption>
</figure>
```

移动页面后应重新检查相对路径；删除图片前应确认没有其他页面引用。

### 2.6 修改后的检查

完成修改后依次检查：

1. 新页面是否已加入 `SUMMARY.md`；
2. 导航层级和页面顺序是否正确；
3. 内部链接与图片是否能够打开；
4. YAML 头部和 Markdown 标题层级是否正确；
5. 是否误加入临时文件、个人信息或大体积无关文件；
6. `pnpm build` 是否成功。

## 3. Git 使用建议

建议在独立分支中修改，避免直接影响稳定内容：

```bash
git switch -c docs/修改主题
```

提交前查看本次变更：

```bash
git status
git diff --check
git diff
```

提交信息应简短说明修改目的，例如：

```bash
git add 修改过的文件
git commit -m "docs: update course guide"
```

一次提交尽量只处理一个主题。推送和合并前，应先完成本地构建并确认页面显示正常。
