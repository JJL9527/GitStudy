# Git + Codex 游戏开发入门指南

> 面向没有编程经验的初学者。你可以把 Git 理解成“给游戏项目拍快照、保存历史、随时回到过去”的工具；把 Codex 理解成“在这个项目文件夹里协助你规划、写代码、修改文件和检查问题的开发伙伴”。

## 你最后要形成的工作习惯

每次让 Codex 改代码，都按下面的顺序做：

1. 先确认当前项目状态。
2. 为新功能建立一个独立分支。
3. 用清楚的中文告诉 Codex：目标、相关文件、限制条件、完成标准。
4. 让 Codex 修改后查看差异，并运行游戏或测试。
5. 确认没有问题后提交一次 Git 记录。
6. 需要备份或协作时，再推送到 GitHub 等远程仓库。

最重要的原则是：**小步修改、经常提交、提交前查看差异。**

---

## 一、先认识几个词

| 词 | 简单解释 |
|---|---|
| 项目文件夹 | 游戏的全部文件所在的文件夹，例如 `MyFirstGame` |
| 仓库（repository） | 被 Git 管理的项目文件夹 |
| 工作区（working tree） | 你当前正在编辑的文件 |
| 提交（commit） | 给当前项目状态拍一张带说明的快照 |
| 分支（branch） | 一条独立的开发线路，用来安全地做新功能 |
| `main` | 通常用来保存“可以运行的主版本” |
| 远程仓库（remote） | GitHub、GitLab 等网站上的备份仓库 |
| 推送（push） | 把本地提交上传到远程仓库 |
| 拉取（pull） | 把远程仓库的新提交下载到本地 |
| 差异（diff） | 查看这次改了哪些行 |
| worktree | Git 为另一条开发线路创建的独立工作目录；Codex 可以用它隔离不同任务 |

你不需要一开始记住所有词。先掌握“状态 → 修改 → 查看 → 提交”这条线即可。

---

## 二、安装 Git

### macOS

打开“终端”应用，输入：

```bash
git --version
```

如果能看到版本号，说明已经安装。若系统提示安装开发者工具，按提示安装即可。

### Windows

安装 Git for Windows：

<https://git-scm.com/download/win>

安装完成后打开 Git Bash，输入：

```bash
git --version
```

### 第一次设置姓名和邮箱

这些信息会写入每次提交中，用于说明是谁做的修改。邮箱可以使用你用于 GitHub 的邮箱，也可以使用 GitHub 提供的隐私邮箱。

```bash
git config --global user.name "你的姓名"
git config --global user.email "你的邮箱"
```

检查设置：

```bash
git config --global --list
```

如果你只在一台电脑上使用 Git，可以先完成这一步，暂时不用研究其他配置。

---

## 三、把游戏项目交给 Git 管理

### 情况 A：你已经有 Unity、Godot 或 Unreal 项目

1. 找到项目的根目录。根目录通常能看到项目配置文件，例如 Unity 的 `Assets`、`Packages`、`ProjectSettings`，Godot 的 `project.godot`，或 Unreal 的 `.uproject` 文件。
2. 在这个目录打开终端。
3. 运行：

```bash
git init -b main
git status
```

如果你的 Git 版本不支持 `-b main`，改用：

```bash
git init
git branch -M main
git status
```

4. 在提交前先创建 `.gitignore`，避免把缓存、构建产物和本地配置提交进去。见本文“`.gitignore` 怎么写”。
5. 第一次提交：

```bash
git add .
git commit -m "chore: 初始化游戏项目"
```

### 情况 B：你还没有项目

先用游戏引擎创建一个空项目，再按上面的步骤初始化 Git。建议先做一个很小的练习，例如：

- 一个可以移动的方块；
- 一个简单的迷宫；
- 一个有开始、暂停、结束按钮的小游戏。

不要一开始就让 Codex 同时制作完整 RPG、联网系统、背包、任务和存档。先让项目能运行，再逐层增加功能。

