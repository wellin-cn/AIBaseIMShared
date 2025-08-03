# Server端开发指南

## 项目概述

这是IM聊天应用的服务端，基于Node.js + Express + Socket.io构建，提供实时消息服务和数据持久化。

## 技术栈

- **Node.js**: ^20.10.0 (LTS)
- **Express**: ^4.18.0 - Web框架
- **Socket.io**: ^4.7.0 - WebSocket服务
- **TypeScript**: ^5.3.0 - 类型安全
- **SQLite**: ^3.40.0 - 轻量级数据库
- **Multer**: ^1.4.5 - 文件上传

## 项目结构

```
server/
├── src/
│   ├── app.ts                  # Express应用配置
│   ├── server.ts               # 服务器启动入口
│   ├── controllers/            # 路由控制器
│   ├── services/               # 业务逻辑层
│   ├── models/                 # 数据模型
│   ├── middleware/             # Express中间件
│   ├── database/               # 数据库相关
│   ├── types/                  # TypeScript类型
│   ├── utils/                  # 工具函数
│   └── config/                 # 配置文件
├── uploads/                    # 文件上传目录
├── data/                       # SQLite数据库文件
├── logs/                       # 日志文件
└── package.json
```

## 核心功能模块

### 1. Socket服务 (services/socket-service.ts)
- WebSocket连接管理
- 用户加入/离开处理
- 消息广播
- 房间管理

### 2. 用户服务 (services/user-service.ts)
- 在线用户管理
- 用户名验证
- 用户状态追踪

### 3. 消息服务 (services/message-service.ts)
- 消息存储和查询
- 消息历史记录
- 消息格式验证

### 4. 文件服务 (services/file-service.ts)
- 文件上传处理
- 文件类型验证
- 文件清理

## 数据模型

### User
```typescript
interface User {
  id: string              // 唯一标识
  socketId: string        // Socket连接ID
  username: string        // 用户名
  joinedAt: Date         // 加入时间
  lastSeen: Date         // 最后活跃时间
  isOnline: boolean      // 在线状态
}
```

### Message
```typescript
interface Message {
  id: string              // 消息ID
  type: 'text' | 'file' | 'system'  // 消息类型
  content: string         // 消息内容
  sender: string          // 发送者ID
  timestamp: Date         // 发送时间
  fileUrl?: string        // 文件URL（如果是文件消息）
  fileName?: string       // 文件名
  fileSize?: number       // 文件大小
}
```

## API接口

### REST API
- `GET /api/health` - 健康检查
- `GET /api/messages` - 获取历史消息
- `POST /api/upload` - 文件上传
- `GET /api/files/:filename` - 文件下载

### Socket.io事件
- `user:join` - 用户加入
- `user:leave` - 用户离开
- `message:send` - 发送消息
- `message:receive` - 接收消息
- `users:update` - 用户列表更新

## 环境配置

### 开发环境
```env
NODE_ENV=development
PORT=3001
DATABASE_PATH=./data/app.db
UPLOAD_DIR=./uploads
MAX_FILE_SIZE=10485760
CORS_ORIGIN=*
LOG_LEVEL=debug
```

### 启动脚本
```bash
# 开发模式
npm run dev

# 构建
npm run build

# 生产模式
npm start

# 测试
npm test
```

## 开发流程

### 1. 环境准备
1. 安装Node.js 20+
2. 克隆项目并安装依赖
3. 创建必要的目录
4. 配置环境变量

### 2. 数据库初始化
1. 创建SQLite数据库文件
2. 运行数据表创建脚本
3. 插入初始数据（如需要）

### 3. 开发调试
1. 使用`npm run dev`启动开发服务器
2. 通过Postman或客户端测试API
3. 查看控制台日志和错误信息

### 4. 部署准备
1. 运行`npm run build`构建项目
2. 配置生产环境变量
3. 使用PM2启动生产服务

## 注意事项

### 性能考虑
- 限制同时在线用户数量
- 实现消息队列避免阻塞
- 定期清理过期文件和日志
- 数据库查询优化

### 安全性
- 输入数据验证和清理
- 文件上传类型和大小限制
- 防止SQL注入攻击
- 适当的错误信息返回

### 监控和日志
- 记录关键操作日志
- 监控服务器性能指标
- 实现健康检查接口
- 错误报警机制

## 故障排除

### 常见问题
1. **端口占用**: 检查3001端口是否被其他进程占用
2. **数据库连接失败**: 检查SQLite文件权限和路径
3. **Socket连接问题**: 检查CORS配置和客户端连接URL
4. **文件上传失败**: 检查uploads目录权限和磁盘空间

### 调试工具
- 使用`console.log`和winston记录日志
- Chrome DevTools的Network面板检查Socket连接
- Postman测试REST API接口
- SQLite客户端检查数据库内容

## 下一步开发

根据knowledge/features/中的需求文档：
1. 先实现Phase 1的基础聊天功能
2. 完成基础的Socket服务和数据存储
3. 实现简单的用户管理
4. 逐步添加文件传输等高级功能

---

**重要**: 开发时请参考根目录下的knowledge/目录中的详细技术文档和需求说明。