# 发布 dsh-gostc 到 DSH 插件市场

本目录是**可发布副本**（已脱离 `@local` 私有 scope）。已安装的本地插件
`@local/dsh-gostc` 不受影响。按下面步骤即可上架应用内插件市场。

市场机制：应用内「插件市场」(dshmarket) 的目录实时取自精选清单仓库
**awesome-dsh-plugin/awesome-dsh-plugin**。上架 = 向该仓库提 PR 新增一个条目文件；
合并后约 1 天内站点与市场自动收录。市场只安装清单内的来源。

> 先把所有 `PonChunhui` 替换成你的真实 GitHub 用户名（package.json / README /
> LICENSE / NOTICE / catalog-entry.yml）。包名默认无 scope `dsh-gostc`；
> 若 npm 上已被占用，改用 `@PonChunhui/dsh-gostc`，并同步修改：
> package.json `name`、cordis.patch.yml `name`、client.js 模块 `id`、
> catalog-entry.yml `name`（写成 `PonChunhui/dsh-gostc`，与仓库一致）。

## 0. 前置条件
- 一个 GitHub 账号；本机已装 git（已有）。建议安装 gh CLI 便于建仓库/提 PR（可选）。
- npm 账号（`npm login`）。当前环境 npm 未登录、无 gh，联网步骤需你授权。
- 清单仓库 CI 要求**仓库创建满 1 天**后再提 PR。

## 1. 创建并推送 GitHub 仓库
```sh
# 在 GitHub 新建公开仓库 PonChunhui/dsh-gostc（不要初始化 README，避免冲突）
# 给仓库添加 topic: dsh-plugin  （Settings → Topics，或 gh repo edit --add-topic dsh-plugin）

cd C:\code\gostc-open-main\dsh-gostc-publish
git init -b main
git add .
git commit -m "dsh-gostc 1.1.0: gostc manager + auto provision/launch"
git remote add origin https://github.com/PonChunhui/dsh-gostc.git
git push -u origin main
```
确认仓库根 `package.json` 含 `dsh.bundle.patch`（已有）——这是 CI 的硬性检查，
只声明 `dsh.client` 会被拒。

## 2. （可选）补充真实截图
市场详情页支持 App Store 式截图。截 1–8 张真实面板图放到 `assets/`，然后：
```sh
# 参照 screenshots.example.json 创建 screenshots.json，列出相对路径
copy screenshots.example.json screenshots.json
# 编辑 screenshots.json 使其指向你实际放入 assets/ 的图片
git add assets screenshots.json && git commit -m "docs: screenshots" && git push
```
不声明也可以——市场会从 README 自动抽取图片。**不要伪造截图。**

## 3. 发布 npm（你已选择发布）
```sh
npm login
npm publish            # 无 scope；若用 @PonChunhui/dsh-gostc 则 npm publish --access public
```
**发布后必须核对产物，而不是只看 `+ name@version` 提示：**
```sh
# dist-tags.latest 应等于 1.1.0，且 dist.tarball 能下载
curl https://registry.npmjs.org/dsh-gostc
```
- 新包名首次发布可能只留下 `0.0.0-stage` 占位包：把同一版本再发一次即可挂上真包。
- 国内镜像可能滞后仍报占位版：`curl -X PUT https://registry.npmmirror.com/dsh-gostc/sync` 后等几分钟。
- 在干净 profile 实测安装：`dsh plugin --profile pubtest add dsh-gostc`。
- 已发布包的 `repository` 字段必须指回第 1 步的仓库（已配置），否则市场不会关联下载量。

## 4. 向清单仓库提 PR
```sh
# fork https://github.com/awesome-dsh-plugin/awesome-dsh-plugin 并 clone
cd awesome-dsh-plugin
# 新增一个文件（文件名必须等于 url 推出的 slug）：
#   data/plugins/PonChunhui__dsh-gostc.yml
# 内容直接用本目录的 catalog-entry.yml（已替换 PonChunhui）
copy C:\code\gostc-open-main\dsh-gostc-publish\catalog-entry.yml data\plugins\PonChunhui__dsh-gostc.yml

# 本地自查（跑的就是 CI 那套条目校验）
npm ci
node scripts/generate-readme.mjs

git checkout -b add-dsh-gostc
git add data/plugins/PonChunhui__dsh-gostc.yml
git commit -m "add dsh-gostc"
git push -u origin add-dsh-gostc
# 在 GitHub 开 PR（一个 PR 最多 3 条；只改自己这一条，不要手改 README）
```
条目要点：`category: remote`；`description.en` 必填单行以句号结尾；含 `: ` 的描述加引号；
不要写注释、不要手写 `npm:` 键；`icon` 必须是 GitHub 托管的位图（已用 assets/icon-180.png）。

## 5. 评审与收录
- CI 依次检查：条目数 → `dsh.bundle` → 仓库年龄(≥1天) → awesome-lint/站点构建。
  红叉会指明要改什么；**同分支推修复**即可，无需重开 PR。
- 维护者会读源码核对描述是否属实。需要修改时打 `held: author` 标签：
  推修复到同分支 + 留**一条**评论说明改了什么，然后等重新评审（不要反复催）。
- 合并后站点与市场自动重建，约 1 天内可在应用内市场搜到并安装。

## 常见打回原因（避免）
- 只声明 `dsh.client` 没有 `dsh.bundle.patch`（本包已具备）。
- 仓库创建不足 1 天 / 占位空壳仓库。
- 描述夸大或与代码不符；分类不贴切（维护者会直接改，不会因此打回）。
- 未加 `dsh-plugin` topic；纯聚合包；依赖指向他人副本而非上游。