---

## 四、`.gitignore`：告诉 Git 哪些文件不要保存

`.gitignore` 是项目根目录下的普通文本文件。它用于排除缓存、临时文件、构建输出和密钥。

### 通用基础内容

```gitignore
# 操作系统和编辑器
.DS_Store
Thumbs.db
.vscode/
.idea/

# 日志和临时文件
*.log
*.tmp
*.temp

# 构建输出
Build/
Builds/
build/
dist/

# 本地秘密配置
.env
.env.*
!.env.example

# 本地保存的密钥、账号和证书
*.key
*.pem
secrets/
```

### Unity 项目常见内容

```gitignore
# Unity 生成的缓存和本地状态
[Ll]ibrary/
[Tt]emp/
[Oo]bj/
[Bb]uild/
[Bb]uilds/
[Ll]ogs/
[Mm]emoryCaptures/
/UserSettings/

# 自动生成的用户解决方案文件
*.csproj
*.sln
*.suo
```

### Godot 项目常见内容

```gitignore
# Godot 4 本地导入缓存
.godot/

# 导出文件
export/
exports/
```

### Unreal 项目常见内容

```gitignore
# Unreal 生成的本地文件
Binaries/
DerivedDataCache/
Intermediate/
Saved/
.vs/
```

不同引擎的推荐规则会随版本变化。如果不确定，让 Codex 先检查项目结构，再生成适合当前引擎版本的 `.gitignore`。不要把所有东西一股脑加入忽略列表，因为场景、脚本、材质和项目设置通常需要进入 Git。

---

## 五、每天最常用的 Git 操作

进入游戏项目根目录后，可以按这个顺序操作。

### 1. 查看状态

```bash
git status
```

它会告诉你：哪些文件被修改、哪些文件还没有被 Git 跟踪、当前在哪个分支。

### 2. 查看具体改动

```bash
git diff
```

只查看某个文件：

```bash
git diff -- 路径/文件名
```

### 3. 把想保存的文件放入暂存区

```bash
git add 路径/文件名
```

确认无误后，可以添加全部改动：

```bash
git add .
```

初学时建议先用 `git status` 看清楚文件，再决定是否使用 `git add .`。

### 4. 提交快照

```bash
git commit -m "feat: 添加玩家移动"
```

提交说明要写“做了什么”，不要只写“修改代码”。常见前缀：

```text
feat: 添加新功能
fix: 修复问题
art: 更新美术资源
refactor: 重构但不改变功能
chore: 项目配置或杂务
docs: 更新说明文档
```

### 5. 查看提交历史

```bash
git log --oneline --decorate --graph -20
```

### 6. 查看当前分支

```bash
git branch --show-current
```

---

## 六、推荐的“一个功能一个分支”流程

假设你要让 Codex 添加“玩家冲刺功能”。

### 1. 回到主分支并同步

如果你已经连接了远程仓库：

```bash
git switch main
git pull --ff-only origin main
```

第一次使用且还没有远程仓库时，可以只运行：

```bash
git switch main
```

### 2. 创建功能分支

```bash
git switch -c feature/player-dash
```

分支名可以用英文、数字和短横线，尽量说明功能。

### 3. 给 Codex 的提示词

可以直接复制下面的模板：

```text
我在做一个【Unity/Godot/Unreal】小游戏。

目标：在当前项目中添加“玩家冲刺”功能。
背景：玩家脚本位于【填写文件路径】；输入方式是【键盘按键或手柄按钮】。
要求：
1. 按住或按下【按键】时触发冲刺。
2. 冲刺持续【数字】秒，冷却【数字】秒。
3. 不改变现有跳跃和移动逻辑。
4. 如果需要新增参数，放在容易调整的位置，并说明如何调整。
5. 先检查项目结构和现有代码，再修改文件。

完成标准：
- 游戏可以启动；
- 玩家能正常移动、跳跃和冲刺；
- 冲刺冷却期间不能重复触发；
- 请列出修改的文件、测试步骤和仍然存在的风险。
先给我一个简短计划，确认后再写代码。
```

