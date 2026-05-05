# 快速参考指南 - V2.6

## IP 备注格式

```
IP#延迟 ms(数据源 - 线路类型)
```

**示例**:
```
1.2.3.4#36ms(164746-电信)
5.6.7.8#67ms(hostmonit-移动)
```

## 数据源管理

### 访问路径
```
登录 → 点击"🌐 数据源管理" → 配置 → 保存
```

### 数据源配置项
- **URL**: 数据源地址
- **名称**: 简短标识（用于显示）
- **类型**: 电信/联通/移动
- **启用**: 开/关

### 常用操作

#### 添加数据源
1. 点击"➕ 添加数据源"
2. 填写 URL、名称、类型
3. 确保"启用"开关打开
4. 点击"💾 保存配置"

#### 删除数据源
1. 找到要删除的数据源
2. 点击"🗑️"按钮
3. 确认删除
4. 点击"💾 保存配置"

#### 启用/禁用
- 点击"👁️"或"🚫"按钮切换状态
- 点击"💾 保存配置"

## API 快速参考

### 获取数据源
```bash
curl 'https://your-worker.workers.dev/sources?token=YOUR_TOKEN'
```

### 保存数据源
```bash
curl -X POST 'https://your-worker.workers.dev/sources' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Token YOUR_TOKEN' \
  -d '{
    "sources": [
      {
        "url": "https://example.com/ips",
        "name": "example",
        "type": "电信",
        "enabled": true
      }
    ]
  }'
```

## 默认数据源

```javascript
[
  { url: 'https://ip.164746.xyz', name: '164746', type: '电信' },
  { url: 'https://ip.haogege.xyz/', name: 'haogege', type: '联通' },
  { url: 'https://stock.hostmonit.com/CloudFlareYes', name: 'hostmonit', type: '移动' },
  { url: 'https://api.uouin.com/cloudflare.html', name: 'uouin', type: '电信' },
  { url: 'https://addressesapi.090227.xyz/CloudFlareYes', name: '090227', type: '联通' },
  { url: 'https://addressesapi.090227.xyz/ip.164746.xyz', name: '164746-api', type: '电信' },
  { url: 'https://www.wetest.vip/page/cloudflare/address_v4.html', name: 'wetest', type: '移动' }
]
```

## 复制功能

### 单个 IP
点击 IP 右侧的"复制"按钮 → 复制格式：`IP#延迟 ms(数据源 - 线路)`

### 全部优质 IP
点击"📋 复制优质 IP" → 每行格式：`IP#延迟 ms(数据源 - 线路)`

## 故障排查

### 问题：数据源管理按钮是灰色的
**解决**: 需要先登录管理员账号

### 问题：保存数据源失败
**解决**: 
1. 检查是否已登录
2. 验证 Token 是否过期
3. 检查 URL 格式是否正确

### 问题：IP 备注不显示
**解决**:
1. 点击"立即更新"重新收集 IP
2. 检查 KV 中是否有 `ipSources` 数据
3. 刷新浏览器页面

### 问题：复制的 IP 没有备注
**解决**: 确保数据已经更新到 V2.6 格式，旧的 IP 数据可能不包含来源信息

---

**提示**: 更多详细信息请查看 `FEATURES_V2.6.md` 和 `CHANGELOG_V2.6.md`
