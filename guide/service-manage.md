# **服务部署**

服务部署支持将平台预置模型、客户私有模型、微调后发布的模型进行在线部署，部署后即可通过API进行业务调用。

### **创建服务**

点击产品「模型服务平台 UModelVerse」—— 功能菜单「服务管理」——创建服务

1. 模型来源：预置模型（开源模型）or我的模型（私有模型、微调后模型）
2. 确认资源：系统自动根据模型计算所需的算力单元，用户可按需自定义副本数

创建后的服务将根据所选的副本数自动创建推理资源生成在线服务

![](/pics/guide/service-manage1.png)

### **查看&管理服务**

点击产品「模型服务平台 UModelVerse」—— 功能菜单「服务管理」

查看已创建的服务的部署状态。部署成功后，平台将自动生成API服务地址和Key，用户可通过调用API进行服务访问，也可点击「访问」打开对话框进行在线验证。

![](/pics/guide/service-manage2.png)