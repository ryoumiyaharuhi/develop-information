[Spring Boot 项目打包 + Shell 脚本部署详细总结](https://mp.weixin.qq.com/s/Cy-2AE9bHEOseFJvoAAj6g)

nohup java -jar ${JAR_NAME} >/dev/null 2>&1 &


```
#!/bin/sh

WEB_APP_NAME=xxx-0.0.1-SNAPSHOT
JAR_NAME=${WEB_APP_NAME}\.jar
PID=${WEB_APP_NAME}\.pid

#使用说明，用来提示输入参数
usage() {
    echo "Usage: sh 执行脚本.sh [start|stop|restart|status]"
    exit 1
}

#检查程序是否在运行
is_exist(){
    pid=`ps -ef|grep ${JAR_NAME}|grep -v grep|awk '{print $2}'`
    #如果不存在返回1，存在返回0
    if [[ -z "${pid}" ]]; then
        return 1
    else
        return 0
    fi
}

#启动方法
start(){
    is_exist
    if [[ $? -eq "0" ]]; then
        echo ">>> ${JAR_NAME} is already running PID=${pid} <<<"
    else
        nohup java -jar ${JAR_NAME} >/dev/null 2>&1 &
        echo $! > ${PID}
        echo ">>> start ${JAR_NAME} successed PID=$! <<<"
    fi
}

#停止方法
stop(){
    pid_file=$(cat ${PID})
    if [[ -z ${pid_file} ]];then
    echo ">>> ${JAR_NAME} pid file is not exist <<<"
    else
    echo ">>> ${JAR_NAME} PID = ${pid_file} begin kill ${pid_file} <<<"
    kill ${pid_file}
    rm -rf ${PID}
    sleep 2
    fi
    is_exist
    if [[ $? -eq "0" ]]; then
        echo ">>> ${JAR_NAME} exist PID = ${pid} begin kill -9 ${pid} <<<"
        kill -9 ${pid}
        sleep 2
        echo ">>> ${JAR_NAME} process stopped <<<"
    else
        echo ">>> ${JAR_NAME} is not running <<<"
    fi
}

#输出运行状态
status(){
    is_exist
    if [[ $? -eq "0" ]]; then
        echo ">>> ${JAR_NAME} is running PID is ${pid} <<<"
    else
        echo ">>> ${JAR_NAME} is not running <<<"
    fi
}

#重启
restart(){
    stop
    start
}

#根据输入参数，选择执行对应方法，不输入则执行使用说明

case "$1" in
    "start")
        start
        ;;
    "stop")
        stop
     ;;
    "status")
        status
        ;;
    "restart")
        restart
        ;;
    *)
        usage
        ;;
esac
exit 0
```
