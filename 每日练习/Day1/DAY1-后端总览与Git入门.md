# Day 1：后端全景图 + Git/GitHub 入门

## 今天的目标

今天先不急着写很多代码，先把脑子里的地图搭起来。

学完今天，你至少要做到：

1. 能讲清楚一个后端项目的完整链路
2. 能讲清楚 FastAPI、MySQL、Redis、Docker、Nginx 的职责
3. 能明白 Git 和 GitHub 到底是什么关系
4. 能掌握最核心的 Git 提交流程

## 一、先把后端项目看成一家餐厅

你可以把一个博客系统理解成一家餐厅：

- 前端：顾客点菜的服务员
- FastAPI：后厨接单窗口
- 业务代码：真正炒菜的厨师
- MySQL：总仓库，长期存原材料和订单记录
- Redis：前台小冰箱，放最常用、最容易取的数据
- Nginx：门口大堂经理，负责分流请求
- Docker：把厨房整套设备打包搬运

### 一个请求是怎么走的

以“查看热门文章列表”为例：

1. 浏览器发起 HTTP 请求
2. 请求先到 Nginx
3. Nginx 转发给 FastAPI
4. FastAPI 先查 Redis 有没有缓存
5. 如果 Redis 有，直接返回
6. 如果 Redis 没有，再查 MySQL
7. 查到后返回给前端，同时把结果写入 Redis

这就是最典型的：

`客户端 -> Nginx -> FastAPI -> Redis/MySQL -> FastAPI -> 客户端`

## 二、你简历上每个技术的原理级理解

## 1. FastAPI 是什么

FastAPI 本质上是一个 Python Web 框架，用来接收 HTTP 请求、执行业务逻辑、返回 JSON 响应。

你不要只记“FastAPI 很快”，你要记住它的本质：

- 它帮你定义接口
- 帮你解析参数
- 帮你校验数据
- 帮你生成接口文档

### 面试可说

“FastAPI 的价值不只是性能，更重要的是它基于类型注解和 Pydantic，能把参数校验、接口定义、文档生成统一起来，比较适合快速搭建规范的后端服务。”

## 2. MySQL 是什么

MySQL 是关系型数据库，用来长期、可靠地保存结构化数据。

博客系统里为什么一定要有 MySQL？

- 用户信息要持久化
- 文章内容要持久化
- 分类、标签、关联关系要持久化
- 数据之间有明确关系

### 面试可说

“MySQL 主要承担核心业务数据的持久化存储，适合处理结构化数据和关联查询，比如用户、文章、标签、多对多关系这些都适合放在 MySQL 里。”

## 3. Redis 是什么

Redis 是内存数据库，重点不是“替代 MySQL”，而是“加速读取”。

为什么热门文章适合放 Redis？

- 被访问频率高
- 数据结构简单
- 可以接受短时间内不是绝对实时

### 你必须理解的关键点

- MySQL：慢一点，但可靠持久
- Redis：快很多，但通常是辅助层

### 面试可说

“Redis 在项目里主要承担热点数据缓存角色，比如热门文章列表、阅读量排行，减少数据库的重复查询压力。”

## 4. Docker 是什么

Docker 解决的是“我电脑能跑，你电脑跑不了”的问题。

本质上它做了两件事：

1. 把运行环境打包
2. 保证环境一致

你项目里用 Docker，不是因为高级，而是因为：

- FastAPI 需要 Python 环境
- MySQL、Redis 是独立服务
- Nginx 也需要配置
- 这些东西一起部署很麻烦

Docker Compose 的作用就是把多个服务编排起来。

## 5. Nginx 是什么

Nginx 最容易理解成“流量入口”。

它常做三类事：

1. 反向代理
2. 静态资源服务
3. 负载均衡

你简历项目里先重点掌握第一类：

浏览器不直接访问 FastAPI，而是访问 Nginx，再由 Nginx 转发到 FastAPI。

好处：

- 对外入口统一
- 配置更灵活
- 更方便加静态资源和 HTTPS

## 三、Git 和 GitHub 到底是什么

很多初学者最容易混的就是这两个。

### Git

Git 是版本控制工具，运行在你本地电脑上。

它负责：

- 记录代码历史
- 比较改动
- 回退版本
- 管理分支

### GitHub

GitHub 是托管 Git 仓库的平台。

它负责：

