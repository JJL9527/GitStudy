# Git 零基础教程：管理文章与图文创作

> 面向完全没有 Git 经验的创作者。目标是：安全保存版本、查看修改、找回旧稿，并建立适合文章和图片项目的工作流。

## 1. Git 是什么

Git 是文件版本管理工具，像创作项目的“时光机”。它会记录每次保存的变化，让你能比较不同版本、恢复旧稿、尝试不同方向。

- Git 管版本变化；网盘主要负责同步和分享。
- 一个项目建议对应一个主题，例如一篇系列文章或一个图文专题。

## 2. 安装与首次设置

安装后在终端检查：

```bash
git --version
```

设置提交记录中的署名：

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

查看设置：

```bash
git config --global --list
```

## 3. 终端最少必学命令

| 命令 | 作用 |
|---|---|
| `pwd` | 查看当前文件夹 |
| `ls`（Windows 用 `dir`） | 查看文件 |
| `cd 文件夹` | 进入文件夹 |
| `cd ..` | 返回上一级 |
| `mkdir 名称` | 新建文件夹 |

路径有空格时加引号：`cd "我的文章"`。

## 4. 建立第一个创作项目

推荐结构：

```text
我的文章项目/
├── drafts/       # 草稿
├── images/       # 图片
├── exports/      # 发布成品
├── notes/        # 资料与灵感
└── README.md     # 项目说明
```

macOS/Linux：

```bash
mkdir -p my-article/{drafts,images,exports,notes}
cd my-article
echo "# 我的文章项目" > README.md
git init
```

Windows PowerShell：

```powershell
mkdir my-article
cd my-article
mkdir drafts, images, exports, notes
"# 我的文章项目" | Out-File README.md -Encoding utf8
git init
```

## 5. Git 的核心流程

```text
修改文件 → git add → git commit
工作区      暂存区      已保存版本
```

创建文章并第一次保存：

```bash
echo "# 我的第一篇文章" > drafts/article.md
git status
git add drafts/article.md
git commit -m "创建文章初稿"
```

提交说明要写清楚，例如“补充开头案例”“调整封面图片”“修正错别字”。确认全部改动都要保存时可以用：

```bash
git add .
git commit -m "保存本次创作进度"
```

## 6. 每天创作的标准动作

```bash
cd 你的项目路径
git status
# 写文章、改图片
git diff
git add .
git commit -m "完成第二版内容"
git log --oneline -5
```

适合提交的时机：完成一个小节、完成一轮结构调整、换好一组图片、完成发布版，或准备尝试新方向之前。

## 7. 查看修改与历史

```bash
git status                 # 当前状态
git diff                   # 尚未暂存的修改
git log --oneline          # 提交历史
git show 提交编号           # 某次提交的内容
git log --oneline -- drafts/article.md  # 某文件历史
```

## 8. 撤销错误修改

### 只改了文件，还没 `git add`

```bash
git restore drafts/article.md
```

这会丢弃未提交内容，使用前先确认。

### 已 `git add`，还没提交

```bash
git restore --staged drafts/article.md
```

文件内容会保留，只是移出暂存区。

### 已经提交，想看旧版本

```bash
git log --oneline
git show 提交编号:drafts/article.md
```

初学者不要随意使用 `git reset --hard`，它可能删除未保存的修改；回退前先复制项目做备份。

## 9. 用分支做平行试验

分支适合尝试另一种标题、封面或文章结构：

```bash
git switch -c try-new-cover
# 修改文件
git add .
git commit -m "尝试新标题与封面"
git switch main
git merge try-new-cover
```

查看分支：`git branch`。如果实验不要了：`git branch -d try-new-cover`。如果默认分支叫 `master`，把 `main` 换成 `master`。

## 10. 文章与图片项目的规则

### 文件命名

推荐英文、数字和短横线：

```text
2026-09-26-article-draft.md
cover-v2.jpg
diagram-01.png
```

### `.gitignore`

在项目根目录新建 `.gitignore`，排除临时文件：

```gitignore
.DS_Store
Thumbs.db
*.tmp
*.bak
.cache/
exports/temp/
```

如果文件已经被 Git 跟踪，后来加入忽略规则，需要：

```bash
git rm --cached 文件名
git commit -m "忽略临时文件"
```

小型封面、信息图适合放进 Git；大量 RAW 原片、视频和重复导出图建议放网盘、NAS 或 Git LFS，并在项目中留下说明。

## 11. 远程备份（熟悉本地 Git 后再学）

可以使用 GitHub、GitLab 或自建服务。创建远程空仓库后：

```bash
git remote add origin 远程仓库地址
git branch -M main
git push -u origin main
```

以后同步：

```bash
git push
git pull
```

公开仓库不要提交密码、API 密钥、身份证、客户资料或未获授权的原图。个人创作项目可设为 Private。

## 12. 常见问题

### `nothing to commit`

表示没有新的变化需要提交，属于正常结果。

### `git add .` 加错了

```bash
git restore --staged 文件名
```

### 合并冲突

```bash
git status
```

打开冲突文件，找到 `<<<<<<<`、`=======`、`>>>>>>>`，编辑成最终内容并删除这些标记，然后：

```bash
git add 冲突文件
git commit -m "解决内容冲突"
```

## 13. 7 天练习计划

1. 安装 Git，设置用户名和邮箱。
2. 建立文章项目并完成第一次 `add + commit`。
3. 修改文章，用 `git diff` 和 `git log` 查看变化。
4. 练习 `git restore` 撤销未提交修改。
5. 整理 `drafts/images/exports/notes`，添加 `.gitignore`。
6. 创建一个封面实验分支，再合并或删除它。
7. 用真实选题连续提交 3～5 个阶段版本。

## 14. 最值得记住的命令

```bash
git init
git status
git add 文件名
git add .
git commit -m "说明"
git log --oneline
git diff
git restore 文件名
git switch -c 分支名
git push
```

只要掌握“查看状态 → 添加修改 → 提交版本 → 查看历史”这条主线，就已经足够管理日常文章和图文创作。远程仓库、Git LFS 和图形化客户端可以在此基础上逐步学习。
