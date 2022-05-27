# azureChina_B2C_custom_policy
azure AD B2C custom policy in azure china cloud

因为在azure china cloud上azure AD B2C的功能相对于azure global有很多缺失，所以只能通过custom policy来处理。
如果从头开始定义custom policy的话，涉及到很多xml的编写工作。
这里可以借助在工具来生成一些custom policy，由于这个工具目前只支持global，所以是现在global试运行，然后把对应的文件下载来，就行相应的修改。

具体步骤如下：

1） 在这个地址中 https://b2ciefsetupapp.azurewebsites.net 生成一些常规的custom policy以及基本元素。
<img width="1065" alt="Screen Shot 2022-05-27 at 11 42 19 AM" src="https://user-images.githubusercontent.com/7360524/170624876-4f61a2ef-acff-4e19-9b5d-0086cdd94004.png">
结果如下图，按照相应的指示，点击两个link完成授权：
<img width="1066" alt="Screen Shot 2022-05-27 at 11 44 44 AM" src="https://user-images.githubusercontent.com/7360524/170625054-cec147eb-1a34-4070-bda4-08f429c882ef.png">

2） 在这个页面中 https://b2ciefsetupapp.azurewebsites.net/Home/Experimental?sampleFolderName=impersonation 选择你要的custom policy
因为在azure china，MFA没有邮件，所以选择这个custom policy来实现邮件的多因子认证。
<img width="1069" alt="Screen Shot 2022-05-27 at 11 46 44 AM" src="https://user-images.githubusercontent.com/7360524/170625358-63f4a18d-39d8-4933-83dc-c9c6d2db817d.png">

3） 在global azure 实验这条custom policy
<img width="1440" alt="Screen Shot 2022-05-27 at 11 48 52 AM" src="https://user-images.githubusercontent.com/7360524/170625504-d762aa47-d5c7-4844-8c1f-7223a192bdea.png">

4）实验成功之后，把对应的定义文件，有继承关系的都下载下来。
我这里就是如上四个文件。

5）注册App（IdentityExperienceFramework，ProxyIdentityExperienceFramework）
b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.是系统默认创建的在azure-common中使用到。
<img width="1129" alt="Screen Shot 2022-05-27 at 12 13 27 PM" src="https://user-images.githubusercontent.com/7360524/170627978-04afa73e-39a1-4ec3-a734-0ed1d0a0f31c.png">

6）创建secret 和签名（保持名字和默认的一致，B2C_1A_TokenSigningKeyContainer，B2C_1A_TokenEncryptionKeyContainer）在token issuer中用到
<img width="656" alt="Screen Shot 2022-05-27 at 12 15 57 PM" src="https://user-images.githubusercontent.com/7360524/170628121-ca6b5b66-ffdc-4a88-a200-0034aeb34f13.png">

7）修改下载下来的定义文件
每个文件的头部TenantId，PublicPolicyUri都要修改成cn上的tenant信息。集成base也要相应修改，删除tenantobjectId。
<img width="886" alt="Screen Shot 2022-05-27 at 12 17 56 PM" src="https://user-images.githubusercontent.com/7360524/170628277-21fa6187-2821-423f-8ad6-59232db698c9.png">

8）修改TrustFrameworkBase的ProviderName，METADATA，authorization_endpoint等
<img width="1390" alt="Screen Shot 2022-05-27 at 12 20 03 PM" src="https://user-images.githubusercontent.com/7360524/170628452-b5730250-c40b-4224-8ca3-46bcc9ec3303.png">

9）修改TrustFrameworkExtensions的login-NonInteractive里面（client_id，IdTokenAudience，resource_id）
<img width="1269" alt="Screen Shot 2022-05-27 at 12 21 54 PM" src="https://user-images.githubusercontent.com/7360524/170628552-d12e0a62-080e-46ec-a5f2-4e476ad78162.png">

