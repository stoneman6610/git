# git

本仓库由本地目录 `git` 初始化而来。

## 开始使用

```bash
git add .
git commit -m "feat: init"
git push
```

## 说明

- 默认分支：`main`
- `.gitignore` 已覆盖常见系统文件、编辑器配置、依赖产物与环境变量文件
- 请勿提交任何密钥、凭证或私人数据

## 提交前密钥扫描（pre-commit）

仓库内置 gitleaks 钩子，提交时自动扫描**暂存区**，命中密钥则阻断提交。

### 首次使用需启用钩子

`core.hooksPath` 是本地配置，**clone 后需手动执行一次**：

```bash
git config core.hooksPath .githooks
```

并确保已安装 gitleaks（钩子会依次在 `PATH`、`~/.local/bin` 中查找）：

```bash
gitleaks version    # 需要 >= 8.19
```

未安装 gitleaks 时钩子会**跳过扫描并告警**，不阻断提交。

### 误报处理

提交被拦时，gitleaks 会输出 `Fingerprint`，可据此精确忽略：

- 临时忽略单条 → 写入 `.gitleaksignore`
- 长期排除某路径/规则 → 编辑 `.gitleaks.toml` 的 `[[allowlists]]`
- 确属密钥 → 改为环境变量注入，不要提交

强行跳过：`git commit --no-verify`（不推荐，密钥会进入历史）。

> ⚠️ 密钥一旦 push，仅删除文件无效，必须**吊销并轮换**。历史清理需用 `git filter-repo`。

