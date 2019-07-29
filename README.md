# dcm4che-doc
开源PACS系统dcm4che学习文档



# 一、介绍
## dcm4che
dcm4che是一个开源的用于医疗信息整合的软件和工具，采用JAVA语言开发，从而提高程序的性能和可移植性。支持JDK1.6及更高版本的部署。

dcm4che项目的核心是DICOM标准的强大实现。dcm4cheDICOM工具包和库在世界各地的许多生产应用程序中使用，当前`5.x`版本已经在`2.x`版本上进行重构，无法直接升级。

## dcm4chee
dcm4che项目中还包含dcm4chee（额外的'e'代表'enterprise'）。dcm4chee是一个图像管理器/图像存档（根据IHE）。该应用程序包含为医疗保健环境提供存储，检索和工作流所需的DICOM，HL7服务和接口。dcm4chee预先打包并部署在JBoss 应用程序服务器中。通过利用许多JBoss功能（JMS，EJB，Servlet Engine等），并承担几个IHE演员的角色 为了互操作性，该应用程序提供了许多强大且可扩展的服务：  

*服务*

|服务|描述|
|:---|:---|
|**基于Web的UI**|dcm4chee包含一个强大的管理员用户界面，完全在Web浏览器中运行。|
|**DICOM存储**|作为存档，dcm4chee能够将任何类型的DICOM对象存储到标准文件系统，并在必要时进行压缩。|
|**DICOM查询/检索**|查询DICOM对象的存档，并检索它们|
|**WADO和RID**|对存档内容的Web访问。|
|**其他DICOM服务**|MPPS，GPWL， MWL，存储承诺，实例可用性通知，研究内容通知，CD媒体输出内容，挂起协议等。|
|**HL7服务器**|集成的HL7服务器，可以对ADT，ORM和ORU消息类型起作用。|
|**IHE服务**|	
通过与XDS / XDSI注册表和存储库集成，充当安全节点并提供合规审计，dcm4chee可以愉快地存在于支持IHE的环境中。|







# 二、安装

[windows安装](/install/for_windows.md)