- 存远程仓库
- 展示项目
- 协作开发
- pull request 代码评审

一句话记住：

`Git = 本地版本管理工具`

`GitHub = 远程代码托管平台`

## 四、你今天必须掌握的 8 个 Git 命令

## 1. 查看状态

```bash
git status
```

看哪些文件被修改了，哪些文件准备提交。

## 2. 初始化仓库

```bash
git init
```

把一个普通文件夹变成 Git 仓库。

## 3. 添加文件到暂存区

```bash
git add .
```

意思不是“提交”，而是“把这次改动放进待提交清单”。

## 4. 提交

```bash
git commit -m "feat: add blog api skeleton"
```

真正把这次修改记录到历史里。

## 5. 查看历史

```bash
git log --oneline
```

## 6. 创建分支

```bash
git switch -c feature/blog-api
```

## 7. 切换分支

```bash
git switch main
```

## 8. 关联远程并推送

```bash
git remote add origin <你的仓库地址>
git push -u origin main
```

## 五、Git 最核心的工作流

你先不要学太复杂的协作流，先记住这条线：

`改代码 -> git status -> git add -> git commit -> git push`

### 这四步分别意味着什么

- `git status`：我现在改了什么
- `git add`：我想提交哪些改动
- `git commit`：把这些改动保存成一个历史版本
- `git push`：把本地历史同步到 GitHub

## 六、把 Day 1 内容套到你的博客项目里

如果你做博客 API，项目里每个技术可以这样解释：

- FastAPI：负责定义注册、登录、文章管理等接口
- MySQL：存用户、文章、分类、标签、关系表
- Redis：缓存热门文章和排行榜
- Docker Compose：把 FastAPI、MySQL、Redis、Nginx 一起启动
- Nginx：作为统一入口转发请求
- Git：管理每次功能迭代
- GitHub：展示项目和托管代码

## 七、面试表达模板

### 1. 你为什么选 FastAPI？

“因为这个项目是典型的 API 后端服务，我希望开发效率高、接口规范清晰。FastAPI 基于 Python 类型注解，配合 Pydantic 可以自动完成参数校验和文档生成，比较适合快速搭建规范接口。”

### 2. MySQL 和 Redis 是怎么分工的？

“MySQL 存核心业务数据，保证持久化和关系查询能力；Redis 主要缓存热点数据，减少重复查询，提高读取性能。”

### 3. Nginx 为什么放在前面？

“Nginx 主要作为统一流量入口，负责反向代理，把外部请求转发到 FastAPI，也方便后续接入静态资源、HTTPS 和多服务部署。”

### 4. Docker Compose 在项目里解决什么问题？

“项目依赖多个服务，手工分别启动和配置成本比较高。Docker Compose 能把 FastAPI、MySQL、Redis、Nginx 统一编排，保证开发环境和测试环境一致，减少部署问题。”

## 八、今天的最小任务

今天不用追求做完整项目，只做 3 件事：

1. 你把这份 Day 1 讲义完整看懂
2. 你能口头复述博客项目请求链路
3. 你开始认识 GitHub 页面结构和 Git 提交流程

## 九、今天建议看的 GitHub 链接

- FastAPI 官方仓库：<https://github.com/fastapi/fastapi>
- SQLAlchemy 官方仓库：<https://github.com/sqlalchemy/sqlalchemy>
- Pydantic 官方仓库：<https://github.com/pydantic/pydantic>
- Alembic 官方仓库：<https://github.com/sqlalchemy/alembic>
- Redis 官方仓库：<https://github.com/redis/redis>
- pytest 官方仓库：<https://github.com/pytest-dev/pytest>
- Docker Compose 示例：<https://github.com/docker/awesome-compose>
- GitHub Hello World：<https://docs.github.com/en/get-started/quickstart/hello-world>

## 十、今天结束前你要能回答的问题

1. FastAPI、MySQL、Redis、Docker、Nginx 分别做什么？
2. 为什么博客项目要同时用 MySQL 和 Redis？
3. Git 和 GitHub 的区别是什么？
4. `git add` 和 `git commit` 的区别是什么？
5. 为什么 Nginx 不直接等于后端服务？

## 十一、明天会学什么

明天进入真正的 FastAPI 核心：

- 路由
- 请求参数
- 响应模型
- Pydantic 校验
- 接口设计

然后我们会正式开始做博客项目的 API 骨架。
