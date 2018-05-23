# nagios
nagios一键安装脚本和插件脚本

##一键安装脚本

###服务端

Ubuntu&Debian:

安装脚本：setup/nagios/ubuntu.sh

脚本会自动安装apache，php，nagios和pnp4nagios

CentOS:

安装脚本：setup/nagios/centos6-nagios-server.sh

脚本会自动安装apache，php和nagios

php4nagios安装脚本：setup/nagios/centos-pnp4nagios.sh

###客户端

客户端安装nrpe

ubuntu&debian：setup/nagios_client/ubuntu.sh

centos：setup/nagios_client/centos.sh

##插件脚本

aaa

###客户端添加需要监控的utl
编辑/usr/local/nagios/etc/nrpe.cfg 在文件结尾添加：command[check_webpage_jira]=/usr/local/nagios/libexec/check_http -l hosts -p 8080 -u "/secure/Dashboard.jspa" -w 10 -c 20
#重启
/root/restart_nrpe.sh  

###服务端添加需要监控的nrpe服务器
#1
define host{
        use             linux-server    ; Inherit default values from a template
        host_name       host    ; The name we're giving to this host
        alias           My Linux Server ; A longer name associated with the host
        address         host    ; IP address of the host
        check_command   check-host-alive
        }
define service{
        use                     generic-service
        host_name               host
        service_description             jira
        check_command           check_nrpe!check_webpage_jira
        }

        >>/usr/local/nagios/etc/objects/hosts.cfg
#2 vi /usr/local/nagios/etc 添加下面命令

cfg_file=/usr/local/nagios/etc/objects/hosts.cfg

# 3 vi /usr/local/nagios/etc/objects/commands.cfg 添加下面命令
define command{
        command_name check_nrpe
        command_line $USER1$/check_nrpe -H "$HOSTADDRESS$" -c "$ARG1$"
        }


#3 重启nagios服务
 service nagios restart