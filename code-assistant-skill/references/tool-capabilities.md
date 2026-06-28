# 工具核心能力参考

## Batch (.bat)

### 核心命令
- `for %f in (*.txt) do ...` — 遍历当前目录所有 .txt 文件
- `for /r` — 递归遍历子目录
- `xcopy source dest /s /e /y` — 复制目录（含子目录、覆盖不提示）
- `robocopy source dest /mir` — 镜像同步目录
- `ren old new` — 重命名文件
- `del /f /q file` — 强制静默删除文件
- `call program.exe args` — 调用外部程序
- `echo text >> file.txt` — 追加文本到文件

### 适用场景
- 一键批量重命名文件
- 批量复制/移动/删除
- 启动多个程序的快捷脚本
- 简单的文件存在性检查

### 限制
- 没有原生的 JSON/CSV 解析能力
- 字符串处理能力弱
- 不支持 try/catch 错误处理
- 循环和条件判断语法不够直观

---

## PowerShell (.ps1)

### 核心能力
- `Get-ChildItem` — 列出文件（类 ls）
- `ForEach-Object` — 管道遍历
- `Where-Object` — 管道过滤
- `Select-Object` — 选择属性
- `Get-Content` / `Set-Content` — 文件读写
- `Invoke-WebRequest` — 轻量 HTTP 请求（无需额外安装）
- `ConvertFrom-Json` / `ConvertTo-Json` — JSON 处理
- `Get-Process` / `Get-Service` — 系统管理
- `New-Item` / `Remove-Item` / `Copy-Item` / `Move-Item` — 文件操作
- COM 对象操作 — 可操控 Excel、Word 等 Office 应用

### 适用场景
- Windows 系统管理（注册表、服务、计划任务）
- 中复杂度文本处理
- 轻量 HTTP 请求与 JSON 解析
- 调用 Windows 原生功能（比 Python 更方便）

### 限制
- 无法使用 pandas 等 Python 数据分析库
- 浏览器自动化能力弱
- 语法对新手不算友好

### 运行权限注意
- 默认情况下双击 `.ps1` 文件不会执行（安全策略），需右键 →「使用 PowerShell 运行」
- 或从 PowerShell 窗口内运行：`.\script.ps1`

---

## Python (.py)

### 标准库能力（无需额外安装）
- `os` / `shutil` / `pathlib` — 文件与目录操作
- `re` — 正则表达式
- `json` / `csv` / `xml` — 数据格式解析
- `urllib` / `http` — 基础网络请求
- `subprocess` — 调用外部程序
- `datetime` — 日期时间处理
- `sqlite3` — 轻量数据库

### 常用第三方库（需 pip 安装）
- `requests` — HTTP 请求（比标准库更友好）
- `pandas` — 数据处理（Excel/CSV/数据库）
- `openpyxl` — Excel .xlsx 读写
- `BeautifulSoup4` + `lxml` — HTML 解析
- `playwright` — 浏览器自动化（比 Selenium 更现代）
- `Pillow` — 图片处理

### 适用场景
- 复杂 Excel/CSV 数据清洗与汇总
- API 调用与数据同步
- 网页数据采集
- 正则/文本深度加工
- 需要大量第三方生态支持的任务

### 限制
- 简单文件操作过于重型（一行 Batch 能解决的不要用 Python）
- 无法直接操作 GUI 桌面应用
- 需要安装依赖库（对零基础用户增加复杂度）

---

## 影刀RPA

### 核心节点类型
- **Python代码块** — 运行标准 Python 代码
- **元素捕获** — 识别和操作桌面/网页元素
- **鼠标/键盘操作** — 模拟人工点击和输入
- **判断节点** — if/else 逻辑分流
- **循环节点** — 遍历列表或计数循环
- **等待节点** — 等待元素出现或固定时长

### Python代码块内置函数
- `yd.get_var("变量名")` — 读取影刀流程变量
- `yd.set_var("变量名", 值)` — 写入影刀流程变量
- `yd.log("内容")` — 输出到影刀运行日志

### 适用场景
- 需要操作没有 API 的网页/桌面软件
- 跨应用流程（从网页取数据→填入Excel→发送邮件）
- 定时重复性业务操作（每日登录后台截图/导数据）
- 可视化编排比纯代码更容易维护的流程

### 限制
- 纯后端数据处理效率不如 Python
- 流程文件难以做 Git 版本管理
- 高频操作受界面响应速度限制
- Python代码块中不保证所有第三方库可用

---

## VBA（Excel/Office 宏）

### 核心能力
- `Workbooks.Open("路径")` — 打开工作簿
- `Worksheets("名称").Range("A1").Value` — 读写单元格
- `For Each cell In Range("A1:A100")` — 遍历单元格
- `Workbooks("名称").SaveAs "路径"` — 另存为
- `Application.WorksheetFunction.VLookup(...)` — 调用 Excel 内置函数
- `UserForm` — 创建自定义对话框
- 自定义函数 (UDF) — 可像 Excel 内置函数一样在单元格中使用

### 适用场景
- 批量处理 Excel 工作簿（格式统一、需要保留公式和格式）
- 在已有 Excel 工作流中嵌入自动化
- 创建带交互界面的 Excel 工具分发给同事

### 限制
- 必须安装 Microsoft Office
- 网络请求能力极弱
- 无法处理非 Office 文件格式
- 代码依赖于具体的 Excel 版本

---

## SQL

### 核心能力
- `SELECT ... FROM ... WHERE ...` — 基础查询
- `JOIN` — 多表关联
- `GROUP BY` + 聚合函数 (`COUNT`, `SUM`, `AVG`, `MAX`, `MIN`)
- `CASE WHEN` — 条件逻辑
- 窗口函数 (`ROW_NUMBER`, `RANK`, `LAG`, `LEAD`)
- CTE (`WITH ... AS`) — 公用表表达式
- `CREATE TABLE` / `INSERT` / `UPDATE` / `DELETE` — 数据操作

### 适用场景
- 从数据库中提取和聚合数据
- 数据清洗（去重、格式化、条件筛选）
- 生成报表和统计汇总
- 与 Python/pandas 配合（SQL 取数据 → pandas 分析）

### 限制
- 必须连接数据库（不能直接操作文件）
- 无法做网络请求或文件操作
- 非结构化数据不支持
