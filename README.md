# 产后核心修复 · 每日一练

单文件 Web App（`index.html`），用 GitHub Pages 托管，数据通过 GitHub 私有仓库在多台设备间同步。

## 结构

| 仓库 | 可见性 | 内容 |
|---|---|---|
| `fitness-app`（名字任意） | Public | `index.html`、`README.md` → GitHub Pages 网站 |
| `fitness-data`（名字任意） | **Private** | `fitness-data.json`（App 自动创建）|

> 免费账号的 GitHub Pages 只能用于公开仓库，所以代码和数据分开放：代码公开不含任何个人数据，数据放私有仓库。

## 设置步骤

1. **建代码仓库**：GitHub → New repository → `fitness-app`，Public → 上传 `index.html` 与 `README.md`。
2. **开 Pages**：仓库 Settings → Pages → Source 选 *Deploy from a branch*，Branch 选 `main` / `(root)` → Save。几分钟后网址为 `https://<用户名>.github.io/fitness-app/`。
3. **建数据仓库**：New repository → `fitness-data`，**Private**，勾选 *Add a README*（仓库不能是空的）。
4. **生成令牌**：头像 → Settings → Developer settings → Personal access tokens → **Fine-grained tokens** → Generate new token
   - Expiration：建议 90 天或 1 年
   - Repository access：**Only select repositories** → 只选 `fitness-data`
   - Permissions → Repository permissions → **Contents: Read and write**（其他保持不变）
   - 生成后复制 `github_pat_…`（只显示一次）
5. **在 App 里填写**：打开网址 → 「进度」页 → 「GitHub 云同步」→ 填用户名、`fitness-data`、令牌 → 保存并同步。
6. **其他设备**：打开同一网址，填同样的设置即可。手机上可“添加到主屏幕”。

## 同步机制

- 打开页面、切回页面、联网恢复、每次保存后 1.5 秒自动同步。
- 每条记录按日期存储并带修改时间 `_t`，两边不一致时保留较新的那条。
- 清空数据会记录 `resetAt`，同步到所有设备；GitHub 提交历史里仍能找回旧数据。
- 每次有变化会在数据仓库产生一次提交（commit），这是正常的。

## 已知限制

- 冲突以“整天”为单位：如果两台设备**都离线**、同一天各自改了记录，联网后只保留较新的那一天记录。
- 令牌保存在各设备浏览器的 localStorage 中；设备丢失时请到 GitHub 撤销令牌。
- 令牌过期后会显示“令牌无效或已过期”，重新生成并填入即可。
