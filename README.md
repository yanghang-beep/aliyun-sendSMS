## 准备工作
我们首先登陆阿里云 https://www.aliyun.com
###  完成阿里云认证
首先我们要想使用阿里云的短信服务，必须完成认证，个人认证和企业认证都可以。
点击你的名字完成认证步骤。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019080809242838.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTI5MzYx,size_16,color_FFFFFF,t_70)
###  生成秘钥

 - 点击控制台
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190808092648425.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTI5MzYx,size_16,color_FFFFFF,t_70)
 - 点击这里
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190808092725755.png)
 
 - 记住这两个值，敲代码的时候要用，有了这两个就能有权限访问你的阿里云。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2019080809280368.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTI5MzYx,size_16,color_FFFFFF,t_70)
###  申请签名和模板
签名就是以谁的名义给你发短信，模板就是短信的模板是什么样的，这个是需要审核的，大概工作日两个小时之内就能审核完成。
在短信服务的如下地方申请签名和模板。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190808093020911.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTI5MzYx,size_16,color_FFFFFF,t_70)
记住签名和你的模板代码（SMS_）开头的。
##  编写代码
签名和模板申请通过之后,我们就可以测试发送短信了。
短信服务的快速学习里面可以查看DEMO。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190808093247784.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTI5MzYx,size_16,color_FFFFFF,t_70)
###  需要引入的JAR包依赖

    <dependency>
      <groupId>com.aliyun</groupId>
      <artifactId>aliyun-java-sdk-core</artifactId>
      <version>4.0.3</version>
    </dependency>

###  代码

```
public class SendSms {
    public static void main(String[] args) {
       // <accessKeyId>、<accessSecret>上面申请的秘钥
        DefaultProfile profile = DefaultProfile.getProfile("cn-hangzhou", "<accessKeyId>", "<accessSecret>");
        IAcsClient client = new DefaultAcsClient(profile);

        CommonRequest request = new CommonRequest();
        request.setMethod(MethodType.POST);
        request.setDomain("dysmsapi.aliyuncs.com");
        request.setVersion("2017-05-25");
        request.setAction("SendSms");
        request.putQueryParameter("RegionId", "cn-hangzhou");
        request.putQueryParameter("PhoneNumbers", "18888888");
        request.putQueryParameter("SignName", "上面申请的签名");
        request.putQueryParameter("TemplateCode", "上面申请SMS开头的");
        // 模板中的占位符
        request.putQueryParameter("TemplateParam", "{\"code\",213121}");
        try {
            CommonResponse response = client.getCommonResponse(request);
            System.out.println(response.getData());
        } catch (ServerException e) {
            e.printStackTrace();
        } catch (ClientException e) {
            e.printStackTrace();
        }
    }
}
```
##  充钱
如果你账户没有余额的话，会提示你余额不足，可以冲一块钱试一下。
