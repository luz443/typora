## CMD.Powershell 等概念解析

### 什么是Terminal等？

Terminal：即终端，可以理解为人与电脑交流的界面

Windows Terminal：豪华版cmd+powershell

Shell：人和电脑沟通的桥梁，常见的有cmd、powershell、bash等等

Bash：linux系统中最常用的shell

CMD.Powershell：Windows系统常用的shell，在terminal直接和电脑后台沟通控制电脑

SSH：一把带锁的远程对讲机，即加密的远程登录工具，只要知道密码就可以远程操纵另一台电脑、服务器

环境变量：超市门口的导购图，电脑根据环境变量明白人输入的命令对应的实际位置

System32：超市核心区，电脑大部分系统工具（ipconfig、where等都住在这里），路径为C:\Windows\System32，系统只有配好环境变量才能找到，否则就要输入全部路径

PowerShell7：旧版powershell的升级版

oh my posh：powershell的一款皮肤

### 配置

- 安装oh-my-posh

  ```powershell
  winget install --id JanDeDobbeleer.OhMyPosh -e
  #安装oh-my-posh
  
  oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH\主题名.omp.json" | Invoke-Expression
  #初始化（在网站找到喜欢的主题）
  
  notepad $PROFILE    
  oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH\主题名.omp.json" | Invoke-Expression
  #新建文件并添加以上内容
  ```

- 安装Powershell 7

```powershell
winget install --id Microsoft.Powershell --source winget
```



- 基本环境变量

1. **Win + S** 搜 **“环境变量”** → 选“编辑系统环境变量”。
2. 点 **“环境变量…”** → 在 **“系统变量”** 里找到 **Path** → 点“编辑”。
3. **新建** 一行填 `C:\Windows\System32` → 确定即可配置环境变量

 注：若无效，以管理员身份打开cmd（win键+搜索cmd+以管理员身份），执行`sfc /scannow`系统补文件



## Bat文件

### <u>bat文件是什么</u>

- bat文件（批处理文件）为cmd专属脚本，即多行的cmd执行命令

- 如何创立bat文件？
  1. 新建文本文档
  
  2. 将.txt后缀改为.bat后缀（注意要改后缀名而不是文件名！在文件查看中显示文件后缀名）
  
  3. 另存为中将编码改为ANSI，防止出现乱码
  
  4. 双击bat文件即可运行命令
  

### 常见命令

1. 查看内容：dir

​	输入 `dir` 即可显示当前文件夹的内容（文件和子文件夹）。也可以 `dir+文件夹路径` 查看某个特定文件夹内容

