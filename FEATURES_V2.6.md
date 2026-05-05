# V2.6 版本新功能说明

## 新增功能

### 1. IP 备注信息增强

现在 IP 列表中的每个 IP 都会显示详细的备注信息，格式为：`延迟 ms(数据源 - 线路类型)`

**示例**：
- `1.2.3.4#36ms(164746-电信)`
- `5.6.7.8#67ms(hostmonit-移动)`

**复制功能**：
- 点击"复制"按钮会复制带有完整备注信息的 IP 地址
- 点击"复制优质 IP"会复制所有优质 IP，每行格式为：`IP#延迟 ms(数据源 - 线路类型)`

### 2. 数据源管理功能

管理员可以通过 Web 界面自定义管理数据源。

**功能特性**：
- ➕ 添加新的数据源 URL
- ✏️ 编辑现有数据源的 URL、名称和线路类型
- 🗑️ 删除不需要的数据源
- 👁️/🚫 启用或禁用数据源
- 💾 保存配置后立即生效（下次更新 IP 时使用新配置）

**线路类型**：
- 电信
- 联通
- 移动

**访问方式**：
1. 登录管理员账号
2. 点击"🌐 数据源管理"按钮
3. 在弹出的模态框中进行配置
4. 点击"保存配置"按钮

### 3. 数据结构变化

#### KV 存储新字段

`cloudflare_ips` 现在包含：
```javascript
{
  ips: ["1.2.3.4", "5.6.7.8", ...],
  ipSources: {
    "1.2.3.4": [
      { name: "164746", type: "电信" },
      { name: "uouin", type: "电信" }
    ],
    "5.6.7.8": [
      { name: "hostmonit", type: "移动" }
    ]
  },
  lastUpdated: "2026-05-05T12:00:00.000Z",
  count: 100,
  sources: [...]
}
```

`cloudflare_fast_ips` 现在包含：
```javascript
{
  fastIPs: [
    {
      ip: "1.2.3.4",
      latency: 36,
      sources: [
        { name: "164746", type: "电信" }
      ]
    },
    ...
  ],
  lastTested: "2026-05-05T12:00:00.000Z",
  count: 25,
  testedCount: 200,
  totalIPs: 1000
}
```

## API 端点

### 新增 API

#### GET /sources
获取当前配置的数据源列表

**请求**：
```
GET /sources?session=xxx 或 ?token=xxx
```

**响应**：
```json
{
  "sources": [
    {
      "url": "https://ip.164746.xyz",
      "name": "164746",
      "type": "电信",
      "enabled": true
    }
  ]
}
```

#### POST /sources
保存自定义数据源配置

**请求**：
```
POST /sources
Content-Type: application/json
Authorization: Bearer {sessionId} 或 Token {token}

{
  "sources": [
    {
      "url": "https://example.com/ips",
      "name": "example",
      "type": "电信",
      "enabled": true
    }
  ]
}
```

**响应**：
```json
{
  "success": true,
  "sources": [...],
  "message": "数据源配置已保存"
}
```

#### GET /raw-ips-with-sources
获取带有来源信息的原始 IP 数据

**请求**：
```
GET /raw-ips-with-sources?session=xxx 或 ?token=xxx
```

**响应**：
```json
{
  "ips": ["1.2.3.4", "5.6.7.8", ...],
  "ipSources": {
    "1.2.3.4": [{"name": "164746", "type": "电信"}]
  },
  "count: 100,
  ...
}
```

## 默认数据源

系统预置了以下数据源：

| 名称 | URL | 线路类型 |
|------|-----|---------|
| 164746 | https://ip.164746.xyz | 电信 |
| haogege | https://ip.haogege.xyz/ | 联通 |
| hostmonit | https://stock.hostmonit.com/CloudFlareYes | 移动 |
| uouin | https://api.uouin.com/cloudflare.html | 电信 |
| 090227 | https://addressesapi.090227.xyz/CloudFlareYes | 联通 |
| 164746-api | https://addressesapi.090227.xyz/ip.164746.xyz | 电信 |
| wetest | https://www.wetest.vip/page/cloudflare/address_v4.html | 移动 |

## 使用建议

1. **首次使用**：建议先使用默认数据源更新一次 IP，确保系统正常工作
2. **自定义数据源**：如果您有自己的 IP 来源，可以通过数据源管理功能添加
3. **优化测速**：禁用来历不明或质量较差的数据源可以提高优质 IP 的整体质量
4. **备份配置**：重要的数据源配置建议做好备份

## 兼容性说明

- 新版本完全向后兼容旧版本的 KV 数据结构
- 如果没有配置自定义数据源，系统会自动使用默认数据源
- 旧的 IP 数据在读取时会自动适配新的数据结构

## 升级步骤

1. 将新的 `_workers.js` 文件部署到 Cloudflare Workers
2. 登录管理员账号
3. （可选）通过"数据源管理"功能自定义数据源配置
4. 点击"立即更新"按钮更新 IP 列表
5. 查看新的 IP 列表，验证备注信息是否正确显示
