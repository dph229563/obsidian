# Claude Code 代理方案总结

下面是专门针对 Mac + Clash Verge + Claude Code + Claude Pro 环境的代理方案总结。

---

## 方案一：TUN 模式（推荐 ⭐⭐⭐⭐⭐）

### 配置

```
Clash Verge
├── 系统代理：开启
├── TUN模式：开启
└── 规则模式：开启
```

### 优点

所有程序自动走代理，无需配置环境变量：

- Claude Code
- curl
- git
- npm / pnpm
- Docker
- VS Code 插件
- Chrome / Safari

### 验证

```bash
curl https://ipinfo.io/json
```

返回以下内容说明终端已经走代理：

```json
{
  "country": "JP"
}
```

或

```json
{
  "country": "SG"
}
```

### 适用场景

- 长期使用 Claude Code
- Next.js / React Native 开发
- GitHub、npm、Docker 用户

---

## 方案二：系统代理 + 环境变量

### 配置

**Clash Verge：**

- 系统代理：开启
- TUN模式：关闭

**终端：**

```bash
export HTTP_PROXY=http://127.0.0.1:7897
export HTTPS_PROXY=http://127.0.0.1:7897
```

> ⚠️ 注意：`7897` 请以你 Clash Verge 实际 HTTP 端口为准。

### 验证

```bash
curl https://ipinfo.io/json
```

返回 `"country": "JP"` 说明生效。

### 缺点

每次新终端都要重新设置。

---

## 方案三：永久环境变量

### 配置

编辑配置文件：

```bash
nano ~/.zshrc
```

追加以下内容：

```bash
export HTTP_PROXY=http://127.0.0.1:7897
export HTTPS_PROXY=http://127.0.0.1:7897
```

使配置生效：

```bash
source ~/.zshrc
```

以后所有终端自动走代理。

---

## 方案四：代理开关（推荐不用 TUN 时使用）

### 配置

编辑配置文件：

```bash
nano ~/.zshrc
```

添加代理开关函数：

```bash
proxyon() {
  export HTTP_PROXY=http://127.0.0.1:7897
  export HTTPS_PROXY=http://127.0.0.1:7897
  echo "Proxy ON"
}

proxyoff() {
  unset HTTP_PROXY
  unset HTTPS_PROXY
  echo "Proxy OFF"
}
```

加载配置：

```bash
source ~/.zshrc
```

### 使用方法

| 命令 | 作用 |
|------|------|
| `proxyon` | 开启代理 |
| `proxyoff` | 关闭代理 |
| `env \| grep -i proxy` | 查看状态 |

---

## 如何验证 Claude Code 是否能正常使用

### 检查出口 IP

```bash
curl https://ipinfo.io/json
```

### 结果判断

| 返回值 | 说明 |
|--------|------|
| `"country": "JP"` 或 `"SG"` | Claude Code 可以正常访问 |
| `"country": "CN"` | 可能出现登录问题 |

如果看到 `"country": "CN"`，执行：

```bash
claude auth login
```

可能出现错误：

```
403
app-unavailable-in-region
```

---

## 最佳实践（推荐）

### 当前环境

- Mac
- Clash Verge
- Claude Pro
- Claude Code
- CCSwitch
- Next.js / React Native

### 推荐长期保持

- ✓ 系统代理
- ✓ TUN 模式
- ✓ 规则模式

### 这样配置的好处

- Claude Official 可正常使用
- 百炼可正常使用
- CCSwitch 可随时切换
- 不需要配置任何终端代理变量
- 不会再出现 Claude Code 登录 403 的问题

> 💡 **结论**：这是目前最稳定、最省心的方案。