2. 切换文件夹：cd
   - cd 绝对路径（bat运行时为cd /d可以跨盘） 或 cd 相对路径 （当前文件夹再往里一层）
   - `cd ..`返回上一级目录
   - `cd \`切换到根目录
   - 跨盘进根：盘符+冒号即可 例如 `D:`
3. 创建文件夹：md

​	在当前文件夹创建文件夹 `md Test`

4. 删除文件夹：rd

​	`rd /s Test`中加上/s可以删除非空文件夹，不加只能删除空文件夹。`/s` 递归删除，`/q` 静默删除

5. 复制文件：copy

   - 把a复制到b  `copy a.txt b`，用 /y 可以覆盖文件而不提醒，/-y 则禁止覆盖现有文件
   - 复制时可以写绝对路径也可以相对路径，但相对路径只能写当前文件夹内部的
   - 复制要看目标路径的落脚点，如果是文件夹那就在它的里面；如果是文件那么就改名
   - 若希望改名并在同一个目录下，可以先copy为新名字再del旧名字

6. 移动文件（夹）：move

   - `movv a.txt b` 即可
   - `move 旧名字 新名字`则可以原地改名（文件和文件夹都可）
   - `move 旧名字.txt 路径+新名字.txt` 可以既改名又移动

7. 改名（文件或文件夹）：ren

   `ren 旧名字 新名字` 但rename不能跨目录！只能在当前目录操作改名

8. 删除文件：del

​	`del C:\Users\Documents\example.txt`

​	加入`/f`强制删除只读文件，`/q`静默删除

9. 查看网络信息：ipconfig

10. 清屏：cls

    cls后仍保留当前目录路径

11. 退出：exit

12. 输出：echo

    - `echo 你要输出的内容`
    - `@echo off` 清理掉无关信息
    - `echo. > C:\1.txt` 创建/覆盖空文本文件
    - `echo 内容 > 路径`创建/覆盖有内容的文本文件
    - `echo 内容 >> 路径` 可以追加内容

13. 暂停：pause

    如果不在最后加pause，运行bat文件时cmd窗口只会闪一下

14. 查看任务列表：tasklist

​	只看某个进程 `tasklist /fi "imagename eq chrome.exe"`

15. 中止任务：taskkill

​	关闭某个 `taskkill /im chrome.exe /f` 其中/f为不保存数据强制关闭

16. 查看与网站的连接通不通：ping

​	`ping www.baidu.com -n 4` 打四次电话

17. 读取文件：type

    把文件内容打印到黑框里看（只可以读不能改） `type 文件路径`

18. 打开文件：notepad

    可以打开已有记事本编辑或者新建一个记事本开始编辑！

19. 复制文件夹：robocopy  

     `/e` 表示递归复制，默认自动覆盖（不可以改名），复制时目标路径的落点就是新文件夹位置

20. 注释： `rem 注释内容` 即可注释

### 常见逻辑

1. 变量

```bat
set /p name=请输入你的名字：
echo 你好，%name% !
```

- `set name=小明` 自己输入一个值
- `set /p` 要求用户输入值
- `set /a cnt+=1` /a 把set后的当做数学表达式，每次把cnt加1
- 变量用%括起来



2. 判断文件是否存在

```bat
if exist "D:\Backup\作业.txt" (
	echo 已找到
) else (
	echo 不存在
)
```

- `if exist "路径"`注意路径前后都有空格
- else前后各一个空格一个括号
- 可以判断文件、文件夹、程序



3. 循环批量操作

```bat
for %%f in (*.jpg) do (
	ren "%%f" "班级_%%f"
)
```

- `%%f` 为变量名
- 必须在前面加上 `setlocal enabledelayedexpansion` (变量需要不断变化修改时必备)
- 可以在通配符前加上路径如 `(D:\*.jpg)`，否则会检索当前目录
- `%%f` 为文件名+路径，`%%~nf` 仅包括文件名和扩展名，不包括路径

```bat
setlocal enabledelayedexpansion
for %%f in (*.jpg) do (
    set "name=%%~nf"          :: 只取文件名（无扩展名）
    set "name=!name: =!"      :: 把空格删掉
    ren "%%f" "!name!%%~xf"   :: 拼回扩展名
)
```

其他批量操作：

| 操作         | 命令                    |
| ------------ | ----------------------- |
| 批量移动     | `move "%%f" "D:\Done\"` |
| 批量删除     | `del "%%f"`             |
| 批量改扩展名 | `ren "%%f" "%%~nf.png"` |
| 批量删字符   | !变量:旧符=!            |



<span style="font-size:1.1em;"><u>***当路径有空格或中文时把路径加上双引号***</u></span>

  

4. 重定向

- `>nul` 把标准输出（正常输出信息）定向到nul，即不显示正常信息
- `2>&1` 把错误输出重定向到标准输出，合并到一个管道里，方便下一步操作
- `>nul 2>&1` 错误定向为标准，标准定向为nul，则不会输出任何信号
- `2>nul` 错误输出定向到nul，即不输出错误信息（不会报错，更加清爽）

eg：

```bat
echo 正常信息
md C:\Folder 2>nul
echo 命令执行完毕
```



5. 通配符

```bat
copy "D:\*.txt" "E:\hi\" 
```

`*` 表示所有