### 4. 让 Codex 做完后，先看差异

在 Codex 中可以要求：

```text
请先查看当前 Git 差异，逐个解释修改了哪些文件、每处修改解决什么问题。不要继续扩大功能范围。
```

如果项目在 Git 仓库中，Codex 的代码审查功能可以查看未提交改动或相对主分支的改动；你也可以在终端使用：

```bash
git diff --stat
git diff
```

### 5. 运行游戏并提交

确认能运行后：

```bash
git status
git add .
git commit -m "feat: 添加玩家冲刺"
```

如果你暂时不确定是否要保存，先不要提交，保留改动并让 Codex 帮你检查。

### 6. 合并回主分支

功能验证完成后：

```bash
git switch main
git merge --no-ff feature/player-dash -m "merge: 合并玩家冲刺功能"
```

确认主分支可以运行后，可以删除已经合并的本地分支：

```bash
git branch -d feature/player-dash
```

如果你使用 GitHub，也可以把功能分支推送上去，再通过 Pull Request 合并。初学者可以先掌握本地流程，再学习 Pull Request。

---

## 七、让 Codex 更稳定地工作的 `AGENTS.md`

`AGENTS.md` 是放在项目中的说明文件。Codex 会把它当作项目协作规则读取。它适合记录：项目结构、启动方法、测试方法、代码约定和“完成的标准”。

你可以在项目根目录创建一个 `AGENTS.md`，内容从下面开始：

```markdown
# 游戏项目协作规则

## 项目说明
- 引擎：填写 Unity / Godot / Unreal 及版本
- 游戏类型：填写 2D / 3D、玩法和目标平台
- 入口场景：填写场景文件

## 目录说明
- `Scripts/`：游戏脚本
- `Scenes/`：场景
- `Art/`：美术资源
- `Audio/`：音频
- `Docs/`：设计说明

## 工作规则
- 修改前先阅读相关文件，不要凭文件名猜内容。
- 优先做小范围修改，不要未经要求重写整个系统。
- 不删除现有功能；如果必须改变行为，先说明影响。
- 不把密钥、账号、个人路径写进代码或提交。
- 每次修改后说明改了哪些文件，以及如何验证。

## 验证方法
- 启动游戏并进入【入口场景】。
- 按照 `Docs/test-checklist.md` 完成功能检查。
- 提交前查看 `git diff`，确认没有缓存、构建输出或秘密配置。
```

如果你使用 Codex CLI，可以让 Codex 先在当前目录生成一个初稿：

```text
请先检查这个游戏项目，并生成一份简短的 AGENTS.md。只记录真实存在的目录、启动方式和验证步骤，不要猜测。
```

生成后要自己打开检查，尤其是引擎版本、启动命令和目录名称。

---

## 八、在 Codex 桌面应用里配合 Git

推荐把 Codex 当作“会改文件的开发伙伴”，把 Git 当作“记录和保护项目历史的工具”。打开 Codex 后，选择游戏项目的根目录；如果目录已经是 Git 仓库，Codex 可以读取项目上下文并显示改动。

复杂任务先这样说：

```text
先进入计划模式。请阅读项目结构和相关脚本，先给出实现计划和验证步骤，暂时不要修改文件。
```

开始修改后，建议分成几个小回合：

1. 先让 Codex 只分析项目和定位文件。
2. 再让它实现一个小功能。
3. 让它查看 `git diff`，解释每个改动。
4. 你在游戏里运行并操作验证。
5. 最后才提交 Git。

如果 Codex 界面提供 **Review** 或 `/review`，可以选择“审查未提交改动”，让它按问题严重程度检查当前差异。审查结果是帮助你判断的依据，仍要自己启动游戏确认实际表现。

