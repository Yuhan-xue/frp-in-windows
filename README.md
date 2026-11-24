# 在 Windows 开机时启动FRP

使用Frp内网穿透，远程访问端口

## Latest
- 20260502更新  
   - 建议采用[Frpc-Desktop](https://github.com/luckjiawei/frpc-desktop)而不是本项目
## Install

1. 下载本存储库并解压至某一目录（最好不要带中文）
    ![download](./resource/down.png)
2. [frp](https://gofrp.org/zh-cn/) 程序内置了GO 可能会被杀毒软件误杀，请放至白名单。
3. 修改frpc.toml
   - `[common]`为远程服务器配置，如果使用公用服务器请不要修改

        ```toml
        serverAddr = "zsyq.hxlab.tech"
        serverPort = 7000
        auth.token = "12345678123456781234567812345678123456781234567812345678"
        ```  

   - 在配置文件中自行添加端口，例如以下把本地端口`3389`穿透至远程服务器端口`5200`

        ```toml
        [[proxies]]
        name = "rdp"
        type = "tcp"
        localIP = "127.0.0.1"
        localPort = 3389
        remotePort = 5200
        ```

        `type`:连接类型，常见的有`tcp`，`udp`
        `local_ip`:穿透的本地IP，`0.0.0.0`,`127.0.0.1`都是本地地址
        `local_port`:需要穿透的端口
        `remote_port`:远程访问端口
   - 可以在[frp-dashboard](http://zsyq.hxlab.tech:7500/static/#/proxies/tcp)(账密:admin)查看可用远程端口
   - 联系[管理员](yhx910@ouc.edu.cn)从服务器防火墙放行远程端口
   - 配置完成后可以双击`rdp.bat`查看frp是否正常运行
   - 更多穿透方法请参考<https://gofrp.org/zh-cn/docs/examples/>

4. 添加windows自启
   - 打开搜索地址栏，输入 “任务计划程序” 将会自动搜索，找到后打开任务计划程序，点击右侧的 “创建任务”，开始创建如下：
     - 常规：
        1. 名称：随意填写比如frpc
        2. 安全选项：选择 “不管用户是否登录都要运行”，不存储密码建议不勾选，勾选也可以
        3. 使用最高权限运行：勾选上，根据个人需要选择
        4. 隐藏：勾选上，就不会在启动时弹出命令行窗口了   
        ![常规](./resource/常规.png)
     - 触发器：
        1. 点击新建
        2. 开始任务：选择 “启动时”
        3. 已启用：勾选上
        4. 确定  
        ![触发器](./resource/触发器.png)
     - 操作：
        1. 新建
        2. 操作：选择 启动程序
        3. 设置：程序或脚本，浏览选择bat运行目录比如：D:\frpc\start.bat
        4. 起始于（可选）：输入bat的上级目录比如：D:\frpc\
        5. 确定   
        ![操作](./resource/操作.png)
     - 返回常规：
        1. 返回常规栏目，点击确定，此时会要求输入密码，输入你的电脑密码，然后点击确定即可。
        2. 这样开机自动启动 frp 了，并且不会弹出命令提示符窗口。   
        ![返回常规](./resource/返回常规.png)

## 参考

[1. FRP 官网](https://gofrp.org/zh-cn/)  
[2. Windows 任务计划程序设置程序frpc自启动服务以及出现0X41301错误解决方法](http://www.kaixinit.com/info/maintenance/8212.html)  
[3. FRP Github](https://github.com/fatedier/frp)  
[4. FRP Dashborad](http://zsyq.hxlab.tech:7500/static/#/proxies/tcp)

---

## 许可与版权

- 本仓库未附 `LICENSE` 文件，即未声明开源许可证；默认保留所有权利（All rights reserved）。
- 仓库内引用的第三方数据、图片、教材与工具，版权归各自权利人；转载或商用前请核对来源条款。
- 如需授权、引用或再分发，请先联系仓库维护者。
- `frp` / `frpc.exe` 遵循上游 fatedier/frp 的 MIT 许可；本仓库的 Windows 封装脚本未声明许可。
