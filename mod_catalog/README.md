# Mod 商店数据源（多源 + 分类）

游戏内「下载模组」商店的数据由本目录远程驱动，**上新 mod / 新仓库 / 新分类都不需要重打包游戏**。

## 文件

- `index.json` — 源索引：`sources` 数组，每项 `{id, name, url}`，`url` 指向某个仓库的 `catalog.json`（raw 直链）。游戏先拉索引，再逐个拉各源目录合并展示；任一源失败只跳过该源。**加新仓库 = 往 `sources` 里加一项。**
- `catalog.json` — 本仓库的模组目录：`mods` 数组，字段与旧版相同（id/name/icon/version/size/download_url/sha256/description），新增可选字段 `category`。

## 条目分类（category）

- `category` 是任意字符串，完全由数据定义，游戏内不写死分类名单；商店顶部会按数据里出现过的分类（首次出现顺序）自动生成筛选。
- **加新分类 = 在条目里写一个新分类名即可**，不用改游戏、不用改索引。
- 不写 `category` 的条目只出现在「全部」里。

## 上新 mod 的步骤

1. 把 .dfmod 传到某个 Release，在对应仓库的 `catalog.json` 里加一条（id/名称/版本/直链/大小/sha256/描述，可带 `category`）。
2. 若是全新仓库：先在该仓库放好 `catalog.json`，再往本仓库 `index.json` 的 `sources` 加一项。
3. 推送后即时生效；玩家打开「下载模组」页签就会看到（下载后默认禁用，需在已安装列表手动启用）。

玩家也可以在商店里点「添加源」，粘贴任意 `catalog.json` 直链作为个人自定义源（只存本地）。
