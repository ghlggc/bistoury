# Bistoury
linux启动：
proxy 默认使用 9090 端口，ui 默认使用 9091 端口，agent 和 proxy 通信默认使用 9880 端口，agent 和应用通信使用的3668端口，ui 和 proxy 通信默认使用 9881 端口，h2 数据库默认使用 9092 端口
agent通过调用proxy端口进行通信，proxy和ui通过注册到同一个zk上进行通信
1.使用D:\workspace_tmp\bistoury>mvn clean install -P prod -Dmaven.test.skip=true 命令打包
获取bistoury-agent安装包位于bistoury-dist/target目录下的bistoury-agent-bin.tar.gz
获取bistoury-ui安装包位于bistoury-ui/target目录下的bistoury-ui-bin.tar.gz
获取bistoury-proxy安装包位于bistoury-proxy/target目录下的bistoury-proxy-bin.tar.gz
2.将三个包上传的服务器上，bistoury-proxy和bistoury-ui可部署在同一台机器上，bistoury-agent必须部署在要监控的机器上
3.修改bistoury-proxy和bistoury-ui中的conf/的jdbc和registry中的mysql和zk的实际信息
4.修改bistoury-agent/bin/bistoury-agent-env.sh中的BISTOURY_PROXY_HOST="127.0.0.1:9090"，默认是127.0.0.1
当agent和proxy不在同一台机器上时需要改为实际proxy所在的机器和端口
5.分别启动ui( sh bistoury-ui.sh -j /export/servers/jdk1.6.0_25 start)，proxy(sh bistoury-proxy.sh  -j /export/servers/jdk1.6.0_25 start)，
agent(sh bistoury-agent.sh -j /export/servers/jdk1.6.0_25 start)

FAQ1.bin中的配置文件报错：: invalid option name2: set: pipefail ，由于windows和linux不兼容vi bistoury-agent-env.sh  :set ff=unix dos2unix -q bistoury-agent-env.sh

windows本地启动：
首先需要修改数据库连接、zk注册地址。agent通过调用proxy端口进行通信所以需要加bistoury.proxy.host=127.0.0.1:9090启动参数。proxy和ui通过zk通信
模拟linux环境的proc目录，所以修改了代码里面写死的proc的路径指向本地windows的目录，并修改目录中的pid。
ui：     启动类：qunar.tc.bistoury.ui.container.Bootstrap         java启动参数：-Dbistoury.conf=D:\workspace_tmp\bistoury\bistoury-ui\conf
proxy：  启动类：qunar.tc.bistoury.proxy.container.Bootstrap      java启动参数：-Dbistoury.conf=D:\workspace_tmp\bistoury\bistoury-proxy\conf
agent：  启动类：qunar.tc.bistoury.indpendent.agent.Main          java启动参数：-Dbistoury.proxy.host=127.0.0.1:9090 -Dbistoury.user.pid=19120 -Dbistoury.lib.dir=D:\workspace_tmp\bistoury\bistoury-dist\target\bistoury-agent-bin\lib\ -Dbistoury.app.lib.class=org.springframework.web.servlet.DispatcherServlet
-Dbistoury.proxy.host表示proxy的地址和端口，便于通信。-Dbistoury.user.pid表示要debug应用的pid。-Dbistoury.lib.dir指定agent和arthas的包路径。-Dbistoury.app.lib.class随便指定一个加载的类