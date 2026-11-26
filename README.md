# xmfu Mobile OTA 更新源

xmfu 手机 App 的纯静态 OTA 热更新源仓库，面向 **EdgeOne Pages + Expo Updates Protocol v1**。

## EdgeOne Pages 绑定参数

- **构建命令（Build Command）**：留空
- **输出目录（Output Directory）**：`public`
- **响应头配置**：根目录 [edgeone.json](./edgeone.json) 已声明协议头与缓存策略

## 目录约定

```text
public/
  health.json                                      # 健康检查端点
  index.html                                       # 静态占位页
  robots.txt                                       # 禁止搜索引擎索引
  ota/
    manifests/<platform>-<runtimeVersion>.json     # 更新清单（例如 android-1.0.3.json）
    bundles/<hash>.hbc                             # 编译后的 Hermes JS Bundle
    assets/<hash>                                  # 静态资源文件
```

## `edgeone.json` 响应头说明

- `/ota/manifests/*`：附带 `expo-protocol-version: 1`、`expo-sfv-version: 0` 与 `Cache-Control: no-cache, no-store, must-revalidate`，确保客户端实时读取最新清单并符合 Expo Updates Protocol v1 校验要求。
- `/ota/bundles/*` 与 `/ota/assets/*`：按内容哈希命名，配置 `Cache-Control: public, max-age=31536000, immutable` 长期缓存。
- `/health.json`：配置 `Cache-Control: no-cache`。

## 安全规范

- 本仓库为 **公开仓库**，仅用于存放 OTA 更新源的纯静态打包产物与公开配置。
- **严禁**在本仓库提交任何代码签名私钥（`*.pem`、`*.key`、`*.p12`、`*.jks`、`*.keystore`）、API Token、环境变量文件（`.env*`）或项目业务源码。
