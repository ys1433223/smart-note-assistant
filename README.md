# 🎓 智能笔记助手 (Smart Note Assistant)

一个基于AI的智能学习工具，帮助学生快速分析笔记、提取关键知识点、自动生成复习题。

## ✨ 功能特性

### 核心功能
- 📝 **智能笔记分析** - 自动提取关键词和知识点
- 🧠 **AI生成复习题** - 基于笔记内容自动生成选择/填空/简答题
- 📊 **知识大纲生成** - 自动生成笔记的结构化大纲
- 📈 **学习统计** - 追踪笔记和复习进度
- 💾 **云端存储** - 保存所有笔记和分析结果

## 🚀 快速开始

### 前置要求
- Python 3.8+
- pip（Python包管理器）
- 一个有效的ChatGPT API密钥（或第三方中转API）

### 安装步骤

#### 1. 克隆项目
```bash
git clone https://github.com/ys1433223/smart-note-assistant.git
cd smart-note-assistant
```

#### 2. 配置环境变量
```bash
# 复制示例配置文件
cp .env.example .env

# 编辑 .env 文件，填入你的API信息
nano .env
```

必需的环境变量：
```
AI_API_KEY=你的API密钥
AI_API_BASE_URL=https://api.openai.com/v1  # 或第三方中转地址
AI_MODEL=gpt-3.5-turbo
```

#### 3. 安装依赖
```bash
cd backend

# 创建虚拟环境（推荐）
python -m venv venv

# 激活虚拟环境
# Windows:
venv\Scripts\activate
# Mac/Linux:
source venv/bin/activate

# 安装依赖
pip install -r requirements.txt
```

#### 4. 运行应用
```bash
python app.py
```

访问: http://localhost:5000

## 📚 API 文档

### 1. 分析笔记
**POST** `/api/analyze`

请求示例：
```bash
curl -X POST http://localhost:5000/api/analyze \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Python是一种高级编程语言，具有简洁的语法...",
    "title": "Python基础",
    "save": true
  }'
```

返回示例：
```json
{
  "success": true,
  "data": {
    "keywords": ["Python", "编程语言", "语法"],
    "outline": {
      "总体结构": "...",
      "主要内容": ["...", "..."]
    },
    "questions": [
      {
        "type": "choice",
        "question": "Python是什么？",
        "options": ["A. 蛇", "B. 编程语言", "C. 框架", "D. 数据库"],
        "answer": "B"
      }
    ],
    "summary": "Python是一种高级编程语言..."
  }
}
```

### 2. 获取笔记列表
**GET** `/api/notes?page=1&per_page=10`

### 3. 获取笔记详情
**GET** `/api/notes/<id>`

### 4. 保存笔记
**POST** `/api/notes`

```bash
curl -X POST http://localhost:5000/api/notes \
  -H "Content-Type: application/json" \
  -d '{
    "title": "我的笔记",
    "content": "笔记内容",
    "tags": ["标签1", "标签2"]
  }'
```

### 5. 删除笔记
**DELETE** `/api/notes/<id>`

### 6. 健康检查
**GET** `/api/health`

## 🏗️ 项目结构

```
smart-note-assistant/
├── README.md                    # 项目说明
├── .env.example                 # 环境变量模板
├── backend/
│   ├── app.py                  # Flask主应用
│   ├── config.py               # 配置管理
│   ├── requirements.txt         # Python依赖
│   └── modules/
│       ├── __init__.py
│       ├── nlp_processor.py    # NLP文本处理
│       ├── ai_handler.py       # AI API调用
│       └── database.py         # 数据库操作
└── frontend/
    ├── index.html              # 前端页面
    ├── styles.css              # 样式文件
    └── script.js               # 前端交互逻辑
```

## 🔧 配置说明

### 使用ChatGPT官方API
```
AI_API_BASE_URL=https://api.openai.com/v1
AI_API_KEY=sk-xxxxxxxxxxxx
```

### 使用第三方中转API
常见中转服务：
- **openai-sb**: https://openai-sb.com
- **API2D**: https://api2d.com
- **其他中转**: 根据提供商配置

例如使用openai-sb：
```
AI_API_BASE_URL=https://api.openai-sb.com/v1
AI_API_KEY=sk-xxxxxxxxxxxx
```

## 🧠 NLP功能说明

### 关键词提取
- 算法：TextRank
- 语言：中文 + 英文
- 去重：自动去除停用词

### 大纲生成
- 基于文本结构分析
- 自动段落分割
- 层级关系识别

### 题目生成
- 调用ChatGPT API自动生成
- 支持多种题型（选择、填空、简答）
- API不可用时本地降级

## 📦 依赖说明

主要依赖：
- `flask` - Web框架
- `flask-cors` - 跨域支持
- `jieba` - 中文分词
- `requests` - HTTP请求
- `python-dotenv` - 环境变量管理
- `textrank4zh` - 关键词提取

## 🐛 常见问题

### Q: API请求失败怎么办？
A: 检查以下几点：
1. `.env` 文件中 API_KEY 是否正确
2. 网络连接是否正常
3. API是否仍有额度
4. API_BASE_URL 是否正确

### Q: jieba分词不准确？
A: 这是正常情况，可以：
1. 自定义词典：添加领域词汇
2. 调整分词参数
3. 使用更高级的NLP模型

### Q: 数据库错误？
A: 通常是权限问题，尝试：
```bash
rm smart_notes.db
python app.py  # 重新创建
```

## 🚢 部署指南

### 部署到Render（推荐）

1. 在Render创建新Web Service
2. 连接GitHub仓库
3. 设置环境变量
4. 部署即可

### 部署到Railway

1. 连接GitHub账户
2. 创建新项目并选择此仓库
3. 配置环境变量
4. 自动部署

### 部署到Heroku

```bash
# 安装Heroku CLI
# 登录Heroku
heroku login

# 创建应用
heroku create smart-note-assistant

# 设置环境变量
heroku config:set AI_API_KEY=your_key

# 部署
git push heroku main
```

## 📈 开发路线图

### Phase 1 - MVP ✅ 完成
- [x] 后端框架搭建
- [x] NLP模块实现
- [x] AI API集成
- [x] 基础CRUD操作

### Phase 2 - 前端与可视化 🔄 进行中
- [ ] 前端UI设计
- [ ] 思维导图展示
- [ ] 结果可视化
- [ ] 用户认证

### Phase 3 - 优化与部署 📅 计划中
- [ ] 性能优化
- [ ] 错误处理完善
- [ ] 用户体验改进
- [ ] 云端部署
- [ ] 移动端适配

## 🤝 贡献指南

欢迎提交Issue和Pull Request！

### 提交PR前：
1. Fork项目
2. 创建特性分支：`git checkout -b feature/xxx`
3. 提交更改：`git commit -am 'Add feature'`
4. 推送分支：`git push origin feature/xxx`
5. 提交Pull Request

## 📄 许可证

MIT License

## 📧 联系方式

如有问题，请提交Issue或发送邮件。

## 🙏 致谢

感谢所有开源项目的支持：
- Flask
- jieba
- textrank4zh
- OpenAI API

---

**Happy Learning! 🎓**
