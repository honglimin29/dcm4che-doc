## 为什么学习dcm4che?

终于等到你！dmc4che -- 开放源码的临床图像和对象管理软件

# 为什么选择dcm4che?

dcm4che是用于医疗企业的开源应用程序和实用程序的集合。为了提高性能和可移植性，这些应用程序均采用Java编程语言开发的，支持在`JDK 1.6`及以上版本部署。

dcm4che项目的核心是DICOM标准的强大实现。`dcm4che DICOM toolkit & library`在世界各地的许多生产应用程序中使用。

### 官网地址

[https://dcm4che.org/](https://dcm4che.org/)

### 版本选择

dcm4che存在 `2.x` 和 `5.x` 版本，其中 `5.x` 版本经过了重新设计，拥有更改的性能和灵活性，在这里强烈推荐使用`5.x`版本

### dcm4chee

dcm4che项目中还包含dcm4chee`（额外的'e'代表'enterprise'）`。dcm4chee是一个图像管理/图像存档软件（根据[IHE](https://www.ihe.net/)）。该应用程序为医疗软件环境提供存储，检索和工作流所需的[DICOM](http://dicom.nema.org/standard.html)，[HL7](http://www.hl7.org/)服务和接口。

dcm4chee预先打包并部署在JBoss 应用程序服务器中。并且通过利用许多JBoss功能（`JMS`，`EJB`，`Servlet Engine`等），为医疗企业信息整合提供多重角色。 为了互操作性，该应用程序提供了许多强大且可扩展的服务:  

*服务*

|服务|描述|
|:---|:---|
|**基于Web的UI**|dcm4chee包含一个强大的管理员用户界面，完全在Web浏览器中运行。|
|**DICOM存储**|作为存档，dcm4chee能够将任何类型的DICOM对象存储到标准文件系统，并在必要时进行压缩。|
|**DICOM查询/检索**|查询DICOM对象的存档，并检索它们|
|**WADO和RID**|基于WEB的方式访问存档内容|
|**其他DICOM服务**|`MPPS`, `GPWL`, `MWL`, `Storage Committment`, `Instance Availability Notification`, `Study Content Notification`, `Output Content to CD Media`, `Hanging Protocols`, and more.|
|**HL7服务器**|通过集成的HL7服务器，可以对`ADT`，`ORM`和`ORU`消息类型起作用。|
|**IHE服务**|通过与XDS / XDSI注册表和存储库集成，充当安全节点并提供合规审计，dcm4chee可以快捷的集成到IHE的环境中。|


### dcm4chee的应用

![](/image/HL7-Flow.jpg)


