# Shared Library 使用指南 - Server端

## 📖 重要说明

**server/shared/** 目录是一个 **Git Subtree**，指向独立的共享代码库。这个目录包含了前后端共享的类型定义、常量、验证器和工具函数。

⚠️ **重要**：不要直接在 `server/shared/` 目录中修改文件，所有修改都应该在主共享库中进行，然后通过 git subtree 同步。

## 🔗 Shared库概述

### 目录结构
```
server/shared/                   # Git Subtree 目录
├── README.md                    # 库使用指南
├── CHANGELOG.md                 # 版本变更记录
├── types/                       # TypeScript类型定义
│   ├── index.ts                 # 统一导出
│   ├── user.ts                  # 用户相关类型
│   ├── message.ts               # 消息相关类型
│   ├── socket.ts                # Socket事件类型
│   └── api.ts                   # API接口类型
├── constants/                   # 常量定义
│   ├── index.ts                 # 统一导出
│   ├── events.ts                # Socket事件名称
│   ├── errors.ts                # 错误码定义
│   └── config.ts                # 配置常量
├── validators/                  # 数据验证函数
│   ├── index.ts                 # 统一导出
│   ├── user.ts                  # 用户验证
│   └── message.ts              # 消息验证
├── utils/                       # 工具函数
│   ├── index.ts                 # 统一导出
│   ├── format.ts                # 格式化工具
│   └── validate.ts              # 验证工具
└── docs/                        # 详细文档
    ├── api-spec.md              # API接口规范
    ├── socket-events.md         # Socket事件文档
    └── data-models.md           # 数据模型文档
```

## 🚀 在Server端使用Shared库

### 1. TypeScript配置
在 `tsconfig.json` 中配置路径别名：
```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@shared/*": ["./shared/*"]
    }
  }
}
```

### 2. 导入示例
```typescript
// 导入类型定义
import { 
  User, 
  Message, 
  ApiResponse,
  MessageSendData 
} from '@shared/types'

// 导入常量
import { 
  USER_EVENTS, 
  MESSAGE_EVENTS, 
  ERROR_CODES,
  MAX_MESSAGE_LENGTH 
} from '@shared/constants'

// 导入验证器
import { 
  validateUsername, 
  validateMessageSendData 
} from '@shared/validators'

// 导入工具函数
import { 
  generateUserId, 
  formatTimestamp,
  isValidUsername 
} from '@shared/utils'
```

### 3. 常用使用场景

#### Socket事件处理
```typescript
import { USER_EVENTS, MESSAGE_EVENTS } from '@shared/constants'
import { UserJoinData, MessageSendData } from '@shared/types'

// 用户加入事件
socket.on(USER_EVENTS.JOIN, (data: UserJoinData) => {
  // 处理用户加入
})

// 消息发送事件
socket.on(MESSAGE_EVENTS.SEND, (data: MessageSendData) => {
  // 处理消息发送
})
```

#### 数据验证
```typescript
import { validateUserJoinData, validateMessageSendData } from '@shared/validators'
import { ERROR_CODES } from '@shared/constants'

// 验证用户加入数据
const userValidation = validateUserJoinData(userData)
if (!userValidation.isValid) {
  socket.emit('user:join:error', {
    code: ERROR_CODES.VALIDATION_ERROR,
    message: userValidation.errors[0]
  })
  return
}

// 验证消息数据
const messageValidation = validateMessageSendData(messageData)
if (!messageValidation.isValid) {
  socket.emit('message:send:error', {
    code: ERROR_CODES.MESSAGE_INVALID_TYPE,
    message: messageValidation.errors[0]
  })
  return
}
```

#### API响应格式
```typescript
import { ApiResponse } from '@shared/types'
import { ERROR_CODES } from '@shared/constants'

// 成功响应
const successResponse: ApiResponse<Message[]> = {
  success: true,
  data: messages,
  timestamp: Date.now()
}

// 错误响应
const errorResponse: ApiResponse = {
  success: false,
  error: {
    code: ERROR_CODES.MESSAGE_NOT_FOUND,
    message: 'Message not found'
  },
  timestamp: Date.now()
}

res.json(successResponse)
```

## 📋 开发规范

### 1. 依赖管理
- **不要修改** shared目录中的文件
- 如需修改共享内容，请联系架构师在主库中修改
- 定期同步最新的shared库版本

### 2. 类型安全
- 始终使用shared库中定义的类型
- 不要在server端重复定义已有类型
- 利用TypeScript编译器检查类型一致性

### 3. 错误处理
- 使用shared库中定义的错误码
- 保持错误消息的一致性
- 遵循错误处理的最佳实践

### 4. 常量使用
- 使用shared库中的事件名称常量
- 使用shared库中的配置常量
- 避免硬编码字符串和数字

## 🔄 Git Subtree 操作

### 查看当前版本
```bash
cd server/
git log --oneline shared/ | head -5
```

### 更新shared库（由架构师执行）
```bash
# 拉取最新的shared库更新
git subtree pull --prefix=shared --squash shared-repo main

# 解决冲突（如有）
git add .
git commit -m "chore: update shared library"
```

### 检查shared库状态
```bash
# 查看shared目录的提交历史
git log --oneline --graph shared/

# 检查shared库是否是最新版本
git subtree push --prefix=shared --dry-run shared-repo main
```

## 📚 参考文档

1. **API规范**: `shared/docs/api-spec.md`
2. **Socket事件**: `shared/docs/socket-events.md`
3. **数据模型**: `shared/docs/data-models.md`
4. **变更日志**: `shared/CHANGELOG.md`

## ⚠️ 注意事项

1. **不要直接修改shared目录**：所有修改都通过主库进行
2. **及时同步更新**：定期检查shared库的更新
3. **保持兼容性**：确保server代码与shared库版本兼容
4. **遵循规范**：严格按照shared库的使用规范开发

## 🆘 常见问题

**Q: 如何知道shared库有更新？**
A: 查看 `shared/CHANGELOG.md` 或联系架构师

**Q: 发现shared库中的bug怎么办？**
A: 立即报告给架构师，不要直接修改shared目录

**Q: 需要新增共享功能怎么办？**
A: 向架构师提出需求，在主库中统一添加

**Q: shared库版本冲突怎么解决？**
A: 联系架构师协助解决git subtree冲突

---

**记住**: Shared库是前后端一致性的保证，请严格遵循使用规范！