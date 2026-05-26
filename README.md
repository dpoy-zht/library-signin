# 📚 SDUFE 图书馆签到系统

> 山东财经大学图书馆远程签到/暂离/签离工具

[![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-在线-green)](https://dpoy-zht.github.io/library-signin/)
[![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)
[![Stars](https://img.shields.io/github/stars/dpoy-zht/library-signin)](https://github.com/dpoy-zht/library-signin)

---

## ✨ 功能特性

- ✅ **签到入馆** - 远程完成入馆签到
- ⏰ **临时离馆** - 快捷办理临时离馆（100分钟内）
- 🚪 **签离出馆** - 快速完成离馆签离
- 🔒 **密码保护** - 防止API被滥用，保护服务稳定性
- 📱 **移动端优化** - 完美适配微信内置浏览器
- 🎨 **优雅界面** - 渐变色彩，流畅动画

---

## 🔒 密码保护

为了保护 API 服务不被滥用，本项目添加了**密码保护功能**。

### 🔑 默认密码

```
联系作者并v作者50，还要学奶龙大笑并且唱念张师即可获得密码。
```

### 📱 使用流程

1. **首次访问** → 弹出密码框，输入密码
2. **密码正确** → 显示"已记住授权，下次无需重新输入"
3. **以后访问** → 自动跳过密码验证
4. **清除授权** → 清除微信浏览器缓存，或执行 `localStorage.removeItem('library_auth')`

---

## 🚀 在线使用

### 🌐 访问地址

```
https://dpoy-zht.github.io/library-signin/
```

### 📱 微信中使用（推荐）

1. 在微信中打开上面的链接
2. 点击右上角 `...` → `收藏`
3. 以后从 **我 → 收藏** 随时打开
4. 可选：用浏览器打开后添加到主屏幕（像 App 一样使用）

---

## 🔧 技术原理

### 📖 核心思路

图书馆签到需要扫描二维码，而二维码本质上是一个 **URL + 参数**。通过破解 URL 结构，我们可以远程生成有效的签到链接。

### 🔍 实现步骤

#### 1️⃣ 解码二维码

扫码后得到 URL 结构：
```
http://www.skalibrary.net/wx/scancheck?school=sdufe&type=1&t=51526269564
```

**参数说明：**
| 参数 | 含义 | 值 |
|------|------|-----|
| `school` | 学校标识 | `sdufe` |
| `type` | 操作类型 | `1`=签到，`2`=暂离，`3`=签离 |
| `t` | 服务器时间戳 | 动态变化 |

#### 2️⃣ 破解时间戳 `t`

通过观察发现：
- `t` 不是标准 Unix 时间戳
- 是图书馆服务器**自定义的时间表示**
- 必须从服务器实时获取

#### 3️⃣ 发现 API 接口

通过微信内置浏览器调试，找到时间戳接口：
```
https://libst.sdufe.edu.cn/api.php/v3qrtime
```

**返回数据：**
```json
{
  "data": 51527920447,
  "msg": "服务器时间",
  "status": 1
}
```

#### 4️⃣ 动态生成签到链接

```javascript
// 1. 获取服务器时间戳
fetch('https://libst.sdufe.edu.cn/api.php/v3qrtime')
  .then(res => res.json())
  .then(data => {
    const timestamp = data.data;
    
    // 2. 拼接完整 URL
    const url = `http://www.skalibrary.net/wx/scancheck?school=sdufe&type=1&t=${timestamp}`;
    
    // 3. 跳转签到
    window.location.href = url;
  });
```

---

## 🏗️ 项目结构

```
library-signin/
├── index.html          # 主页面（纯前端实现）
├── README.md           # 项目文档
├── deploy.sh          # 部署脚本
└── assets/            # 资源文件（如有）
```

---

## ⚠️ 使用须知

### 📋 规则提醒

- ⏰ **暂离时间上限**：100 分钟（中午除外）
- 🚫 **违约后果**：累计 3 次违约将被禁止入馆 3 天
- 📶 **使用建议**：合理安排时间，避免违规

### 🔒 安全提示

- ✅ 本工具仅供**个人学习研究**使用
- ✅ 请遵守图书馆相关规定
- ✅ 不得用于任何商业或非法用途
- ✅ 使用本工具产生的一切后果由使用者自行承担
- ⚠️ **密码保护**：请勿将密码分享给不信任的人
- ⚠️ **API 调用**：避免频繁请求，防止被图书馆服务器封禁
- ⚠️ **账号安全**：别人无法使用你的账号签到（微信登录状态隔离）



## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request！

### 贡献步骤

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 提交 Pull Request

---

## 📄 开源协议

本项目采用 **MIT 协议** 开源。

Copyright © 2026 dpoy-zht

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction...

---

## 🙏 致谢

- 感谢山东财经大学提供的图书馆服务
- 感谢所有为这个项目提出建议的同学

---

## 📮 联系方式

- GitHub: [@dpoy-zht](https://github.com/dpoy-zht)
- 项目 Issues: [提交问题](https://github.com/dpoy-zht/library-signin/issues)

---

## 🌟 支持项目

如果这个项目对你有帮助，请给我一个 ⭐️ Star！

你的支持是我持续更新的动力 💪

---

<div align="center">

**⚡ 让技术为生活带来便利 ⚡**

Made with ❤️ by [dpoy-zht](https://github.com/dpoy-zht)

</div>
