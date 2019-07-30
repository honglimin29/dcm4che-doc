## Window环境安装

##### 安装工具版本：
- Java SE8 ：直接exe文件安装
- Wildfly 16：https://wildfly.org/downloads/
- OpenLDAPforwindows 2.4.40：https://www.userbooster.de/en/download/openldap-for-windows.aspx
- Apache Directory Studio 2.0.0-M9 ：http://directory.apache.org/apacheds/download/download-windows.html
- Mysql 5.7
- dcm4chee-arc-5.17.0-mysql：https://sourceforge.net/projects/dcm4che/files/dcm4chee-arc-light5/

##### 安装配置
- 解压dcm4chee-arc-5.17.0-mysql
- 设置环境变量DCM4CHEE_ARC=解压文件的目录
##### 数据库
- 创建数据库并授权用户
```
> mysql -u root -p<root-password>
mysql> create database pacsdb;
mysql> grant all on pacsdb.* to 'pacsdb' identified by 'pacsdb';
mysql> quit
 ```
- 导入sql文件创建表
```
mysql -u pacsdb -ppacsdb pacsdb < $DCM4CHEE_ARC/sql/create-mysql.sql
```

##### openLDAP
- 安装openLDAPforwindows :http://www.userbooster.de/en/support/feature-articles/openldap-for-windows-installation.aspx 
- 将LDAP模式的文件导入到OpenLDAP中
```
$DCM4CHEE_ARC/ldap/schema/*  ---->  OpenLDAP\schema
```

- 修改配置文件slapd.conf
```
include         /etc/openldap/openldap/schema/dicom.schema
include         /etc/openldap/openldap/schema/dcm4che.schema
include         /etc/openldap/openldap/schema/dcm4chee-archive.schema

suffix          "dc=dcm4che,dc=org"
rootdn          "cn=admin,dc=dcm4che,dc=org"
rootpw          secret
```
##### Apache Directory Studio
- 安装：exe文件一直下一步
- 创建新的LDAP连接： 菜单 LDSP --> NEW Connection  参数：
```
Network Parameter:
    Hostname: localhost
    Port:     389
Authentication Parameter:
    Bind DN or user: cn=admin,dc=dcm4che,dc=org
    Bind password:   secret
Browser Options
    Base DN : dc=dcm4che,dc=org
```
- 导入ldif文件：导入步骤为：在此连接右键-》import-》LDIF import,选择要导入的文件
```
$DCM4CHEE_ARC/ldap/init-baseDN.ldif
$DCM4CHEE_ARC/ldap/init-config.ldif
$DCM4CHEE_ARC/ldap/default-config.ldif
$DCM4CHEE_ARC/ldap/add-vendor-data.ldif
```
- 新建AETitle文件并导入
创建一个 LDIF 文件并导入命名AETitle.ldif， 文件内容如下
```
version: 1
  # LDIF for modifying the AE Title of the Archive
  # Adjust Base DN (dc=dc=dcm4che,dc=org), Device name (dcm4chee-arc), previous AE Title (DCM4CHEE),
  # new AE Title (MY_AE) before import it into the LDAP server
  dn: dicomAETitle=DCM4CHEE,dicomDeviceName=dcm4chee-arc,cn=Devices,cn=DICOM Configuration,dc=dcm4che,dc=org
  changetype: modify
  newrdn: dicomAETitle=MY_AE
  deleteoldrdn: 1
  newsuperior: dicomDeviceName=dcm4chee-arc,cn=Devices,cn=DICOM Configuration,dc=dcm4che,dc=org
 ```
 - 修改$DCM4CHEE_ARC/ldap/sample-device.ldif 并且导入（直接复制下面的覆盖原文件内容）
 ```
 version: 1
# LDIF for adding a Device providing one Application Entity available on one Network connection
# Adjust Base DN (dc=dcm4che,dc=org), Device name (SAMPLE_DEVICE), AE Title (SAMPLE_AET),
# Hostname (sample.host.name) and Port number before import it into the LDAP server

# Unique AE Title
# (will fail if there is already an object for the same AE Title)
dn: dicomAETitle=SAMPLE_AET,cn=Unique AE Titles Registry,cn=DICOM Configuration,dc=dcm4che,dc=org
objectClass: dicomUniqueAETitle
dicomAETitle: SAMPLE_AET

# Device
dn: dicomDeviceName=SAMPLE_DEVICE,cn=Devices,cn=DICOM Configuration,dc=dcm4che,dc=org
objectClass: dicomDevice
dicomDeviceName: SAMPLE_DEVICE
dicomInstalled: TRUE

# Network Connection
dn: cn=dicom,dicomDeviceName=SAMPLE_DEVICE,cn=Devices,cn=DICOM Configuration,dc=dcm4che,dc=org
objectClass: dicomNetworkConnection
cn: dicom
dicomHostname: sample.host.name
dicomPort: 12345

# Network Connection (secure)
# dn: cn=dicom-tls,dicomDeviceName=SAMPLE_DEVICE,cn=Devices,cn=DICOM Configuration,dc=dcm4che,dc=org
# cn: dicom-tls
# objectClass: dicomNetworkConnection
# dicomHostname: sample.host.name
# dicomPort: 23456
# dicomTLSCipherSuite: TLS_RSA_WITH_AES_128_CBC_SHA
# dicomTLSCipherSuite: SSL_RSA_WITH_3DES_EDE_CBC_SHA

# Network Application Entity
dn: dicomAETitle=SAMPLE_AET,dicomDeviceName=SAMPLE_DEVICE,cn=Devices,cn=DICOM Configuration,dc=dcm4che,dc=org
objectClass: dicomNetworkAE
dicomAETitle: SAMPLE_AET
dicomNetworkConnectionReference: cn=dicom,dicomDeviceName=SAMPLE_DEVICE,cn=Devices,cn=DICOM Configuration,dc=dcm4che,dc=org
# dicomNetworkConnectionReference: cn=dicom-tls,dicomDeviceName=SAMPLE_DEVICE,cn=Devices,cn=DICOM Configuration,dc=dcm4che,dc=org
dicomAssociationInitiator: TRUE
dicomAssociationAcceptor: TRUE
 ```
 
 ##### Wildfly
