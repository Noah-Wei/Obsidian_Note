# VScode使用Git

## Q1:本地代码提交到Github代码库

**需求**

> 现在我的github中，有一个库DemoHub，里面有一些文件。然后我在本地使用了Vscode编写了一个文件1.txt，我现在想把这个文件推送到我的DemoHub库中，我应该怎么做？

**步骤**

#### 1. **确保本地已安装 Git 并配置 GitHub**
   - 如果尚未安装 Git，请先下载并安装：[Git 官网](https://git-scm.com/)
   - 配置 Git 用户名和邮箱（与 GitHub 一致）：
     ```bash
     git config --global user.name "你的GitHub用户名"
     git config --global user.email "你的GitHub邮箱"
     ```

---

#### 2. **将本地文件放入仓库目录**
   - 将 `1.txt` 复制到 `DemoHub` 仓库的本地文件夹中（如果没有克隆过仓库，先执行下一步的克隆）。

---

#### 3. **克隆远程仓库到本地（如果尚未克隆）**
   ```bash
   git clone https://github.com/你的用户名/DemoHub.git
   cd DemoHub  # 进入仓库目录
   ```

---

#### 4. **添加文件到本地仓库**
   - 将 `1.txt` 复制到 `DemoHub` 文件夹后，执行：
     ```bash
     git add 1.txt
     ```
     或添加所有更改的文件：
     ```bash
     git add .
     ```

---

#### 5. **提交更改**
   ```bash
   git commit -m "添加了1.txt文件"
   ```

---

#### 6. **推送到远程仓库**
   ```bash
   git push origin main  # 如果分支是main（GitHub默认分支）
   ```
   如果分支是 `master`，则替换为：
   ```bash
   git push origin master
   ```

---

#### 7. **验证**
   - 刷新 GitHub 仓库页面，确认 `1.txt` 已出现。

---

#### 常见问题：
1. **权限错误**：
   - 如果 `git push` 失败，检查是否已登录 GitHub。可以通过 HTTPS 或 SSH 验证：
     - HTTPS：输入用户名和密码（推荐使用 Personal Access Token 替代密码）。
     - SSH：需提前配置 SSH 密钥，参考 [GitHub SSH 文档](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)。

2. **分支冲突**：
   - 如果远程有更新，先执行 `git pull origin main` 合并更改后再推送。

3. **VSCode 内置 Git 工具**：
   - 可以直接在 VSCode 的源代码管理界面（左侧图标）中点击按钮完成 `add`、`commit` 和 `push`。

## Q2:本地代码库和Github代码库同步

**需求**

> 如果现在DemoHub的文件发生了变化，但是我本地还是之前的版本，我怎么把本地的代码和DemoHub中的代码同步

**步骤**

#### **1. 确保本地仓库已关联远程仓库**

- 进入本地仓库目录：
  ```bash
  cd /path/to/DemoHub  # 替换为你的本地仓库路径
  ```
- 检查是否已关联远程仓库：
  ```bash
  git remote -v
  ```
  - 如果显示 `origin` 指向你的 `DemoHub` 仓库（如 `https://github.com/你的用户名/DemoHub.git`），则跳过下一步。
  - 如果没有关联，手动添加远程仓库：
    ```bash
    git remote add origin https://github.com/你的用户名/DemoHub.git
    ```

---

#### **2. 拉取远程最新更改**
- 执行以下命令拉取远程分支（默认通常是 `main` 或 `master`）：
  ```bash
  git pull origin main   # 如果远程分支是 main
  ```
  或
  ```bash
  git pull origin master # 如果远程分支是 master
  ```

  - **注意**：如果本地有未提交的更改，Git 会拒绝合并。你需要先：
    1. 提交本地更改：
       ```bash
       git add .
       git commit -m "备份本地更改"
       ```
    2. 再执行 `git pull`。
    3. 如果存在冲突，需手动解决冲突后重新提交。

---

#### **3. 处理冲突（如果有）**
- 如果拉取时提示冲突，Git 会在文件中标记冲突内容（如 `<<<<<<< HEAD` 和 `>>>>>>>`）。
- 手动编辑文件解决冲突，保留需要的代码，删除冲突标记。
- 标记冲突已解决并提交：
  ```bash
  git add 冲突的文件名
  git commit -m "解决冲突"
  ```

---

#### **4. 强制覆盖本地（谨慎使用）**
如果希望完全丢弃本地更改，强制与远程一致：
```bash
git fetch origin         # 获取远程最新记录
git reset --hard origin/main  # 强制将本地分支重置到远程状态（谨慎！会丢失本地未提交的更改）
```

---

#### **5. 验证同步结果**
- 检查本地文件是否已更新：
  ```bash
  git status  # 应显示 "Your branch is up to date with 'origin/main'."
  ```

---

#### **关键注意事项**
1. **分支一致性**：
   - 确保本地和远程分支名称一致（如 `main` 或 `master`），可通过 `git branch -a` 查看所有分支。
2. **HTTPS/SSH 权限**：
   - 如果 `git pull` 提示权限问题，可能需要重新输入账号密码（HTTPS）或配置 SSH 密钥。
3. **VSCode 操作**：
   - 在 VSCode 中，可以直接点击左侧源代码管理图标 → **拉取更改**（同步图标）完成同步。

---

#### **总结命令流程**
```bash
cd /path/to/DemoHub
git pull origin main     # 常规拉取
# 或
git fetch origin         # 获取远程更新
git merge origin/main    # 手动合并
```

同步后，本地代码将与 GitHub 上的 `DemoHub` 仓库完全一致。

## Q3:代码库变化，本地提交

**需求**

> 那如果现在DemoHub库中被别人提交了一些代码文件，然后现在我又要提交代码，我应该怎么做？

**步骤**

**完整流程：同步远程更改 → 提交本地代码 → 推送**

#### 1. **拉取远程最新代码（必做！）**
   ```bash
   git pull origin main      # 假设远程分支是 main（GitHub 默认）
   ```
   - 如果分支是 `master`，替换为 `git pull origin master`。
   - 这一步会将远程的更新合并到你的本地仓库。

   **可能遇到的情况：**
   - **无冲突**：自动合并成功，直接进入下一步。
   - **有冲突**：Git 会提示冲突文件，需手动解决（见下文）。

---

#### 2. **处理合并冲突（如果有）**
   - 冲突文件会包含类似标记：
     ```
     <<<<<<< HEAD
     你的本地代码
     =======
     远程的代码
     >>>>>>> commit-hash
     ```
   - **手动编辑文件**，保留需要的部分，删除冲突标记。
   - 标记冲突已解决：
     ```bash
     git add 冲突的文件名   # 将解决后的文件加入暂存区
     git commit            # 提交合并结果（会自动生成提交信息）
     ```

---

#### 3. **添加并提交你的本地代码**
   ```bash
   git add .               # 添加所有更改的文件（或指定文件 git add 1.txt）
   git commit -m "你的提交说明"
   ```

---

#### 4. **推送到远程仓库**
   ```bash
   git push origin main    # 推送本地提交到远程 main 分支
   ```

---

#### **关键注意事项**
1. **永远先 `git pull` 再 `git push`**  
   - 直接推送可能导致冲突被拒绝（需先合并远程更改）。

2. **分支一致性**  
   - 使用 `git branch -a` 确认本地和远程分支名称一致（如 `main` 或 `master`）。

3. **VSCode 快捷操作**  
   - 在 VSCode 的源代码管理界面：
     1. 点击 **同步更改**（循环箭头图标）拉取远程更新。
     2. 解决冲突后提交。
     3. 点击 **推送**（上传图标）。

---

#### **常见问题处理**
#### ❌ 错误：`! [rejected] main -> main (non-fast-forward)`
  - **原因**：本地分支落后于远程。
  - **解决**：先执行 `git pull` 同步。

#### ❌ 错误：`Your local changes would be overwritten by merge`
  - **原因**：本地有未提交的更改。
  - **解决**：
    ```bash
    git stash          # 临时保存本地更改
    git pull           # 拉取远程更新
    git stash pop      # 恢复本地更改并解决冲突
    ```

---

#### **总结流程**
```bash
cd /path/to/DemoHub
git pull origin main     # 1. 拉取远程更新
git add .                # 2. 添加你的更改
git commit -m "你的描述"
git push origin main     # 3. 推送
```