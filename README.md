# EducationalManagementSystem-安卓教务管理系统
An Android Educational management system—安卓教务管理系统
## 实验需求分析
实验目的是使用Android移动开发技术设计开发一个掌上课堂信息管理系统，作品具备学生课堂及课下相关表现的数据。所以设置学生、教师、管理员三个角色，学生可以自行注册，只有管理员可以添加教师和管理员帐号。
在注册功能中用线程通信的httpGet搭配云服务实现短信发送验证码验证，广播接收者接收短信，预期用于实现自动填写短信。
学生、教师登录后主界面显示头像，点击头像即可修改，在个人信息维护界面修改其他个人信息。学生可以对课程进行评价、查询自己的平时成绩和出勤情况。教师可以添加学生的考勤记录和平时作业成绩，管理员对学生、教师帐号进行管理。
## 系统功能结构
![图片](https://i.loli.net/2019/08/05/RhAuzmPq6yCdMrn.png)
## 各功能模块设计
1. Actity用作用户交互类
2. Service服务用作播放背景音乐
3. Provider内容提供者用作学生信息添加
4. Broadcast广播接收者接收短信
5. SQLite数据库用于存储数据
6. XML解析用于注册中的地区选择
7. 网络通讯用于调用接口发送短信
## 线程通信：基于云服务的API短信验证模块
本功能在教材中网络编程的案例改进，仅作学习没有存储传输手机号、短信的数据
1. 在ASP .Net Core MVC 框架中为短信功能编写简单的网络接口提供给安卓调用
![图片](https://i.loli.net/2019/08/05/a7dvh3bOu9NjfUP.png)

2. 备案域名部署在服务器中，短信服务需要使用Https通信，所以服务器需要安装SSL证书使使用https加密
![图片](https://i.loli.net/2019/08/05/nTH3uxaAwKItJGR.png)

3. 根据后端框架规则，此接口URL为https://teresalanguagecenter.cn/home/SMSforAndroid
根据教材编写HttpUnits类调用此接口，因为url是https的所以连接方法不用HttpURLConnection而使用HttpsURLConnection
4. 发送android.os.NetworkOnMainThreadException错误，因为在Android4.0以后，只要是写在主线程中的HTTP请求，运行时都会报错，这是因为Android在4.0以后为了防止应用的ANR（Aplication Not Response）异常。
在Activity的onCreate()方法中加入这样一段代码可以解决此问题：
```java
if (android.os.Build.VERSION.SDK_INT > 9) {
    StrictMode.ThreadPolicy policy = new StrictMode.ThreadPolicy.Builder().permitAll().build();
    StrictMode.setThreadPolicy(policy);
}
```
大功告成😊成就感满满!
![图片](https://i.loli.net/2019/08/05/BkzUoIjxrg1idOT.png)