如果你想让 Codex 同时尝试两个互不相关的功能，可以使用 **Worktree**。它会为任务提供独立的工作目录，减少不同任务互相覆盖的机会。初学时只要记住：一个分支同一时间不要在两个工作目录同时修改；完成后把已验证的改动合并回 `main`。

每次提示 Codex 时，尽量写清四件事：

- **目标：** 想新增或修复什么；
- **背景：** 使用的引擎、场景、相关文件和现象；
- **限制：** 不要改哪些系统，性能或平台有什么要求；
- **完成标准：** 运行后应该看到什么，以及怎样验证。

这比只说“帮我把游戏做得更好”更容易得到稳定结果。

---

## 九、常见的撤销和恢复

### 还没有提交，想丢弃某个文件的改动

```bash
git restore 路径/文件名
```

这会删除该文件未提交的本地改动。执行前一定确认文件里没有需要保留的内容。

### 已经 `git add`，但还没有提交，想取消暂存

```bash
git restore --staged 路径/文件名
```

这不会删除文件内容，只是把它从“准备提交”状态移回普通修改状态。

### 提交说明写错了，想修改最近一次提交

```bash
git commit --amend -m "正确的提交说明"
```

只适合尚未推送给别人的最近一次提交。

### 最近一次提交不合适，但想保留文件改动

```bash
git reset --soft HEAD~1
```

提交会被撤回，文件改动会保留在暂存区。初学时如果不确定，先让 Codex 解释当前状态，不要连续执行多个 reset。

### 已经推送的提交需要反向修正

```bash
git revert <提交编号>
```

这会创建一个新的“撤销提交”，更适合已经共享给别人的分支。

### 不知道自己现在处于什么状态

```bash
git status
git log --oneline --decorate --graph -10
git diff
git diff --staged
```

把这四条命令的输出交给 Codex，并告诉它“只分析，不要执行删除或重置操作”。

---

## 十、遇到合并冲突怎么办

冲突表示两个修改同时改了同一处内容，Git 无法自动判断应该保留哪一个。

1. 先查看冲突文件：

```bash
git status
```

2. 打开文件，找到类似标记：

```text
<<<<<<< HEAD
当前分支的内容
=======
要合并进来的内容
>>>>>>> feature/player-dash
```

3. 保留正确内容，并删除这些冲突标记。
4. 运行游戏或测试。
5. 标记冲突已解决并提交：

```bash
git add 冲突文件路径
git commit -m "fix: 解决玩家移动逻辑冲突"
```

如果你不确定应该保留哪一段，可以把冲突文件和需求告诉 Codex：

```text
这是 Git 合并冲突。请先解释两段代码分别做什么，指出保留哪部分的理由，给出最小修改方案。不要直接删除任何一段。
```

如果发现这次合并方向错了，还没有提交时可以取消：

```bash
git merge --abort
```

---

## 十一、连接 GitHub 做备份和协作

Git 只保存在本地时，电脑损坏仍可能丢失项目。把仓库推送到 GitHub 可以增加远程备份，也便于之后使用 Pull Request。

### 创建远程仓库

在 GitHub 网站创建一个空仓库。初次创建时不要自动添加 README、`.gitignore` 或许可证，因为本地项目已经有这些文件。

然后在本地运行：

```bash
git remote add origin https://github.com/你的账号/你的仓库名.git
git push -u origin main
```

检查远程地址：

```bash
git remote -v
```

以后推送：

```bash
git push
```

以后开始工作前同步：

```bash
git pull --ff-only
```

不要把下面内容提交到 GitHub：

- API 密钥、账号密码、私钥；
- `.env`、`secrets/` 等本地秘密配置；
- 体积很大的构建文件和缓存；
- 未确认版权的素材、音乐和字体。

如果游戏素材很大，可以以后再了解 Git LFS。先用 `.gitignore` 排除可重新生成的缓存和构建产物。

---

## 十二、适合游戏项目的提交节奏

