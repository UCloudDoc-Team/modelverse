# 使用UmodelVerse QwQ API实现MCP Server

## 以MCP SQLITE为示例

### 安装VSCODE

从官网 [https://code.visualstudio.com/下载vscode安装包进行安装。](https://code.visualstudio.com/下载vscode安装包进行安装。)

### 安装CLINE

在vscode extension列表中找到CLINE并选择安装。

![image](assets/resources/ZgMNy350iWSJFl0I5J1bc58eppVM_ZRhsRWBml55o88.png)

### 配置CLINE使用UCloud QWQ API

#### 1.点击VSCODE左侧的小机器人图标

![image](assets/resources/Kci9lGQeFTB0s26pxSezcg1LxxvoYU0gP6YjQbY2lSo.png)

#### 2.首次使用会提示配置chatbot的api,如果之前使用过，可以点击CLINE的设置齿轮进行配置。

下图中：

API Provider选择OpenAI Compatible

Base url,API key,Model ID 为UCloud提供的API信息

由于QWQ为思维链模型，所以在Model Configuration里需要选择Enable R1 messages format。

Context Window Size限制40000

![image](assets/resources/VkstAzwZgzKUY5WMcET3Gcu-ZZFVOB3DMfugIMaPZHs.png)

#### 3.配置好后可以进行对话测试

看到回复表明配置成功。

![image](assets/resources/RomNRkouC_Tnn86bjqJ5qzJNcyWyk5WFegCYFo_CxC8.png)

### 安装UV 和SQLITE

参考 [https://zhuanlan.zhihu.com/p/20222456593](https://zhuanlan.zhihu.com/p/20222456593)

Windows 用户在cmd中运行

```
winget install --id=astral-sh.uv -e
winget install sqlite.sqlite
```
 
Mac用户使用

```
brew install uv
brew install sqlite3
```
 
### 配置CLINE使用mcp-server-sqlite

点击小机器人图标打开CLINE

选择页面上方server的小图标

在MCP Servers中选择installed标签，下面会有Configure MCP Servers,点击打开Cline的MCP配置文件

![image](assets/resources/4zPO7r7L11FSk9wqV32ddJEJv62VSmpvjG9nhE-yUJA.png)

修改CLine MCP配置文件。

Windows修改参考：

```
"sqlite_server" :{
    "command": "cmd.exe",
  "args": [
  "/c",
  "uvx",
  "mcp-server-sqlite",
  "--db-path",
  "D:\\ymkdatabase\\tmp.db"
]
```
 
Mac修改参考：

```
{
  "mcpServers": {
    "sqlite_server": {
      "command": "uvx",
      "args": [
        "mcp-server-sqlite",
        "--db-path",
        "D:\\ymkdatabase\\tmp.db"
      ]
    }
  }
}
```
 
其中："D:\\\\ymkdatabase\\\\tmp.db"修改为希望储存sqlite数据库的位置，可以为已有db文件，如果没有会自动创建。

文件保存后会提示

![image](assets/resources/I9MBR__of3Je9gHaUEj09y_ZVWH_xVQndBj50Xf4WQw.png)

installed选项卡下会出现sqlite_server的选项卡，如下图绿色状态说明已经启用。

![image](assets/resources/D7V8xrsn-zoMLT2FiKsrBEKG2gacF4Lsc-bdvIfpr-g.png)

### 交互示例

1. #### 列出database

在对话框内输入以下指令并发送：

```
你能打开哪些database？
```
 
![image](assets/resources/5ZbZNJPXvQ6vQXaFHtcUafyvxpBAf3T5G90bIidloI0.png)

模型会返回需要运行的命令并询问是否要运行，选择run command或approve会执行命令。

一次任务可能会涉及多次运行，每次都需要人类进行批准。

如果想要模型全自动执行，可以配置auto approve选型，但是会有一定安全风险。

![image](assets/resources/ePNjqGKOb-U5nnl9k56NYzTSYhksHXUPAnd9YeRbols.png)

运行后得到以下回答：

![image](assets/resources/-kFOKE_BHA_uqCMAfGqB46OFUxQwnUD8xwPf8z11elU.png)

2. #### 创建table

```
我需要创建一个员工名单列表，里面需要记录11位数员工id,员工名称，员工职级和员工入职日期，帮我创建一下。
```
 
![image](assets/resources/y4jG0cpGyxephjNGpU-UAHCRGC7VQ8tjj8lMc0Sdf7Y.png)

模型请求执行创建table命令，选择批准。

![image](assets/resources/dj_gkHuRapIrjP85UdT6csgwD8rxQVTd5NqnuZ9FHc4.png)

这样就创建好了一个table用于存储数据。

![image](assets/resources/DfuZR-Ja9LLGpIrIqHXlmT2EZ7N2fqHAaKuzjAv1mxE.png)

### 插入随机示例数据

```
帮我生成20个随机的员工样本，我需要看一下表格是否符合预期。
```
 
模型请求执行插入命令，选择批准。

![image](assets/resources/AyhcW_g57HqvWLc3akWTlCum3YtIQpYZz6vCI_MYPXg.png)

模型请求执行select命令用于检查是否插入成功，选择批准。

![image](assets/resources/FA-c-arJyWZ8T5S6lcR0CepnLwvdAykY9aLC6kxzhHM.png)

模型执行完了select命令，可以看到成功在sqlite中插入了随机数据，任务完成。

![image](assets/resources/RLTtKSiPxoXZVjZlu-1S6MgeDT0PfWqiEnFWnvu87ho.png)



### 清空数据

```
帮我把数据和列表都删除掉，我需要空的database。
```
 
模型请求执行删除列表命令，选择批准。

![image](assets/resources/rblT0gQJLs7hT5nHpE1gIgECNxhc_zxCHxJ6JK9h7cA.png)

模型请求执行列表查看功能，选择批准。

![image](assets/resources/-APBPfqHcKTrkWWeyCvoJU-XbGJd8DQxODApwCSqG2Q.png)

模型确认数据已清空，任务完成。

![image](assets/resources/LeHxuu22POsU3oBUtx7YT98cRlDvCt2LbRZhS_V95tg.png)




