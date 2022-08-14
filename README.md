
# ALittlePay - 一个开源免费的挂机支付监控项目 （代码已完全提交，有问题加群或者issues反馈）
```
拒绝第三方易支付圈钱跑路,拒绝游戏支付回收困难，拒绝码支付收费和云端挂机，作者随时都能替换二维码割韭菜，自建支付系统和自己挂机，代码完全透明，没有暗门，保证笔笔资金都由自己掌控
```

ALittlePay是一个基于Thinkphp后端框架和Bootstrap前端框架开发的个人挂机支付系统。它是专门个人用户使用APP监控支付而创建的，以服务无法开通商家收款的用户。ALittlePay保持了Thinkphp的原始特性，允许开发人员以最小的学习成本和努力编写更多模块
          
- 基于Thinkphp后端框架
- 基于Bootstrap前端框架
- 精简的admin&api应用分类
- 自动且精准判断`微信`或`支付宝`收款通知
- 安全的接口权限审计
- 独特的数据锁让数据无误差记录
- Redis应用让数据处理更加高效
- 配套APP监听,无绑定无授权,真正的免费(仅免费使用,内置监听插件为第三方插件,约束不可开源代码)
- 完善的交易分析,带有详细的支付调试功能和记录
- 独特的1+1+N监控设计,1个网站1个用户N台设备轮训收款,并带有独特的价格浮动判断
## 演示图
-后台&APP
![image](https://user-images.githubusercontent.com/110278132/182033038-dc185fe5-02c2-490d-8786-33580da2f0b8.png)
![image](https://user-images.githubusercontent.com/110278132/182033076-c45f0159-ec81-475e-95df-0c777b42f9d2.png)
![image](https://user-images.githubusercontent.com/110278132/182033101-2a0be17d-b606-4f40-bfce-8803e3f8c0db.png)

![15f260aa9b3192c9439053f010c46ae](https://user-images.githubusercontent.com/110278132/182033138-1a69858d-be10-4e63-a271-f1d2da06bdc4.jpg)

## Doc 
- [中文文档(暂未开放)](https://github.com/alittletry/alittlepay)

## 推荐服务器（虚拟空间不支持）
服务器环境推荐要求：

```
Nignx/Apache/IIS
PHP 7.1 ~ 7.4
MySQL 5.7
Redis 且php安装redis拓展
```
## 安装教程

1. 下载代码

    打开命令行，输入以下命令
    ```bash
    git clone https://github.com/alittletry/alittlepay.git
    cd alittlepay
    
    或者
    下载压缩包,自行解压
    ```
    
2. 配置数据库：
    * 后端数据库参考TP6填写参数
    * 将根目录下.dev.env文件改名为.env并将文件中中数据库和redis配置进行填写
    * 将根目录下init.sql文件导入自己的数据库中


3. 打开后台

    注意：1 2步完成后，需要设置网站运行目录为public并且配置对应的thinkphp伪静态，具体参考下方图片(Nginx为例)
    ![image](https://user-images.githubusercontent.com/110278132/182032968-9fb9e7b9-67c6-4952-9bd7-3687d19b4cbd.png)
    ![image](https://user-images.githubusercontent.com/110278132/182032974-acba2e47-c4a9-496d-ac6e-dbb4a2e6d325.png)
    ```bash
    location / {
	if (!-e $request_filename){
		rewrite  ^(.*)$  /index.php?s=$1  last;   break;
	}
    }
    ```
    ```bash
    打开 http://你的域名/admin/login/login
    默认账号密码：admin/123456
    ```
    
4. 配置后台
   ```bash
    点击系统设置->网站配置  
          ->基本->修改网站的商户ID、商户秘钥、监听秘钥
          ->邮件->填写你的邮箱配置和接收邮箱及接收内容设置(非必要设置)
          
    点击通道管理->新增通道
          通道名称->自定义，用来自我判断通道的名称
          二维码链接->收款二维码链接(字符串),微信请自行解析收款码变成wxp://开头的字符串，支付宝请输入userid(登录支付宝PC网站,在个人中心邮件网页查看源代码中搜索userid即可找到)
          通道类型->选择微信或支付宝
          每日限额->单位元,只支持整数,0为不限制，到达额度后该通道在第二天0点之前不会再生成支付
          浮动类型->上下浮动 向上浮动 向下浮动 ，例如上下浮动，每笔交易超时为5分钟,当5分钟内同时有3个人获取该通道相同金额10元，则系统会自动分配为10.00 10.01 9.99这样上下浮动
          浮动次数->假设金额10元 浮动单位0.01元  次数为3 则该通道能给出的支付金额为 9.97 9.98 9.99  10  10.01 10.02 10.03  上下浮动3次
          浮动单位->单位为元，需输入两位浮点数,如0.01 代表每次浮动如上述举例,0.03则为 10.00 10.03 9.97
          通道状态->自行选择 禁用后将不在使用
          通道轮训->默认参与，不参与逻辑暂未开发
    ```      
5. 配置前台
     ```bash
    1.下载APK到手机安装(本app需开启通知权限)
    2.
    3.点击齿轮配置，然后保存并连接
        服务器域名(带http 结尾不带/)
        服务器通讯key(如上配置的监听秘钥)
        支付宝通道标识
        微信通道标识  (支付宝和微信至少填一个，具体填写请看下这个手机上的支付宝和微信对应自己添加的支付通道所生成的通道标识 如 10003)
        
    3.服务器连接成功后，点击播放按钮，再按照app弹窗中1 2 3号按钮一次按一遍即可
        先初始化
        再检查获取通知权限(首次安装app会自动跳转设置，注意是获取通知权限，非通知权限)
        开始监听
        检测电池白名单(非必要，部分手机不支持,如果不支持请自行设置屏幕常亮且不锁屏)
        
    4.测试
        再次点击播放按钮，点击最下方 发送测试监听 系统会自动推送一条通知，并抓取在监控日志中
        
    5.app说明,本app无机密信息,无木马,可自行杀毒，不放心者请自行杀毒或干脆别用
   ```      
6. 说明
   
   后端tp6的float类型容易有bug，尤其是在金额两位小数的精准运算和判断大小上,如有问题请及时联系群管理或issue反馈

## 开源协议&免责声明 
- ALittlePay是一款不以盈利为目的的开源挂机支付系统。程序的著作权均归作者所有，用户具有自由的学习权(法律范围内,用户仅享用学习权和自我测试权，如需商用请自行申请商家支付，个人支付仅用于个人测试和学习)。用户在使用本系统时造成对用户自己或他人任何形式的损失和伤害，作者不承担任何责任,代码公开审计,如有问题请自行删除程序。 本系统只提供做测试学习，用户使用本系统从事任何违法违规的事情，一切后果由用户自行承担，作者不承担任何责任。

## Others 
- QQ交流群
    - VIP群 878074065 （本群需要付费99元）暂未开放
    - ALittlePay官方一群 683331712(未满)
    
    -加群获取apk文件,入群验证信息填写你的github用户名(需要右上角点个star支持哦)
    
- 商业支持：
    - QQ 444797006
    - EMAIL km8prhp@gmail.com