推荐每完成一个可以描述清楚的小功能就提交一次：

```text
feat: 添加玩家移动
feat: 添加相机跟随
feat: 添加第一关入口
fix: 修复玩家穿墙
art: 更新主角待机动画
ui: 添加开始菜单
save: 添加本地存档
chore: 更新引擎配置
```

一个提交尽量只表达一个主题。比如“添加敌人巡逻”和“重做整个 UI”最好分成两次提交。这样出了问题时更容易找回，也更容易让 Codex 或其他人审查。

建议给阶段性版本打标签：

```bash
git tag -a v0.1.0 -m "完成第一关可玩原型"
git push origin v0.1.0
```

---

## 十三、你可以直接复制给 Codex 的常用提示词

### 了解项目，不改代码

```text
请先检查这个游戏项目的目录结构、引擎版本和入口场景。只做分析，不修改文件。最后告诉我：项目如何启动、主要脚本在哪里、下一步最适合做什么。
```

### 做一个小功能

```text
请为当前游戏添加【功能名称】。
先阅读相关文件并给出 3 到 5 步计划，不要马上改代码。
限制：只修改实现该功能所需的文件，不重写无关系统。
完成标准：【列出玩家可以观察到的结果】。
完成后请列出修改文件、运行步骤和未解决风险。
```

### 检查而不修改

```text
请检查当前未提交的 Git 改动。
重点关注：运行错误、输入逻辑、空引用、性能问题、资源路径和不必要的大范围改动。
只报告问题和证据，不要修改文件。
```

### 做完后验证

```text
请按照以下清单验证当前功能：
1. 启动游戏；
2. 进入【场景名】；
3. 执行【操作】；
4. 检查【预期结果】；
5. 查看 Git diff，确认没有生成物和秘密配置。
如果某项无法验证，请明确说明原因。
```

### 请 Codex 帮你解释错误

```text
我没有编程经验。请用通俗中文解释这个错误：
【粘贴错误信息】
请按“原因、影响、最小修复步骤、如何验证”的顺序回答。先不要大范围重构。
```

---

## 十四、最小日常清单

开始工作：

```bash
git switch main
git pull --ff-only
git switch -c feature/本次功能
```

让 Codex 工作后：

```bash
git status
git diff
```

确认游戏运行正常后：

```bash
git add .
git commit -m "feat: 描述本次功能"
git push -u origin feature/本次功能
```

结束工作前：

```bash
git status
```

你希望看到的是：工作区干净，或者只剩下你明确知道还没有完成的改动。

---

## 十五、给你的第一周建议

**第 1 天：** 安装 Git，配置姓名和邮箱，创建一个很小的游戏项目，完成第一次提交。

**第 2 天：** 学会 `git status`、`git diff`、`git add`、`git commit`，让 Codex 做一个小功能。

**第 3 天：** 学会创建和切换分支，给“玩家移动”“开始菜单”分别建立分支。

**第 4 天：** 创建 `AGENTS.md`，写清楚项目结构、运行方式和验证步骤。

**第 5 天：** 创建 GitHub 远程仓库，推送 `main` 和一个功能分支。

**第 6 天：** 故意做一次小修改，再用 `git restore` 撤销，理解 Git 的保护作用。

**第 7 天：** 做一个可运行的小版本，打上 `v0.1.0` 标签，并写一份游戏测试清单。

先做到“每次修改前后都知道 Git 状态”，再学习更复杂的 rebase、LFS、CI 和多人协作规则。

---

## 参考

- Git 官方网站：<https://git-scm.com/doc>
- Codex 官方文档中的项目指导与 `AGENTS.md`：<https://learn.chatgpt.com/docs/agent-configuration/agents-md>
- Codex 官方文档中的代码审查：<https://learn.chatgpt.com/docs/code-review>
- Codex 官方文档中的 Git worktree：<https://learn.chatgpt.com/docs/environments/git-worktrees>