- 安装 ：直接解压。设：环境变量WILDFLY_HOME为wildfly根目录
- 拷贝配置文件到WildFly安装路径中
 ```
 xcopy %DCM4CHEE_ARC%\configuration\dcm4chee-arc %WILDFLY_HOME%\standalone\configuration\dcm4chee-arc
 ```
- 添加dcm4chee-arc配置文件
```
cp $WILDFLY_HOME/standalone/configuration/standalone-full.xml  $WILDFLY_HOME/standalone/configuration/dcm4chee-arc.xml
vim dcm4chee-arc.xml(把所有的127.0.0.1改为0.0.0.0)
```
- 解压dcm4chee-arc-5.17.0-mysql\jboss-modules下面所有模块到到wildfly
```
解压的时候选择解压到wildfly根目录就行
```
- 下载jdbc驱动：https://dev.mysql.com/downloads/connector/j/
```
将解压得到的mysql-connector-java-5.1.47-bin.jar 放到wildfly-16.0.0.Final\modules\com\mysql\main 目录下
修改module.xml，确保path后面的值跟目录下的jar文件名一致
<resource-root path="mysql-connector-java-5.1.47-bin.jar"/>
```
- 启动wildfly
```
$WILDFLY_HOME/bin/standalone.bat -c dcm4chee-arc.xml
```
- 打开新的cmd窗口
```
$WILDFLY_HOME/bin/jboss-cli.bat -c

------创建一个新的数据源绑定到java:/PacsDS JNDI
[standalone@localhost:9990 /] /subsystem=datasources/jdbc-driver=mysql:add(driver-name=mysql,driver-module-name=com.mysql)
[standalone@localhost:9990 /] data-source add --name=PacsDS      --driver-name=mysql --connection-url=jdbc:mysql://localhost:3306/pacsdb  --jndi-name=java:/PacsDS --user-name=pacsdb --password=pacsdb
[standalone@localhost:9990 /] data-source enable --name=PacsDS

[standalone@localhost:9990 /] jms-queue add --queue-address=StgCmtSCP --entries=java:/jms/queue/StgCmtSCP
[standalone@localhost:9990 /] jms-queue add --queue-address=StgCmtSCU --entries=java:/jms/queue/StgCmtSCU
[standalone@localhost:9990 /] jms-queue add --queue-address=MPPSSCU --entries=java:/jms/queue/MPPSSCU
[standalone@localhost:9990 /] jms-queue add --queue-address=IANSCU --entries=java:/jms/queue/IANSCU
[standalone@localhost:9990 /] jms-queue add --queue-address=Export1 --entries=java:/jms/queue/Export1
[standalone@localhost:9990 /] jms-queue add --queue-address=Export2 --entries=java:/jms/queue/Export2
[standalone@localhost:9990 /] jms-queue add --queue-address=Export3 --entries=java:/jms/queue/Export3
[standalone@localhost:9990 /] jms-queue add --queue-address=HL7Send --entries=java:/jms/queue/HL7Send
[standalone@localhost:9990 /] jms-queue add --queue-address=RSClient --entries=java:/jms/queue/RSClient

[standalone@localhost:9990 /] /subsystem=ee/managed-executor-service=default:undefine-attribute(name=hung-task-threshold)
[standalone@localhost:9990 /] /subsystem=ee/managed-executor-service=default:write-attribute(name=long-running-tasks,value=true)
[standalone@localhost:9990 /] /subsystem=ee/managed-executor-service=default:write-attribute(name=core-threads,value=2)
[standalone@localhost:9990 /] /subsystem=ee/managed-executor-service=default:write-attribute(name=max-threads,value=100)
[standalone@localhost:9990 /] /subsystem=ee/managed-executor-service=default:write-attribute(name=queue-length,value=0)

------开始部署
[standalone@localhost:9990 /]deploy $DCM4CHEE_ARC/deploy/dcm4chee-arc-ear-5.x-psql.ear
```