# Gitea Mirror - LazyCat App

[Gitea Mirror](https://github.com/RayLabsHQ/gitea-mirror)（自动将 GitHub 仓库同步到自托管 Gitea/Forgejo）的懒猫微服（LazyCat）打包。

## 模式

- **镜像模式（mirror）**：`ghcr.io/raylabshq/gitea-mirror` 镜像经 `ghcr.1ms.run` 加速器拉取，`require_digest_match: true` 校验镜像内容与上游一致。
- **自动跟随上游**：每日检查上游 semver tag（`channel: stable`），发现新版本自动更新版本号与镜像、构建 LPK、发布商店。
- **喵喵商店发布**：仅私有商店（MiaoMiao private store），不发布官方商店。

## 配置

- SQLite 存储（`DATABASE_URL=file:data/gitea-mirror.db`），数据卷 `/lzcapp/var/gitea-mirror:/app/data`
- 反向代理三件套（`BETTER_AUTH_URL` / `PUBLIC_BETTER_AUTH_URL` / `BETTER_AUTH_TRUSTED_ORIGINS`）自动指向子域名
- `BETTER_AUTH_SECRET`、`ENCRYPTION_SECRET` 用 `stable_secret` 自动生成（后者固定保证 GitHub Token 加密在重建后仍可解密）
- GitHub Token 等在 Web UI 首次设置向导中填写

## 结构

| 文件 | 说明 |
| --- | --- |
| `package.yml` | 包元数据（`community.lazycat.app.gitea-mirror`） |
| `lzc-manifest.yml` | 服务与路由配置 |
| `lzc-build.yml` | 构建配置 |
| `icon.png` | 图标（上游 apple-touch-icon） |
| `.github/lazycat-action.yml` | [lazycat-github-action](https://github.com/ca-x/lazycat-github-action) 配置 |
| `.github/workflows/lazycat.yml` | 构建 + 发布工作流 |

## 所需 Secrets

| Secret | 说明 |
| --- | --- |
| `APPSTORE_URL` / `APPSTORE_TOKEN` | 喵喵商店 API 地址与发布令牌 |
| `APP_ID` | 可选 |
| `PRIVATE_STORE_GROUP_CODES` | 可选，私有分组码 |

## 许可

AGPL-3.0（上游）
