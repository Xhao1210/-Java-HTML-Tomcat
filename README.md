[README.md](https://github.com/user-attachments/files/29254462/README.md)
# 美发店管理系统

基于 Java Web 的美发店管理系统，提供客户管理、员工管理、服务管理、预约管理等功能。

## 技术栈

- **后端**: Java 8+ (JDK 24 兼容)
- **框架**: Servlet 4.0
- **数据库**: MySQL 8.0+
- **连接池**: HikariCP 5.0
- **服务器**: Apache Tomcat 9.0
- **前端**: HTML5 + CSS3 + JavaScript + jQuery

## 功能模块

| 模块 | 功能描述 |
|------|----------|
| 客户管理 | 客户信息增删改查、地址管理 |
| 员工管理 | 员工信息管理、职位管理 |
| 服务管理 | 服务项目管理、价格设置 |
| 预约管理 | 预约创建、查询、状态管理 |
| 订单管理 | 订单创建、支付状态 |
| 库存管理 | 商品库存管理 |

## 项目结构

```
hair_shop_management/
├── backend/                    # 后端代码
│   ├── src/main/java/          # Java 源代码
│   ├── src/main/webapp/        # Web 应用资源
│   └── pom.xml                # Maven 配置
├── database/                   # 数据库脚本
│   └── hair_shop.sql          # 数据库初始化脚本
├── start_server.bat            # Windows 启动脚本
├── env_config.bat             # 环境配置文件
└── README.md                   # 项目说明
```

## 环境要求

- JDK 8+ (推荐 JDK 11 或 JDK 24)
- MySQL 8.0+
- Apache Tomcat 9.0+
- Maven 3.6+ (可选，用于编译)

## 部署步骤

### 1. 环境准备

#### 1.1 安装 JDK

下载并安装 JDK 8+，配置环境变量：
```bash
set JAVA_HOME=C:\Program Files\Java\jdk-24
set PATH=%JAVA_HOME%\bin;%PATH%
```

#### 1.2 安装 MySQL

下载并安装 MySQL 8.0+，创建数据库：
```sql
CREATE DATABASE hair_shop CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

#### 1.3 安装 Tomcat

下载并解压 Apache Tomcat 9.0 到 `C:\apache-tomcat-9.0.22`

### 2. 数据库配置

#### 2.1 执行初始化脚本

```bash
mysql -u root -p hair_shop < database/hair_shop.sql
```

#### 2.2 配置数据库连接

编辑 `env_config.bat` 文件，设置数据库连接信息：

```batch
set "USE_EXISTING_MYSQL=1"
set "MYSQL_SERVICE_NAME=MySQL95"
set "JAVA_HOME=C:\Program Files\Java\jdk-24"
set "TOMCAT_HOME=C:\apache-tomcat-9.0.22"
set "MYSQL_HOME=C:\mysql-9.5.0-winx64"
```

### 3. 编译项目

#### 3.1 使用 Maven 编译

```bash
cd backend
mvn clean package -DskipTests
```

#### 3.2 手动编译（无 Maven）

确保 `backend/target/webapp/` 目录包含所有必要的类文件和依赖。

### 4. 启动服务

#### 方法一：使用启动脚本（推荐）

```bash
start_server.bat
```
### 5. 启动服务
保证你的环境正确，并让AI编辑器知道你的bin目录地址，直接要求AI编辑器启动就行，下面乱七八糟的看都看不懂，别JB浪费时间，全交给AI！！！

#### 方法二：手动启动

1. 部署 WAR 文件到 Tomcat：
```bash
copy backend\target\hair-shop.war C:\apache-tomcat-9.0.22\webapps\
```

2. 手动解压 WAR（JDK 24 兼容性）：
```powershell
Add-Type -AssemblyName System.IO.Compression.FileSystem
[System.IO.Compression.ZipFile]::ExtractToDirectory("C:\apache-tomcat-9.0.22\webapps\hair-shop.war", "C:\apache-tomcat-9.0.22\webapps\hair-shop")
Remove-Item "C:\apache-tomcat-9.0.22\webapps\hair-shop.war"
```

3. 启动 Tomcat：
```bash
C:\apache-tomcat-9.0.22\bin\startup.bat
```

### 5. 访问系统

打开浏览器访问：

```
http://localhost:8080/hair-shop/
```

## API 接口

### 客户管理

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/api/clients` | 获取所有客户 |
| GET | `/api/clients/{id}` | 获取单个客户 |
| POST | `/api/clients` | 创建客户 |
| PUT | `/api/clients/{id}` | 更新客户 |
| DELETE | `/api/clients/{id}` | 删除客户 |

### 员工管理

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/api/staff` | 获取所有员工 |
| GET | `/api/staff/{id}` | 获取单个员工 |
| POST | `/api/staff` | 创建员工 |
| PUT | `/api/staff/{id}` | 更新员工 |
| DELETE | `/api/staff/{id}` | 删除员工 |

### 服务管理

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/api/services` | 获取所有服务 |
| GET | `/api/services/{id}` | 获取单个服务 |
| POST | `/api/services` | 创建服务 |
| PUT | `/api/services/{id}` | 更新服务 |
| DELETE | `/api/services/{id}` | 删除服务 |

### 预约管理

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/api/appointments` | 获取所有预约 |
| GET | `/api/appointments/{id}` | 获取单个预约 |
| POST | `/api/appointments` | 创建预约 |
| PUT | `/api/appointments/{id}` | 更新预约 |
| DELETE | `/api/appointments/{id}` | 删除预约 |

## 注意事项

### JDK 24 兼容性

由于 JDK 24 与某些旧版 JDBC 驱动存在兼容性问题，系统采用以下解决方案：

1. 显式加载 MySQL 驱动：`Class.forName("com.mysql.cj.jdbc.Driver")`
2. 使用 PowerShell 手动解压 WAR 文件（避免 Tomcat 自动解压问题）

### 管理员权限

启动脚本需要管理员权限才能控制 MySQL 服务。如果遇到权限问题：

1. 右键点击 `start_server.bat`
2. 选择"以管理员身份运行"

### 端口占用

如果 8080 端口被占用，修改 Tomcat 配置文件 `conf/server.xml`：

```xml
<Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

## 故障排除

### 数据库连接失败

1. 检查 MySQL 是否启动：`net start MySQL95`
2. 检查数据库配置是否正确
3. 确保 MySQL 用户有访问权限

### Tomcat 启动失败

1. 检查 JAVA_HOME 配置
2. 检查端口是否被占用
3. 查看 Tomcat 日志：`logs/catalina.out`

### API 返回 404

1. 确保 WAR 文件已正确解压
2. 检查 web.xml 中 Servlet 配置
3. 确认 Tomcat 已加载 hair-shop 应用

## 开发说明

### 开发环境

你直接用TraeCN。

### 代码结构

```
src/main/java/com/example/hairshop/
├── servlet/          # Servlet 控制器
├── dao/              # 数据访问层
├── model/            # 实体类
├── util/             # 工具类
└── filter/           # 过滤器
```

### 编译命令

```bash
# 编译项目
mvn clean compile

# 打包 WAR
mvn package -DskipTests

# 运行测试
mvn test
```

## 版本历史

| 版本 | 日期 | 更新内容 |
|------|------|----------|
| 1.0 | 2026-06-23 | 初始版本 |


---

**项目作者**: Xhao1210

**联系邮箱**: wang981294598@163.com
