# 🧠 智能化 Profile 设置指南

让你的 GitHub Profile 变成一个**自更新的智能仪表盘**。按顺序操作，全部搞完大约 15 分钟。

---

## 📡 Step 1: 实时活动流 (Recent Activity)

在仓库中创建 `.github/workflows/update-activity.yml`：

```yaml
name: Update Activity

on:
  schedule:
    - cron: "*/30 * * * *"  # 每30分钟更新
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v4
      - uses: jamesgeorge007/github-activity-readme@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          COMMIT_MSG: "⚡ Update recent activity"
          MAX_LINES: 5
```

创建后去 **Actions** 页面手动触发一次。

---

## ⏱️ Step 2: WakaTime 编码统计

### 2.1 安装 WakaTime
1. 去 [wakatime.com](https://wakatime.com) 注册
2. 在 VS Code 安装 WakaTime 插件
3. 输入你的 API Key

### 2.2 获取 WakaTime API Key
1. 登录 WakaTime → Settings → API Key
2. 去你的 GitHub 仓库 → Settings → Secrets → New repository secret
3. 名字: `WAKATIME_API_KEY`，值: 你的 API Key

### 2.3 创建 Workflow
创建 `.github/workflows/wakatime.yml`：

```yaml
name: WakaTime Stats

on:
  schedule:
    - cron: "0 0 * * *"  # 每天更新
  workflow_dispatch:

jobs:
  update-readme:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: anmol098/waka-readme-stats@master
        with:
          WAKATIME_API_KEY: ${{ secrets.WAKATIME_API_KEY }}
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          SHOW_OS: "True"
          SHOW_PROJECTS: "True"
          SHOW_EDITORS: "True"
          SHOW_LANGUAGE: "True"
          SHOW_LINES_OF_CODE: "True"
          SHOW_PROFILE_VIEWS: "False"
          SHOW_SHORT_INFO: "False"
          SHOW_LOC_CHART: "False"
          LOCALE: "en"
          COMMIT_BY_ME: "True"
```

---

## 🏔️ Step 3: 3D 贡献地图

创建 `.github/workflows/3d-contrib.yml`：

```yaml
name: 3D Contribution

on:
  schedule:
    - cron: "0 1 * * *"  # 每天凌晨1点更新
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v4
      - uses: yoshi389111/github-profile-3d-contrib@0.7.1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          USERNAME: pettyboy-hue
      - name: Commit & Push
        run: |
          git config user.name github-actions[bot]
          git config user.email github-actions[bot]@users.noreply.github.com
          git add -A .
          git diff --staged --quiet || git commit -m "🏔️ Update 3D contribution map"
          git push
```

首次运行后，会在仓库里自动创建 `profile-3d-contrib/` 文件夹，里面包含多种主题的 3D SVG。

---

## ✅ 完成后的效果

| 模块 | 更新频率 | 数据来源 |
|------|---------|---------|
| 📡 Activity Feed | 每30分钟 | GitHub Events API |
| ⏱️ WakaTime Stats | 每天 | WakaTime API |
| 🏔️ 3D Contribution | 每天 | GitHub GraphQL |
| 📊 Summary Cards | 实时 | 第三方 API |
| 🐍 Snake | 每12小时 | GitHub Contributions |
| 📈 Activity Graph | 实时 | 第三方 API |
| 🏆 Trophies | 实时 | 第三方 API |

---

## 🔧 故障排查

**Q: Action 失败了怎么办？**
去 Actions 页面查看日志，一般是权限问题。确保仓库 Settings → Actions → General → Workflow permissions 选择 "Read and write permissions"。

**Q: WakaTime 没数据？**
需要先用 VS Code 编码几天，积累数据后才会显示。

**Q: 3D 贡献图没出现？**
首次运行需要手动触发 workflow，等它跑完后刷新 Profile 页面。

---

> 💡 设置完成后，你的 Profile 会成为一个**活的仪表盘**，每天自动更新，展示你的实时编码状态！
