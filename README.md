# 跑起来 · 共享核心

[English](README.en.md) · 中文

`@paobu/core` 是跑起来主站、知识站和工具站共用的共享核心仓库：设计 tokens、基础布局、导航、页头页脚、主题、语言切换、路由 helper、SEO/JSON-LD、PWA/offline shell、内容契约与发布 helper。

它不包含任何站点的业务页面，也不包含 Cesium 地球、路线几何渲染或工具计算实现。三个站点各自构建，通过版本化 tgz 消费本仓库。

## 范围

放入本仓库：

- 设计 tokens、字体、颜色、间距、基础样式
- 通用布局、导航、页头页脚、主题与语言切换
- i18n、路由前缀、SEO/JSON-LD 生成
- PWA/offline shell、release manifest、hosting 文件生成
- 通用按钮、卡片、表格、搜索壳与内容校验契约
- 内容加载、digest、审阅与来源校验类型
- 测地距离与轨迹简化等通用地理计算 helper

不放入本仓库：

- Cesium 地球、3D 场景、卫星贴图
- 路线详情模板、路线几何渲染、赛事内容
- 工具计算器交互与具体计算实现
- 任一站点首页、栏目页或搜索业务

## 安装与打包

```bash
pnpm install
node scripts/pack-core.mjs
```

`pack-core.mjs` 打包并写出：

```text
core-dist/paobu-core-0.3.0.tgz
core-dist/paobu-core-0.3.0.tgz.sha256
core-dist/core.lock.json
```

直接 `pnpm pack` 只会按 npm 包规则产出 tgz，不会补齐 `core-dist` 的 `.sha256` 与 `core.lock.json`；生成可发布制品必须用 `node scripts/pack-core.mjs`。三个站点从 `core-dist/core.lock.json` 读取版本与 SHA-256，锁定同一个制品。

## 发版

1. 修改 `package.json` 的版本号。
2. 运行 `node scripts/pack-core.mjs`。
3. 打 tag `core-v0.3.0`，发布同名 GitHub Release，上传 tgz、sha256 与 `core.lock.json`。工作流定义在 `.github/workflows/release.yml`。
4. 三个站点运行 `node scripts/sync-core.mjs` 更新各自 `core.lock.json`，重新安装并构建。
5. 在发布包中检查三站使用同一 `version` 与 `sha256`。

本地阶段不需要 GitHub Release：三个站点通过 `scripts/sync-core.mjs` 从 `../paobu-core/core-dist` 复制制品。

## 兼容性

- 包名保持 `@paobu/core`。
- 公开导出保持稳定；删除或重命名导出需要升 minor/major 版本。
- 站点只能通过 `@paobu/core/...` 导出消费，不复制 `src/`。
- 本仓库不保存站点业务页面。

## 许可

MIT。见 [LICENSE](LICENSE)。
