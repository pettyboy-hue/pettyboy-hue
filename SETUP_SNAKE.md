# 🐍 贪吃蛇贡献图设置指南

由于 GitHub API 的 `workflow` 权限限制，需要你手动创建 Action 文件。

## 操作步骤（2分钟搞定）

### 1. 创建 workflow 文件

1. 打开 https://github.com/pettyboy-hue/pettyboy-hue
2. 点击 **Add file → Create new file**
3. 文件名输入：`.github/workflows/snake.yml`
4. 粘贴以下内容：

```yaml
name: Generate Snake Animation

on:
  schedule:
    - cron: "0 */12 * * *"
  workflow_dispatch:

jobs:
  generate:
    permissions:
      contents: write
    runs-on: ubuntu-latest
    timeout-minutes: 5
    steps:
      - name: Generate Snake
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-snake.svg
            dist/github-snake-dark.svg?palette=github-dark

      - name: Push to output branch
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

5. 点 **Commit new file**

### 2. 手动触发首次运行

1. 去仓库的 **Actions** 标签页
2. 左侧选 **Generate Snake Animation**
3. 点 **Run workflow → Run workflow**
4. 等 1 分钟左右，完成后刷新你的 Profile 页面就能看到贪吃蛇了！

### 3. 清理

设置完成后可以删除这个 `SETUP_SNAKE.md` 文件。
