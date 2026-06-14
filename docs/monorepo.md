# Poprako Monorepo 使用说明

## 目录结构

```text
poprako/
├── web/                 # 前端（Vue 3 + Electron）
├── backend-v2-dotnet/   # .NET 后端
├── backend-v3-rust/     # Rust 后端（默认）
├── docs/                # 仓库级文档
└── design-system/       # 设计规范
```

## 首次克隆

```bash
git clone --recursive <monorepo-url> poprako
cd poprako
```

已克隆但未拉取子模块：

```bash
git submodule update --init --recursive
```

## 子模块仓库

| 路径 | 远程仓库 |
|------|----------|
| `web` | https://github.com/SeaAndStars/poprako-web.git |
| `backend-v2-dotnet` | https://github.com/SeaAndStars/backend-v2-dotnet.git |
| `backend-v3-rust` | https://github.com/SeaAndStars/backend-v3-rust.git |

## 日常开发

### 前端

```bash
cd web
pnpm install
pnpm dev
```

### Rust 后端（默认）

```bash
cd backend-v3-rust
cp .env.sample .env
cargo run -p poprako-migration
cargo run -p poprako-api
```

### .NET 后端

```bash
cd backend-v2-dotnet
dotnet build Poprako.Backend.slnx
```

本地前端通过 `web/.env.development` 中 `VITE_API_PROXY_TARGET=http://127.0.0.1:18880` 代理到 Rust 后端。

## 子模块更新

拉取 monorepo 与子模块最新提交：

```bash
git pull
git submodule update --init --recursive
```

在子模块内开发并回写指针：

```bash
cd backend-v3-rust
git checkout dev
# ... 修改并提交 ...
git push origin dev

cd ..
git add backend-v3-rust
git commit -m "chore: bump backend-v3-rust"
```

## 注意事项

- 不要在 monorepo 根目录直接修改子模块文件却不更新子模块提交指针。
- 若迁移后残留空 `backend/` 目录且无法删除，先结束占用 `poprako-api.exe` 的进程，再手动删除。
