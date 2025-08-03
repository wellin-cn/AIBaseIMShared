# Shared Library - IM聊天系统公共库

## 📖 概述

这是IM聊天系统的共享代码库，包含前后端通用的类型定义、接口规范、工具函数等。通过软链接的方式同步到server和client项目中。

## 📁 目录结构

```
shared/
├── README.md                    # 本文件
├── CHANGELOG.md                 # 变更日志
├── types/                       # TypeScript类型定义
│   ├── index.ts                 # 导出所有类型
│   ├── user.ts                  # 用户相关类型
│   ├── message.ts               # 消息相关类型
│   ├── socket.ts                # Socket事件类型
│   └── api.ts                   # API接口类型
├── constants/                   # 常量定义
│   ├── index.ts                 # 导出所有常量
│   ├── events.ts                # Socket事件常量
│   ├── errors.ts                # 错误码常量
│   └── config.ts                # 配置常量
├── validators/                  # 数据验证规则
│   ├── index.ts                 # 导出所有验证器
│   ├── user.ts                  # 用户数据验证
│   └── message.ts              # 消息数据验证
├── utils/                       # 工具函数
│   ├── index.ts                 # 导出所有工具函数
│   ├── format.ts                # 格式化工具
│   └── validate.ts              # 验证工具
└── docs/                        # 文档
    ├── api-spec.md              # API接口规范
    ├── socket-events.md         # Socket事件文档
    └── data-models.md           # 数据模型文档
```

## 🔧 使用方式

### 在Server项目中使用
```typescript
import { User, Message, SocketEvents } from '@shared/types'
import { USER_EVENTS, ERROR_CODES } from '@shared/constants'
import { validateUserName } from '@shared/validators'
```

### 在Client项目中使用
```typescript
import { User, Message, ApiResponse } from '@shared/types'
import { SOCKET_EVENTS, MAX_MESSAGE_LENGTH } from '@shared/constants'
import { formatTimestamp } from '@shared/utils'
```

## 📋 开发规范

### 类型定义规范
- 使用PascalCase命名接口和类型
- 所有接口必须导出并在index.ts中重新导出
- 添加详细的JSDoc注释

### 常量定义规范
- 使用UPPER_SNAKE_CASE命名常量
- 按功能模块分组织
- 提供默认值和说明注释

### 变更管理
- 任何接口变更必须更新CHANGELOG.md
- 破坏性变更需要版本号升级
- 新增功能需要添加对应的文档

## 🚀 版本管理

- **主版本号**：破坏性变更
- **次版本号**：新增功能，向后兼容
- **修订版本号**：Bug修复

## 📝 贡献指南

1. 在shared目录进行修改
2. 更新相关文档
3. 记录CHANGELOG
4. 确保两端项目都能正常编译
5. 提交变更并推送

---

**注意**：此库的变更会同时影响server和client项目，请谨慎操作并充分测试。