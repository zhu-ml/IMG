# git常用命令整理

### 工作流程

 ![git流程图](https://github.com/zhu-ml/IMG/raw/master/img/gitTheory.png)

### PPAP项目常用操作命令

#### 配置全局用户

git config --global user.name "用户名"
git config --global user.email "邮箱"

#### 克隆远程仓库到本地
git clone http://git.dev.sh.ctripcorp.com/sixthquake/ppap-ui.git

#### 创建各自开发分支
git checkout -b feature/business1
创建并切换到新分支，分支命名规则：feature/business（业务功能），hotFix/bug（bug修复）

#### 查看文件状态
git status

#### 添加本地代码到暂存区
git add filename 加入单个文件
git add . 或   git add * 加入多个文件
注：git add . 会把本地所有未跟踪的文件都加入暂存区，并且会根据.gitignore做过滤（常用）
git add * 会忽略.gitignore把任何文件都加入

#### 提交暂存区代码到本地仓库
git commit -m ‘message’
git commit --no-verify -m "message" 跳过验证继续提交

#### 将多次提交合并成一个
git commit –amend 修复合并上一次的提交，修改提交信息后wq保存
git commit –amend –m "本次提交信息" 修复提交，同时修改提交信息
git commit –amend –no-edit 修复但不修改提交信息

#### 推送代码到远程分支
git push --set-upstream origin feature/business1 首次推送（省略形式为：git push -u origin feature/business1）
git push –f 强制推送

#### 拉取分支
git fetch	拉取所有分支的更新
git fetch –p 拉取更新、（prune）删除远程已被删除的分支

#### 列出分支
git branch 列出所有本地分支
git branch -r 列出所有远程分支
git branch –a 列出所有本地和远程分支

#### 合并远程代码到本地
git rebase origin/master
git add .    git rebase –continue 继续rebase直到没有冲突再进行提交推送代码
git rebase --abort放弃rebase操作
git merge origin/master 
git merge --abort 放弃merge操作

#### 查看操作记录
git log 可以显示所有提交过的版本信息
git reflog 可查看所有分支的所有操作记录（包括已被删除的commit记录和reset操作）

#### 回退到对应的版本
git reset version --hard 彻底回退到对应的版本version
git reset HEAD^  回退到add之前的状态，更改本地源码，慎用
git reset --soft HEAD^  回退到add之后，commit之前的状态，仅修改暂存区的记录
reset直接删除指定的commit,将HEAD向后移动（仅作用本地不影响远程分支）

#### 撤销操作
git revert HEAD 撤销指定的commit
revert用新的commit回滚之前的commit,HEAD继续向前（直接影响远程分支）

#### 合并部分代码
git cherry-pick <commitHash> 将指定的提交应用于其他分支
git cherry-pick feature 将feature分支最近一次提交，转移到当前分支

### GitHook工具husky
husky可以防止使用Git hooks的一些不好的commit或者push
pre-commit: 在代码提交之前做些东西，如代码打包，代码检查，称为钩子。在commit之前执行一个函数（callback）。函数成功执行后再继续commit，失败后阻止commit。
npm install husky --save-dev 安装pre-commit
lint-staged: 用于实现每次提交只检查本次提交所修改的文件

package.json配置:
```
"husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.{js,jsx,d.ts,ts,tsx}": [
      "eslint"
    ],
    "*.{js,jsx,d.ts,ts,tsx,json,css,less}": [
      "prettier --write"
    ]
  }
```

代码pre-commit检查提交：

![pre-commit检查](https://github.com/zhu-ml/IMG/raw/master/img/pre-commit.png)

![推送代码到远程仓库](https://github.com/zhu-ml/IMG/raw/master/img/push.png)

