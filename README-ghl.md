# Bistoury
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

FAQ1.bin中的配置文件报错，由于windows和linux不兼容 dos2unix -q bistoury-agent-env.sh 