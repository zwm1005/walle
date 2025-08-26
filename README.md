瓦力机器人DIY
#### qq交流群：757384775

作者：B站up 李不胖谁胖

#### 概述

简介：

​		相信童年看过《机器人瓦力》电影的小伙伴一定都想拥有一台属于自己的瓦力，本教程就DIY瓦力所需的物料和控制程序开始，带着大家一起DIY一个瓦力机器人。我想没有男孩子能抵抗的住这集成声、光、电、动的小机器人玩具吧，寒假带着娃做一个，把隔壁小孩馋哭  哈哈。

#### 物料清单(BOM)

| 序号 | 模块                           | 功能                       | 数量 | 参考价格（单价） | 参考价格（总价） | 备注                                        |
| ---- | ------------------------------ | -------------------------- | ---- | ---------------- | ---------------- | ------------------------------------------- |
| 1    | Arduino UNO R3开发板           | 主控芯片                   | 1    | 16.4             | 16.4             |                                             |
| 2    | ESP32 C3开发板                 | MQTT协议连接服务器         | 1    | 9.9              | 9.9              |                                             |
| 3    | L298N 双H桥点击驱动板          | 驱动履带电机               | 1    | 6.4              | 6.4              |                                             |
| 4    | PCA9685 16路舵机驱动板         | 舵机驱动                   | 1    | 12.5             | 12.5             |                                             |
| 5    | 随身WIFI                       | 提供WIFI                   | 1    | 15               | 15               |                                             |
| 6    | DC-DC 12V 转5V 直流降压模块    | 12V 转 5V供电              | 1    | 5.9              | 5.9              |                                             |
| 7    | 12V 370偏轴减速电机 107转/分钟 | 驱动履带                   | 2    | 15               | 30               | 注意要买偏轴的，不是中置的轴                |
| 8    | 18650电池盒(3节)               | 电池仓                     | 1    | 1.2              | 1.2              |                                             |
| 9    | 18650电池                      | 串联提供12V电源            | 3    | 8                | 24               |                                             |
| 10   | 18650充电器                    |                            | 1    | 10               | 10               |                                             |
| 11   | SG90 9g舵机                    | 驱动运动关节               | 7    | 3.79             | 26.53            |                                             |
| 12   | DC电源插头5.5mm（公头）        | 电池仓电源输出接头         | 2    | 0.3              | 0.6              |                                             |
| 13   | PETG-ECO 黑色                  | 打印履带、电机座           | 1    | 35               | 35               | 实际总用量大概1.2kg左右吧，毛估没有具体算过 |
| 14   | PETG-ECO 灰色                  | 打印瓦力头部               | 1    | 35               | 35               |                                             |
| 15   | PETG-ECO 卡特黄                | 打印瓦力身体               | 1    | 35               | 35               |                                             |
| 16   | ASR PRO语音模块（带喇叭）      | 语音识别                   | 1    | 28.9             | 28.9             | 语音控制（可选）                            |
| 17   | 1.3寸TFT彩屏                   | 天气时钟屏幕               | 1    | 12.9             | 12.9             | 天气时钟使用（可选）                        |
| 18   | ESP8266开发板                  | 天气时钟控制芯片           | 1    | 13.3             | 13.3             | 天气时钟使用（可选）                        |
| 19   | 5V 激光模块                    | 眼睛                       | 1    | 0.65             | 0.65             | 可选                                        |
| 20   | WIFI摄像头                     | 网络传输视频               | 1    | 20               | 20               | 可选                                        |
| 21   | Mini Mp3 Player                | 播放音乐                   | 1    | 3.8              | 3.8              | 可选                                        |
| 22   | MFRC-522 RFID模块              | NFC无线识别                | 1    | 3.9              | 3.9              | 可选                                        |
| 23   | DHT11温度传感器                | 测量温度                   | 1    | 2.5              | 2.5              | 可选                                        |
| 24   | 船型开关                       | 电源开关                   | 1    | 0.5              | 0.5              | 可选                                        |
| 25   | 3W 8R 喇叭                     | 播放音乐                   | 1    | 4.2              | 4.2              | 可选                                        |
| 26   | 1u2g云服务器                   | MQTT服务器、部署H5控制页面 | 1    | 69               | 69               | 可选                                        |
| 27   | 丙烯颜料(12种盒装)             | 瓦力上色                   | 1    | 1.8              | 1.8              | 可选                                        |
|      |                                |                            |      | 合计             | 424.88           |                                             |

> 如果只想完成基础功能，实现履带底盘的行进和关节运动的话硬件部分成本大概在160元，扩展的模块可根据自己需要选择性购买即可。

##### 一下是Diy必备的一些工具和常见的小物品就不算在内了

| 序号 |      工具&物品名称       |
| :--: | :----------------------: |
|  1   |         热熔胶枪         |
|  2   |          万用表          |
|  3   |          电烙铁          |
|  4   |          老虎钳          |
|  5   |       TF卡、读卡器       |
|  6   |          回形针          |
|  7   |       M3螺柱 螺母        |
|  8   |         502胶水          |
|  9   | 杜邦线（公对母、母对母） |



##### 使用或涉及的技术栈

1. Arduino IDE  开发环境

2. Element Plus + Vue3 + Vite + TypeScript  自建H5控制页面

3. Linux + Nginx + Docker + EMQX  后台服务器

   

#### 一、Web控制端

##### 1.1 Arduino IDE开发环境搭建

Arduino官网  https://www.arduino.cc/

Arduino中文社区 https://arduino.me/

1. 安装Arduino软件

   - 中文社区提供的安装教程 https://arduino.me/s/arduino-getting-started?aid=125

2. 添加开发板管理器   文件 -> 首选项 -> 其他开发板管理器地址 将如下链接填入

   > 一下是Arduino中文社区提供的index 会比github上下载的快些，github可能会遇到下载失败的情况

```json
http://arduino.me/packages/esp8266.json
https://arduino.me/packages/esp32.json
```

  3.安装Blinker库 在线安装失败 请尝试离线安装包 https://www.diandeng.tech/doc/getting-start-esp32-wifi

> Blinker 首页 https://www.diandeng.tech/home
>
> Blinker 的github仓库地址可以自行下载最新支持包 https://github.com/blinker-iot
>
> Blinker 官网提供的安装包 https://www.diandeng.tech/sdk/blinker-library-0.3.10230510.zip

  4.安装 ESP32、ESP8266 开发板库，直接点击分享的对应库的exe文件安装既可

  5.安装USB串口驱动，这个要看你所使用的开发板上下载芯片的型号，合宙的ESP32 C3要安装CH343驱动，常见的是CH34X、CP21XX芯片等，文件里分享了这两种，如果没有的话请搜索你开发板的串口芯片型号，下载对应驱动后安装。

##### 1.2 使用Blinker App 作为控制端

1. 新建Blinker独立设备、复制设备秘钥替换代码中的Blinker设备秘钥。

   ```c++
   // Blinker设备秘钥
   char auth[] = "xxxxxxxx";
   ```

   

2. 更新Blinker App 瓦力设备的界面配置，使用如下代码进行替换。

> 瓦力界面配置代码：

```
{¨version¨¨2.0.0¨¨config¨{¨headerColor¨¨transparent¨¨headerStyle¨¨dark¨¨background¨{¨img¨¨assets/img/headerbg.jpg¨¨isFull¨«}}¨dashboard¨|{¨type¨¨tex¨¨t0¨¨blinker入门示例¨¨t1¨¨文本2¨¨bg¨Ë¨ico¨´´¨cols¨Í¨rows¨Ê¨key¨¨tex-272¨´x´É´y´É¨speech¨|÷¨lstyle¨Ê¨clr¨¨#FFF¨}{ßC¨btn¨ßJ¨fas fa-arrow-alt-down¨¨mode¨ÉßE¨后退¨ßGßHßIÉßKËßLËßM¨btn-back¨´x´Ì´y´¤FßPÉßQ¨#076EEF¨}{ßCßSßJ¨fas fa-arrow-alt-up¨ßUÉßE¨前进¨ßGßHßIÉßKËßLËßM¨btn-go¨´x´Ì´y´¤BßQßXßPÉ}{ßCßSßJ¨fal fa-power-off¨ßUÉßE¨急停¨ßGßHßIÉßKËßLËßM¨btn-stop¨´x´Ì´y´¤DßQ¨#EA0909¨ßPÉ}{ßCßSßJ¨fas fa-arrow-alt-right¨ßUÉßE¨向右¨ßGßHßIÉßKËßLËßM¨btn-right¨´x´Î´y´¤DßPÉßQßX}{ßCßSßJ¨fas fa-arrow-alt-left¨ßUÉßE¨向左¨ßGßHßIÉßKËßLËßM¨btn-left¨´x´Ê´y´¤DßQßXßPÉ}{ßCßSßJ¨fad fa-arrow-alt-circle-up¨ßUÉßE¨抬脖子¨ßGßHßIÉßKËßLËßM¨btn-nick-up¨´x´Ì´y´ÏßQ¨#FBA613¨}{ßCßSßJ¨fad fa-arrow-alt-circle-down¨ßUÉßE¨低脖子¨ßGßHßIÉßKËßLËßM¨btn-nick-down¨´x´Ì´y´ÑßQßoßPÉ}{ßCßSßJ¨fad fa-arrow-alt-up¨ßUÉßE¨左手上¨ßGßHßIÉßKËßLËßM¨btn-left-hand-up¨´x´É´y´ÒßQ¨#00A90C¨ßPÉ}{ßCßSßJßsßUÉßE¨右手上¨ßGßHßIÉßKËßLËßM¨btn-right-hand-up¨´x´Ï´y´ÒßQßv}{ßCßSßJ¨fad fa-arrow-alt-down¨ßUÉßE¨左手下¨ßGßHßIÉßKËßLËßM¨btn-left-hand-down¨´x´É´y´¤BßPÉßQßv}{ßCßSßJßyßUÉßE¨右手下¨ßGßHßIÉßKËßLËßM¨btn-right-hand-down¨´x´Ï´y´¤BßQßv}{ßCßSßJ¨fad fa-arrow-alt-circle-left¨ßUÉßE¨左转头¨ßGßHßIÉßKËßLËßM¨btn-head-left¨´x´Ê´y´ÐßQßo}{ßCßSßJ¨fad fa-arrow-alt-circle-right¨ßUÉßE¨右转头¨ßGßHßIÉßKËßLËßM¨btn-head-right¨´x´Î´y´ÐßPÉßQßo}{ßCßSßJßyßUÉßE´低头´ßGßHßIÉßKËßLËßM¨btn-head-down¨´x´Ì´y´ÍßQßoßPÉ}{ßCßSßJßsßUÉßE´抬头´ßGßHßIÉßKËßLËßM¨btn-head-up¨´x´Ì´y´ËßQßo}{ßCßSßJ¨fad fa-user-robot¨ßUÉßE¨动作1¨ßGßHßIÉßKËßLËßM¨btn-act1¨´x´É´y´ÎßQßXßPÉ}{ßCßSßJ¨fad fa-redo-alt¨ßUÉßE¨动作复位¨ßGßHßIÉßKËßLËßM¨btn-reset¨´x´Ï´y´ÎßPÉßQßX}{ßCßSßJ¨fad fa-lightbulb-on¨ßUÉßE´激光´ßGßHßIÉßKËßLËßM¨btn-led¨´x´É´y´ÌßPÉßQße}{ßCßSßJ¨fad fa-thermometer-three-quarters¨ßUÉßE´温度´ßGßHßIÉßKËßLËßM¨btn-tem¨´x´Ï´y´ÌßPÉßQßv}{ßC¨deb¨ßUÉßIÉßKÑßLÌßM¨debug¨´x´É´y´¤H}÷¨actions¨|¦¨cmd¨¦¨switch¨‡¨text¨‡´on´¨打开?name¨¨off¨¨关闭?name¨—÷¨triggers¨|{¨source¨ß1P¨source_zh¨¨开关状态¨¨state¨|´on´ß1S÷¨state_zh¨|´打开´´关闭´÷}÷´rt´|÷}

```

   3.烧写控制代码到ESP32 C3开发板

​	合宙ESP32 C3 开发板官方文档 https://wiki.luatos.com/chips/esp32c3/index.html

​	如果使用的开发板不是ESP32 C3 请自行替换引脚定义，否则编译可能会出现报错。

代码中需要替换以下内容

```c++
// Blinker设备秘钥
char auth[] = "xxxxxxxx";
// WIFI名称
char ssid[] = "xxxxxx";
// WIFI密码
char pswd[] = "xxxxx";
```

更改设备秘钥和wifi名称密码后，将ESP32_Blinker.ino 烧录到ESP32 C3开发板上



> 烧录之前请确保Arduino IDE已安装如下库：

```json
Blinker
DHT sensor library
Adafruit Unified Sensor
EspSoftwareSerial
```



> 使用合宙ESP32 C3 烧录程序有一下几点需要注意，否则可能会导致烧录失败：
>
> 1.Flash Mode选择 "DIO" 模式。
>
> 2.USB CDC On Boot 选择 "Enabled" 方便串口调试。
>
> 3.按住板载boot键上电进入下载模式，此时两个板载led微亮。
>
> 4.如果烧录完成，板载的灯没有亮起，还是微亮状态，请按下reset键重启，或者给开发板重新上电。



##### 1.3 使用自建H5页面作为控制端

- 本节需要在Linux环境下使用Docker安装EMQTX，并使用Nginx作为Web服务器部署瓦力的H5控制页面。
- EMQX官方文档：https://www.emqx.io/docs/zh/latest/deploy/install-docker.html
- EMQX Vue3示例：https://github.com/emqx/MQTT-Client-Examples/tree/master/mqtt-client-Vue3.js



1.安装Docker

```shell
#安装相关工具
yum install -y yum-utils device-mapper-persistent-data lvm2
#配置清华yum源
yum-config-manager --add-repo https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/docker-ce.repo
#安装Docker
yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin
#启动Docker
systemctl start docker
#查看Docker版本号
docker -v
```

2.Docker安装 EMQX

```shell
#拉取镜像
docker pull emqx/emqx:5.4.1
#启动EMQX
docker run -d --name emqx -p 1883:1883 -p 8083:8083 -p 8084:8084 -p 8883:8883 -p 18083:18083 emqx/emqx:5.4.1
#开启服务器的1883、8083、8084、8883端口
```

3.安装Nginx

```shell
yum install -y nginx
#启动 nginx
systemctl start nginx
```

 4.部署H5控制页面

```shell
#安装unzip
yum install -y unzip
   
#新建文件夹用于存放H5页面
cd /home
mkdir emqx
   
#上传dist.zip到 /home/emqx文件夹中
   
#解压dist.zip
unzip /home/emqx/dist.zip
   
#Nginx增加如下配置
#域名 xxxx.xxx
  server {
          listen      80;
          server_name xxxx.xxx;
          location / {
              client_max_body_size 200m;
              root   /home/emqx/dist;
              index  index.html index.htm;
          }
          error_page   500 502 503 504  /50x.html;
          location = /50x.html {
               root   html;
          }
      }
   
#6.重启Nginx
 nginx -s reload

```



   3.烧录ESP32_H5.ino程序到ESP32中

- 打开 ESP32_H5.ino 文件，替换mqtt链接相关的参数

```c++
   // MQTT Broker
   const char *mqtt_broker = "服务器ip或域名";
   const char *topic = "订阅的topic";
   const char *mqtt_username = "";
   const char *mqtt_password = "";
   const int mqtt_port = 1883;

```

- 参数替换完成后，烧录程序到ESP32中，打开串口监视器查看日志。
- 打开H5控制页面，测试发送指令，并在串口监视器中查看。

##### 考虑到这里部署H5页面对没接触过Linux的初学者可能会有些挑战、还有些小伙伴没有服务器的情况，所以如果不想搭建的话可以使用我分享出来的网站和MQTT服务器直接使用，为防止多个小伙伴一起使用时，串Topic的情况，使用时请务必按照格式替换Topic

```c++
H5控制页面地址：https://walle.werfamily.fun
mqtt服务器：t.werfamily.fun
Topic 替换为自己的topic防止与其他小伙伴冲突 格式为 bili/B站UID

   // MQTT Broker
   const char *mqtt_broker = "t.werfamily.fun";
   const char *topic = "bili/你的B站UID";
   const char *mqtt_username = "";
   const char *mqtt_password = "";
   const int mqtt_port = 1883;

```

自己搭建：需要替换如下，协议、服务器地址、端口

![](https://ipai-saas.obs.cn-east-3.myhuaweicloud.com:443/efdf7161-f577-4266-997a-1dca01034cc4.png)



使用我搭建好的：topic请输入自己的B站UID

![](https://ipai-saas.obs.cn-east-3.myhuaweicloud.com:443/0599699b-9c46-49b0-a7fd-54446ab203cc.png)



为了防止小伙伴订阅的主题重复冲突，所以使用时务必要将UID替换为自己的B站UID

> B站UID查看的方法为 进入"我的主页"  ->  点击空间  -> 点击详情

#### 如何获取UID?

##### 电脑浏览器查看自己的UID 在个人资料中

![](https://ipai-saas.obs.cn-east-3.myhuaweicloud.com:443/88f4b033-444e-4e23-b451-67bfdcdbe401.png)

##### 手机端查看自己的UID

第一步

![](https://ipai-saas.obs.cn-east-3.myhuaweicloud.com:443/18c02f95-9411-464b-acef-c22d6b431d11.png)

第二步

![](https://ipai-saas.obs.cn-east-3.myhuaweicloud.com:443/9ebab89c-2568-49b2-98fd-b8231f190fdf.png)



第三步

![](https://ipai-saas.obs.cn-east-3.myhuaweicloud.com:443/065aa929-a2d2-4b6b-adc3-36daa4af7433.png)



### 二、履带电机控制

##### 2.1 履带电机使用的是12V偏轴107转/分钟的减速电机，电机驱动使用的是双H桥L298N驱动板

L298N资料：

| IN1  | IN2  | ENA（A） | 电机A状态 |
| ---- | ---- | -------- | --------- |
| 0或1 | 0或1 | 0        | 停止      |
| 1    | 0    | 1        | 顺时针    |
| 0    | 1    | 1        | 逆时针    |
| 0    | 0    | 1        | 制动      |
| 1    | 1    | 1        | 制动      |

| IN3  | IN4  | ENA（B） | 电机B状态 |
| ---- | ---- | -------- | --------- |
| 0或1 | 0或1 | 0        | 停止      |
| 1    | 0    | 1        | 顺时针    |
| 0    | 1    | 1        | 逆时针    |
| 0    | 0    | 1        | 制动      |
| 1    | 1    | 1        | 制动      |

这里我们不使用PWM调速，ENA(A) ENA(B)默认短接就是最大转速。

控制方式：两个履带电机正转为前进，都反转为后退，一正转一反转为转向。

| IN1  | IN2  | IN3  | IN4  | 行进状态 |
| ---- | ---- | ---- | ---- | -------- |
| 1    | 1    | 1    | 1    | 停止     |
| 0    | 0    | 0    | 0    | 停止     |
| 1    | 0    | 1    | 0    | 前进     |
| 0    | 1    | 0    | 1    | 后退     |
| 1    | 0    | 0    | 1    | 右转     |
| 0    | 1    | 1    | 0    | 左转     |

##### 2.2 电路连接：

DC12V降压模块的12V 连接 L298N 的12V输入来给L298N供电。

```c++
L298N模块                           Arduino UNO开发板

  VN1     --------------------------      6

  VN2     --------------------------      7

  VN3     --------------------------      8

  VN4     --------------------------      9

```



```c++
ESP32 C3开发板                      Arduino UNO开发板

   GND    --------------------------     GND

   5V     --------------------------      5V

   IO00   --------------------------      TX

   IO01   --------------------------      RX

```



##### 2.3 Arduino UNO代码

百度网盘链接：https://pan.baidu.com/s/1HNKWE4Z2C2nG0kT3WxBQWA?pwd=zm6l

```c++
/**
 * @author     B站 李不胖谁胖
 * @version    1.0.0
 * @date       2024-01-14
 */
// 前进
void handleForward(){
  digitalWrite(6, HIGH);
  digitalWrite(7, LOW);
  digitalWrite(8, HIGH);
  digitalWrite(9, LOW);
}

// 后退
void handleFallback(){
  digitalWrite(6, LOW);
  digitalWrite(7, HIGH);
  digitalWrite(8, LOW);
  digitalWrite(9, HIGH);
}

// 左转
void handleTurnLeft(){
  digitalWrite(6, HIGH);
  digitalWrite(7, LOW);
  digitalWrite(8, LOW);
  digitalWrite(9, HIGH);
}

// 右转
void handleTurnRight(){
  digitalWrite(6, LOW);
  digitalWrite(7, HIGH);
  digitalWrite(8, HIGH);
  digitalWrite(9, LOW);
}

// 停止
void handleStop(){
  digitalWrite(6, HIGH);
  digitalWrite(7, HIGH);
  digitalWrite(8, HIGH);
  digitalWrite(9, HIGH);
}

// 处理动作
void handleMotorAction(char commend){
  if(commend == '\n' || commend == '\r'){
    return;
  }
  Serial.print("commend:");
  Serial.print(commend);
  Serial.println();

  switch (commend){
    case 'w':
        // 如果指令为前进
        handleForward();
        break;
    case 's':
        // 如果指令为后退
        handleFallback();
        break;
    case 'a':
        // 如果指令为左转
        handleTurnLeft();
        break;
    case 'd':
        // 如果指令为右转
        handleTurnRight();
        break;
    case 'q':
        // 如果指令为停止
        handleStop();
        break;
    default:
        // 如果不匹配任何指令 不进行操作
        break;
  }
}

void setup() {
  // put your setup code here, to run once:
  // 初始化串口
  Serial.begin(115200);
  // 定义6、7、8、9引脚模式
  pinMode(6, OUTPUT);
  pinMode(7, OUTPUT);
  pinMode(8, OUTPUT);
  pinMode(9, OUTPUT);
  // 初始化时履带为停止状态 所以给6、7、8、9引脚输出高电平
  digitalWrite(6, HIGH);
  digitalWrite(7, HIGH);
  digitalWrite(8, HIGH);
  digitalWrite(9, HIGH);
}

void loop() {
  // put your main code here, to run repeatedly:
  // 如果串口接收到了控制指令则读取指令并做出相应的动作
  if (Serial.available() > 0) {
    char commend =  Serial.read();
     handleMotorAction(commend);
  }
}

```



#### H5页面控制方式的ESP32 C3代码（推荐，连接很稳定）

百度网盘链接：https://pan.baidu.com/s/1HEGymdvOeyf_LJFmb-WZXg?pwd=qh1i

```c++
/**
 * @author     B站 李不胖谁胖
 * @version    1.0.0
 * @date       2024-01-14
 */
#include <WiFi.h>
#include <PubSubClient.h>
#include <SoftwareSerial.h>
#include <ArduinoJson.h>  // JSON解析库
#include <string.h>
SoftwareSerial uart1(6,7);//RX=6,TX=7
// WiFi
const char *ssid = "VACA"; // Enter your WiFi name
const char *password = "vaca9999";  // Enter WiFi password

// MQTT Broker
const char *mqtt_broker = "t.werfamily.fun";
const char *topic = "bili/423212190";
const char *mqtt_username = "";
const char *mqtt_password = "";
const int mqtt_port = 1883;
// 合宙ESP32 C3板载Led引脚
const int board_led_left = 12;
const int board_led_right = 13;
WiFiClient espClient;
PubSubClient client(espClient);

void setup() {
 // 初始化串口
 Serial.begin(115200);
 uart1.begin(115200);
 Serial1.begin(115200, SERIAL_8N1, 0, 1);
 pinMode(board_led_left, OUTPUT);
 pinMode(board_led_right, OUTPUT);
 digitalWrite(board_led_left, HIGH);
 digitalWrite(board_led_right, LOW);
 pinMode(10, OUTPUT);
 digitalWrite(10, LOW);
 // 连接wifi
 WiFi.begin(ssid, password);
 while (WiFi.status() != WL_CONNECTED) {
     delay(500);
     Serial.println("Connecting to WiFi..");
 }
 Serial.println("Connected to the WiFi network");
 // 连接mqtt broker
 client.setServer(mqtt_broker, mqtt_port);
 client.setCallback(callback);
 while (!client.connected()) {
     String client_id = "esp32-client-";
     client_id += String(WiFi.macAddress());
     Serial.printf("The client %s connects to the public mqtt broker\n", client_id.c_str());
     if (client.connect(client_id.c_str(), mqtt_username, mqtt_password)) {
         Serial.println("Mqtt broker已连接");
     } else {
         Serial.print("Mqtt broker连接失败 state:");
         Serial.print(client.state());
         delay(2000);
     }
 }
 // 发送消息 
 client.publish(topic, "Hi EMQX I'm ESP32");
 // 订阅消息topic
 client.subscribe(topic);
}

// led交替闪烁
void setLed() {
  int currentLevelLeft = digitalRead(board_led_left);
  int currentLevelRight = digitalRead(board_led_right);
  if (currentLevelLeft == LOW) {
    digitalWrite(board_led_left, HIGH);
  }
  if (currentLevelLeft == HIGH) {
    digitalWrite(board_led_left, LOW);
  }
  if (currentLevelRight == LOW) {
    digitalWrite(board_led_right, HIGH);
  }
  if (currentLevelRight == HIGH) {
    digitalWrite(board_led_right, LOW);
  }
}

// 处理瓦力眼睛红色激光灯
void handlerRedLed(String msg) {
  String ledLow = "0";
  String ledHigh = "1";
  if (ledHigh == msg) {
    digitalWrite(10, HIGH);
  }
  if (ledLow == msg) {
    digitalWrite(10, LOW);
  }
}

// mqtt回调
void callback(char *topic, byte *payload, unsigned int length) {
  Serial.print("Message arrived in topic: ");
  Serial.println(topic);
  Serial.print("Message:");
  String msg = "";
  for (int i = 0; i < length; i++) {
    msg.concat((char)payload[i]);
  }
  Serial.println(msg);
  // 解析JSON消息
  char *msgBody;                                               // 创建指向char类型的指针变量
  int len = msg.length();                                      // 获取字符串长度
  msgBody = new char[len + 1];                                 // 分配足够大小的内存空间（加上结尾的'\0'）
  strcpy(msgBody, msg.c_str());                                // 将字符串复制到char*变量中
  const size_t capacity = JSON_OBJECT_SIZE(1) + 60;            // 根据JSON结构调整容量大小
  StaticJsonDocument<256> doc;                                 // 创建一个静态大小为 256 字节的 JsonDocument 对象
  DeserializationError error = deserializeJson(doc, msgBody);  // 将 JSON 字符串反序列化到 JsonDocument
  if (error) {
    Serial.print("deserializeJson() failed with code ");
    Serial.println(error.c_str());
    return;
  }
  String commend = doc["commend"];  // 获取名称属性值
  String value = doc["value"];      // 获取年龄属性值
  Serial.print("指令: ");
  Serial.println(commend);
  Serial.print("参数值: ");
  Serial.println(value);
  // 处理瓦力眼睛红色激光灯
  handlerRedLed(commend);
  // 向arduino uno透传数据
  Serial1.println(commend);
  // 板载Led交替闪亮
  setLed();
}

void loop() {
  client.loop();
  // 接收到语音模块的之后后向Arduino UNO透传指令
  if(uart1.available()>0){
    char commend = uart1.read();
    Serial1.println(commend);
  }
}

```



#### Blinker控制方式的ESP32 C3代码（连接可能不稳定）

百度网盘链接：https://pan.baidu.com/s/1OK_kX1OvmjliplcLMSUIGQ?pwd=e1in

```c++
/**
 * @author     B站 李不胖谁胖
 * @version    1.0.0
 * @date       2024-01-14
 *
 * 如何使用:
 * 1. 安装Blinker库
 *    a. 在Arduino IDE中 点击项目->导入库->管理库 搜索Blinker
 * 2. 上传代码至 合宙ESP32 C3
 */
#define BLINKER_WIFI
#include <Blinker.h>
#include <SoftwareSerial.h>
// 温度模块
#include "DHT.h"
#define DHTTYPE DHT11   // DHT 11
#define DHTPIN 4  // 温度模块引脚
DHT dht(DHTPIN, DHTTYPE);
// RX, TX 软串口用于连接语音模块串口 
SoftwareSerial asrProSerial(4, 5); 
// Blinker设备秘钥
char auth[] = "ecfebe6af444";
// WIFI名称
char ssid[] = "TP-LINK_HA";
// WIFI密码
char pswd[] = "libaobao";

// 合宙ESP32 C3板载Led引脚
const int board_led_left = 12;
const int board_led_right = 13;

// 新建控制组件对象
BlinkerButton Button1("btn-abc");
// 前进
BlinkerButton Button_GO("btn-go");
// 后退
BlinkerButton Button_BACK("btn-back");
// 左转
BlinkerButton Button_LEFT("btn-left");
// 右转
BlinkerButton Button_RIGHT("btn-right");
// 停止
BlinkerButton Button_STOP("btn-stop");
// 左手上抬
BlinkerButton Button_LEFT_HAND_UP("btn-left-hand-up");
//左手下
BlinkerButton Button_LEFT_HAND_DOWN("btn-left-hand-down");
// 右手上抬
BlinkerButton Button_RIGHT_HAND_UP("btn-right-hand-up");
//右手下
BlinkerButton Button_RIGHT_HAND_DOWN("btn-right-hand-down");
// 抬脖子
BlinkerButton Button_NICK_UP("btn-nick-up");
// 低脖子
BlinkerButton Button_NICK_DOWN("btn-nick-down");
// 抬头
BlinkerButton Button_HEAD_UP("btn-head-up");
// 低头
BlinkerButton Button_HEAD_DOWN("btn-head-down");
// 左转头
BlinkerButton Button_HEAD_LEFT("btn-head-left");
// 右转头
BlinkerButton Button_HEAD_RIGHT("btn-head-right");
// 温度
BlinkerButton Button_TEM("btn-tem");
// 激光
BlinkerButton Button_LED("btn-led");
// 动作1
BlinkerButton Button_ACT1("btn-act1");
// 动作重置
BlinkerButton Button_RESET("btn-reset");

// 按下前进按键即会执行该函数
void button_go_callback(const String & state)
{
  if (state=="tap") {
    Button_GO.print("前进");
    Serial1.println('w');
    delay(100);
  }
  if(state=="pressup"){
    Button_GO.print("前进结束");
    Serial1.println('q');
  }
  // 板载Led交替闪亮
  setLed();
  //digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
}

// 按下后退按键即会执行该函数
void button_back_callback(const String & state)
{
  if (state=="tap") {
    Serial1.println("s");
    delay(100);
    Button_BACK.print("后退");
  }
  if(state=="pressup"){
    Serial1.println("q");
    Button_BACK.print("后退结束");
  }
  // 板载Led交替闪亮
  setLed();
  //digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
}

// 按下左转按键即会执行该函数
void button_left_callback(const String & state)
{
  if (state=="tap") {
    Serial1.println("a");
    Serial.println("a");
    Button_LEFT.print("左转一下");
  }
  if(state=="pressup"){
    Serial1.println("q");
    Serial.println("q");
    Button_LEFT.print("左转结束");
  }
  // 板载Led交替闪亮
  setLed();
  //digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
}

// 按下右转按键即会执行该函数
void button_right_callback(const String & state)
{
  if (state=="tap") {
    Serial1.println("d");
    Serial.println("d");
    Button_RIGHT.print("右转一下");
  }
  if(state=="pressup"){
    Serial1.println("q");
    Serial.println("q");
    Button_RIGHT.print("右转结束");
  }
  // 板载Led交替闪亮
  setLed();
  //digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
}


// 按下停止按键即会执行该函数
void button_stop_callback(const String & state)
{
  if (state=="tap") {
    Serial1.println("q");
    Serial.println("q");
    Button_STOP.print("停止");
  }
  if(state=="pressup"){
    Serial1.println("q");
    Serial.println("q");
    Button_STOP.print("停止");
  }
  // 板载Led交替闪亮
  setLed();
  //digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
}


// 按下抬脖子执行该函数
void button_nick_up_callback(const String & state)
{
  if (state=="tap") {
    Serial1.println("k");
    Serial.println("k");
    Button_NICK_UP.print("抬脖子");
  }
  // 板载Led交替闪亮
  setLed();
  //digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
}

// 按下低脖子执行该函数
void button_nick_down_callback(const String & state)
{
  if (state=="tap") {
    Serial1.println("i");
    Serial.println("i");
    Button_NICK_DOWN.print("低脖子");
  }
  // 板载Led交替闪亮
  setLed();
  //digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
}

// 按下左转头执行该函数
void button_head_left_callback(const String & state)
{
  if (state=="tap") {
    Serial1.println("j");
    Serial.println("j");
    Button_HEAD_LEFT.print("左转头");
  }
  // 板载Led交替闪亮
  setLed();
  //digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
}

// 按下右转头执行该函数
void button_head_right_callback(const String & state)
{
  if (state=="tap") {
    Serial1.println("l");
    Serial.println("l");
    Button_HEAD_RIGHT.print("右转头");
  }
  // 板载Led交替闪亮
  setLed();
  //digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
}

// 按下右手上执行该函数
void button_right_hand_up_callback(const String & state)
{
  if (state=="tap") {
    Serial1.println("y");
    Serial.println("y");
    Button_RIGHT_HAND_UP.print("右手上");
  }
  // 板载Led交替闪亮
  setLed();
  //digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
}

// 按下右手下执行该函数
void button_right_hand_down_callback(const String & state)
{
  if (state=="tap") {
    Serial1.println("h");
    Serial.println("h");
    Button_RIGHT_HAND_DOWN.print("右手下");
  }
  // 板载Led交替闪亮
  setLed();
  //digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
}

// 按下左手上执行该函数
void button_left_hand_up_callback(const String & state)
{
  if (state=="tap") {
    Serial1.println("t");
    Serial.println("t");
    Button_LEFT_HAND_UP.print("左手上");
  }
  // 板载Led交替闪亮
  setLed();
  //digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
}

// 按下左手下执行该函数
void button_left_hand_down_callback(const String & state)
{
  if (state=="tap") {
    Serial1.println("g");
    Serial.println("g");
    Button_LEFT_HAND_DOWN.print("左手下");
  }
  // 板载Led交替闪亮
  setLed();
  //digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
}

// 按下抬头执行该函数
void button_head_up_callback(const String & state)
{
  if (state=="tap") {
    Serial1.println("r");
    Serial.println("r");
    Button_HEAD_UP.print("抬头");
  }
  // 板载Led交替闪亮
  setLed();
  //digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
}

// 按下低头执行该函数
void button_head_down_callback(const String & state)
{
  if (state=="tap") {
    Serial1.println("f");
    Serial.println("f");
    Button_LEFT_HAND_DOWN.print("低头");
  }
  // 板载Led交替闪亮
  setLed();
  //digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
}

// 按下动作1执行该函数
void button_act1_callback(const String & state)
{
  if (state=="tap") {
    Serial1.println("2");
    Serial.println("2");
    Button_LEFT_HAND_DOWN.print("动作1");
  }
  // 板载Led交替闪亮
  setLed();
  //digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
}

// 按下重置执行该函数
void button_reset_callback(const String & state)
{
  if (state=="tap") {
    Serial1.println("3");
    Serial.println("3");
    Button_LEFT_HAND_DOWN.print("重置");
  }
  // 板载Led交替闪亮
  setLed();
  //digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
}

// 按下温度执行该函数
void button_tem_callback(const String & state)
{
  if (state=="tap") {
    // 打印温度
    float h = dht.readHumidity();
    // Read temperature as Celsius (the default)
    float t = dht.readTemperature();
    String str1 = String(t, 1);
    String wendu = "温度";
    wendu.concat(str1);
    String str2 = String(h, 1);
    String shidu = "湿度";
    shidu.concat(str2);
    wendu.concat("\n");
    wendu.concat(shidu);
    Button_TEM.print(wendu);
  }
  // 板载Led交替闪亮
  setLed();
  //digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
}

// 按下激光执行该函数
void button_led_callback(const String & state)
{
  int currentLedLevel = digitalRead(10);
  if (state=="tap") {
    if(currentLedLevel == HIGH){
      digitalWrite(10, LOW);
      Serial.println("关闭激光");
      Button_LED.print("关闭激光");
    }
    if(currentLedLevel == LOW){
      digitalWrite(10, HIGH);
      Serial.println("开启激光");
      Button_LED.print("开启激光");
    }
  }
  
  // 板载Led交替闪亮
  setLed();
  //digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
}

// 接收到指令时 板载Led交替闪亮 方便查看
void setLed() {
  int currentLevelLeft = digitalRead(board_led_left);
  int currentLevelRight = digitalRead(board_led_right);
  if (currentLevelLeft == LOW) {
    digitalWrite(board_led_left, HIGH);
  }
  if (currentLevelLeft == HIGH) {
    digitalWrite(board_led_left, LOW);
  }
  if (currentLevelRight == LOW) {
    digitalWrite(board_led_right, HIGH);
  }
  if (currentLevelRight == HIGH) {
    digitalWrite(board_led_right, LOW);
  }
  delay(1000);
}

// 初始化板载led
void initLed(){
    pinMode(board_led_left, OUTPUT);
    pinMode(board_led_right, OUTPUT);
    digitalWrite(board_led_left, HIGH);
    digitalWrite(board_led_right, LOW);
}

void setup()
{
    Serial.begin(115200);
    // 初始化串口1 用于连接Arduino UNO 主控板
    Serial1.begin(115200, SERIAL_8N1, 0, 1);
    // 初始化软串口 用于连接语音模块接收语音识别控制信号 
    asrProSerial.begin(115200);
    // 初始化温度传感器
    dht.begin();
    // 初始化有LED的IO
    pinMode(LED_BUILTIN, OUTPUT);
    digitalWrite(LED_BUILTIN, HIGH);
   // 初始化激光引脚
    pinMode(10, OUTPUT);
    digitalWrite(10, LOW);
    // 初始化blinker
    Blinker.begin(auth, ssid, pswd);
    // 注册控制按钮组件
    // 前、后、左、右
    Button_GO.attach(button_go_callback);
    Button_BACK.attach(button_back_callback);
    Button_LEFT.attach(button_left_callback);
    Button_RIGHT.attach(button_right_callback);
    // 停止
    Button_STOP.attach(button_stop_callback);
    // 抬脖子
    Button_NICK_UP.attach(button_nick_up_callback);
    // 抬脖子
    Button_NICK_DOWN.attach(button_nick_down_callback);
    // 左转头
    Button_HEAD_LEFT.attach(button_head_left_callback);
    // 右转头
    Button_HEAD_RIGHT.attach(button_head_right_callback);
    // 抬头
    Button_HEAD_UP.attach(button_head_up_callback);
    // 低头
    Button_HEAD_DOWN.attach(button_head_down_callback);
    // 温度
    Button_TEM.attach(button_tem_callback);
    // 激光
    Button_LED.attach(button_led_callback);
    // 动作1
    Button_ACT1.attach(button_act1_callback);
    // 动作重置
    Button_RESET.attach(button_reset_callback);
    // 初始化板载led灯  12 13引脚
    initLed();
    BLINKER_DEBUG.stream(Serial);
}

void loop() {
    Blinker.run();
    // 语音识别模块是否发来了控制指令
    if(asrProSerial.available() > 0) {
     // 读取控制指令，并下发到Arduino UNO主控板
     char commend =  asrProSerial.read();
     Serial1.println(commend);
  }
}

```



### 三、舵机控制

舵机控制使用的是PCA9685 16路舵机驱动板



下面是电路连接图：

```c++
PCA9685舵机驱动板                   Arduino UNO开发板

  GND     --------------------------      GND

  SCL     --------------------------      A5

  SDA     --------------------------      A4

  VCC     --------------------------      5V


```

使用前请先确保Arduino IDE已安装了Adafruit PWM Servo Driver Library库

Arduino UNO舵机控制代码

百度网盘地址：链接：https://pan.baidu.com/s/1U4rW1MrFWgN5JhVQwd_Qtg?pwd=rghc

```c++
/**
 * @author     B站 李不胖谁胖
 * @version    1.0.0
 * @date       2024-01-20
 */
#include <Adafruit_PWMServoDriver.h>
// Servo shield controller class - assumes default address 0x40
Adafruit_PWMServoDriver pwm = Adafruit_PWMServoDriver();
//控制头俯仰
double servo_0_min_angle = 0;
double servo_0_max_angle = 120;
double servo_0_current_angle = 0;

//控制头旋转
double servo_1_min_angle = 45;
double servo_1_max_angle = 165;
double servo_1_current_angle = 105;

//控制脖子俯仰
double servo_2_min_angle = 0;
double servo_2_max_angle = 50;
double servo_2_current_angle = 0;

//控制左手
double servo_5_min_angle = 20;
double servo_5_max_angle = 100;
double servo_5_current_angle = 100;

//控制右手
double servo_7_min_angle = 100;
double servo_7_max_angle = 180;
double servo_7_current_angle = 100;

// 前进
void handleForward(){
  digitalWrite(6, HIGH);
  digitalWrite(7, LOW);
  digitalWrite(8, HIGH);
  digitalWrite(9, LOW);
}

// 后退
void handleFallback(){
  digitalWrite(6, LOW);
  digitalWrite(7, HIGH);
  digitalWrite(8, LOW);
  digitalWrite(9, HIGH);
}

// 左转
void handleTurnLeft(){
  digitalWrite(6, HIGH);
  digitalWrite(7, LOW);
  digitalWrite(8, LOW);
  digitalWrite(9, HIGH);
}

// 右转
void handleTurnRight(){
  digitalWrite(6, LOW);
  digitalWrite(7, HIGH);
  digitalWrite(8, HIGH);
  digitalWrite(9, LOW);
}

// 停止
void handleStop(){
  digitalWrite(6, HIGH);
  digitalWrite(7, HIGH);
  digitalWrite(8, HIGH);
  digitalWrite(9, HIGH);
}

// 控制舵机转动
void slowSetServoPulse(uint8_t n, double step, double current, double min, double max) {
    double min_pulse = getServoPulse(min);
    double max_pulse = getServoPulse(max);
    double current_pulse = getServoPulse(current);
    double target_pulse = getServoPulse(current + step);
    Serial.print("min_pulse:");
    Serial.println(min_pulse);
    Serial.print("max_pulse:");
    Serial.println(max_pulse);
    Serial.print("target_pulse:");
    Serial.println(target_pulse);
    Serial.print("current_pulse:");
    Serial.println(current_pulse);
    if(current_pulse == target_pulse){
      Serial.println("current_pulse == target_pulse:");
       return;
    }

    if(target_pulse > max_pulse || target_pulse < min_pulse){
      Serial.println("outof_pulse:");
       return;
    }
    
    if(current_pulse > target_pulse){
      for(int pulseLength = (int)(current_pulse - 1); pulseLength >= target_pulse ; pulseLength--){
        pwm.setPWM(n, 0, pulseLength);
        //servo_1_current_angle = pulseLength;
        Serial.print("--current_pulse:");
        Serial.println(pulseLength);
        delay(6);
      }
    }
    if(current_pulse < target_pulse){
      for(int pulseLength = (int)(current_pulse + 1); pulseLength <= target_pulse ; pulseLength++){
        pwm.setPWM(n, 0, pulseLength);
        //servo_1_current_angle = pulseLength;
        Serial.print("++current_pulse:");
        Serial.println(pulseLength);
        delay(6);
      }
    }
       Serial.print("servo:");
       Serial.println(n);
       Serial.print("current_angle:");
    if(n==0){
      servo_0_current_angle += step;
       Serial.println(servo_0_current_angle);
    }
    if(n==1){
      servo_1_current_angle += step;
       Serial.println(servo_1_current_angle);
    }
    if(n==2){
      servo_2_current_angle += step;
       Serial.println(servo_2_current_angle);
    }
    if(n==5){
      servo_5_current_angle += step;
       Serial.println(servo_5_current_angle);
    }
    if(n==7){
      servo_7_current_angle += step;
       Serial.println(servo_7_current_angle);
    }
    delay(100);
}

 double getServoPulse(double angle) {
  double pulse = angle;
   pulse = pulse/90 + 0.5;
   double pulselength;
   pulselength = 1000000;   // 1,000,000 us per second
   pulselength /= 50;   // 50 Hz
   pulselength /= 4096;  // 12 bits of resolution
   pulse *= 1000;
   pulse /= pulselength;
   Serial.print("getServoPulse pulse:");
   Serial.println(pulse);
   return pulse;
 }

 //设置9g舵机角度
 void servo_9g_write(uint8_t n,int Angle)
 {
   double pulse = Angle;
   pulse = pulse/90 + 0.5;
   setServoPulse(n,pulse);//0到180度映射为0.5到2.5ms
 }
 
  void setServoPulse(uint8_t n, double pulse) {
   double pulselength;
   pulselength = 1000000;   // 1,000,000 us per second
   pulselength /= 50;   // 50 Hz
   pulselength /= 4096;  // 12 bits of resolution
   pulse *= 1000;
   pulse /= pulselength;
   Serial.print("pulse:");
   Serial.println(pulse);
   pwm.setPWM(n, 0, pulse);
 }

 void demoAction(){
 slowSetServoPulse(0, 90-servo_0_current_angle, servo_0_current_angle,servo_0_min_angle,servo_0_max_angle);
 slowSetServoPulse(1, 75-servo_1_current_angle, servo_1_current_angle,servo_1_min_angle,servo_1_max_angle);
 slowSetServoPulse(5, 50-servo_5_current_angle, servo_5_current_angle,servo_5_min_angle,servo_5_max_angle);
 slowSetServoPulse(7, 100-servo_7_current_angle, servo_7_current_angle,servo_7_min_angle,servo_7_max_angle);
 delay(1000);
 slowSetServoPulse(0, 0-servo_0_current_angle, servo_0_current_angle,servo_0_min_angle,servo_0_max_angle);
 slowSetServoPulse(1, 135-servo_1_current_angle, servo_1_current_angle,servo_1_min_angle,servo_1_max_angle);
 slowSetServoPulse(5, 100-servo_5_current_angle, servo_5_current_angle,servo_5_min_angle,servo_5_max_angle);
 slowSetServoPulse(7, 150-servo_7_current_angle, servo_7_current_angle,servo_7_min_angle,servo_7_max_angle);
 delay(2000);
 slowSetServoPulse(0, 90-servo_0_current_angle, servo_0_current_angle,servo_0_min_angle,servo_0_max_angle);
 slowSetServoPulse(1, 75-servo_1_current_angle, servo_1_current_angle,servo_1_min_angle,servo_1_max_angle);
 slowSetServoPulse(5, 50-servo_5_current_angle, servo_5_current_angle,servo_5_min_angle,servo_5_max_angle);
 slowSetServoPulse(7, 100-servo_7_current_angle, servo_7_current_angle,servo_7_min_angle,servo_7_max_angle);
 delay(1000);
 slowSetServoPulse(0, 0-servo_0_current_angle, servo_0_current_angle,servo_0_min_angle,servo_0_max_angle);
 slowSetServoPulse(1, 135-servo_1_current_angle, servo_1_current_angle,servo_1_min_angle,servo_1_max_angle);
 slowSetServoPulse(5, 100-servo_5_current_angle, servo_5_current_angle,servo_5_min_angle,servo_5_max_angle);
 slowSetServoPulse(7, 150-servo_7_current_angle, servo_7_current_angle,servo_7_min_angle,servo_7_max_angle);
 delay(2000);
}

// 处理动作
void handleMotorAction(char commend){
  if(commend == '\n' || commend == '\r'){
    return;
  }
  Serial.print("commend:");
  Serial.print(commend);
  Serial.println();

  switch (commend){
    case 'w':
      // 如果指令为前进
      handleForward();
        break;
    case 's':
      // 如果指令为后退
      handleFallback();
        break;
    case 'a':
      // 如果指令为左转
      handleTurnLeft();
        break;
    case 'd':
      // 如果指令为右转
      handleTurnRight();
        break;
    case 'q':
      // 如果指令为停止
      handleStop();
        break;
    case 'k':
      // 抬脖子
      slowSetServoPulse(2, -25, servo_2_current_angle,servo_2_min_angle,servo_2_max_angle);
        break; 
    case 'i': 
      // 低脖子
      slowSetServoPulse(2, 25, servo_2_current_angle,servo_2_min_angle,servo_2_max_angle);
        break;
    case 'j':
      // 左转头
      slowSetServoPulse(1, -30, servo_1_current_angle,servo_1_min_angle,servo_1_max_angle);
        break;
    case 'l': 
      // 右转头
      slowSetServoPulse(1, 30, servo_1_current_angle,servo_1_min_angle,servo_1_max_angle);
        break;
    case 'r': 
      // 抬头
      slowSetServoPulse(0, 40, servo_0_current_angle,servo_0_min_angle,servo_0_max_angle);
        break;
    case 'f': 
      // 低头
     slowSetServoPulse(0, -40, servo_0_current_angle,servo_0_min_angle,servo_0_max_angle);
        break;
    case 't':
      // 左手上
      slowSetServoPulse(5, -20, servo_5_current_angle,servo_5_min_angle,servo_5_max_angle);
        break;
    case 'g':
      // 左手下
      slowSetServoPulse(5, 20, servo_5_current_angle,servo_5_min_angle,servo_5_max_angle);
        break;
    case 'y':
      // 右手上
      slowSetServoPulse(7, 20, servo_7_current_angle,servo_7_min_angle,servo_7_max_angle);
        break;
    case 'h':
      // 右手下
      slowSetServoPulse(7, -20, servo_7_current_angle,servo_7_min_angle,servo_7_max_angle);
        break;
    case 'z':
      // 播放音乐4
      //myDFPlayer.playMp3Folder(4);
        break;
    case '1':
      // 播放音乐6
      //myDFPlayer.playMp3Folder(6);
        break;
    case '2':
      // 动作编组1
      demoAction();
        break;
    case '3':
      // 复位
      resetServo();
        break;
    default:
        // 如果不匹配任何指令 不进行操作
        break;
  }
}

void initMotor(){
  // 定义6、7、8、9引脚模式
  pinMode(6, OUTPUT);
  pinMode(7, OUTPUT);
  pinMode(8, OUTPUT);
  pinMode(9, OUTPUT);
  // 初始化时履带为停止状态 所以给6、7、8、9引脚输出高电平
  digitalWrite(6, HIGH);
  digitalWrite(7, HIGH);
  digitalWrite(8, HIGH);
  digitalWrite(9, HIGH);
}

// 关节复位
void resetServo(){
  slowSetServoPulse(0, 0-servo_0_current_angle, servo_0_current_angle,servo_0_min_angle,servo_0_max_angle);
  slowSetServoPulse(1, 105-servo_1_current_angle, servo_1_current_angle,servo_1_min_angle,servo_1_max_angle);
  slowSetServoPulse(2, 0-servo_2_current_angle, servo_2_current_angle,servo_2_min_angle,servo_2_max_angle);
  slowSetServoPulse(5, 100-servo_5_current_angle, servo_5_current_angle,servo_5_min_angle,servo_5_max_angle);
  slowSetServoPulse(7, 100-servo_7_current_angle, servo_7_current_angle,servo_7_min_angle,servo_7_max_angle);
}
void servoSetUp(){
  servo_9g_write(0,0);//控制头俯仰
  servo_9g_write(1,105);//控制头旋转
  servo_9g_write(2,0);//控制脖子俯仰
  // servo_9g_write(3,0);//控制右眼
  // servo_9g_write(4,0);//控制左眼
  servo_9g_write(5,100);//控制左手
  servo_9g_write(7,100);//控制右手
}
void initServo(){
  // Communicate with servo shield (Analog servos run at ~60Hz)
	pwm.begin();
	pwm.setPWMFreq(50);
  servoSetUp();
}

void setup() {
  // put your setup code here, to run once:
  // 初始化串口
  Serial.begin(115200);
  initMotor();
  initServo();
  servoSetUp();
}

void loop() {
  // put your main code here, to run repeatedly:
  // 如果串口接收到了控制指令则读取指令并做出相应的动作
  if (Serial.available() > 0) {
    char commend =  Serial.read();
     handleMotorAction(commend);
  }
}



```



#### 四、扩展模块（选配）

> 没有使用的模块请自行删除对应代码



完整版（包含所有模块）ESP32 C3代码：链接：https://pan.baidu.com/s/1AcRYgk3Fzj8J-DgBbBVwbA?pwd=453w

完整版（包含所有模块）Arduino UNO代码：链接：https://pan.baidu.com/s/1WTP7OwrzfyabWFWJCDkhqA?pwd=w5f2

使用语音控制模块前请先安装天问Block软件

天问官网 ：http://www.twen51.com/new/twen51/index.php

ASR PRO语音模块代码：https://pan.baidu.com/s/1F9mbbMGzFKIO1RYE3TRoSA?pwd=ucwb

##### 4.1 ASR PRO语音控制模块  

```c++
             
ASR PRO语音模块                          ESP32 C3

  GND     --------------------------      GND

  5V      --------------------------      5V

  PB5     --------------------------      IO06

  PB6     --------------------------      IO07


```



##### 4.2 DHT11温湿度模块 

需要安装 DHT sensor library  库

```
DHT11温湿度模块                          ESP32 C3

   +       --------------------------      3.3V

  OUT      --------------------------      IO04

  GND      --------------------------      GND

```



##### 4.3 RC522 RFID识别模块

Arduino_NFC代码：链接：https://pan.baidu.com/s/1FvI96uUmDy-ZOgpB4U8FaA?pwd=wzwa

需要安装 MFRC522 库

```
RC522 RFID模块                        Arduino UNO 开发板

  SDA     --------------------------      10

  SCA     --------------------------      13

  MOSI    --------------------------      11

  MISO    --------------------------      12
  
  GND     --------------------------      GND

  3.3V    --------------------------      3.3V


```

##### 4.4 Mini Mp3Player Mp3播放模块

需要安装 DFRobotDFPlayerMini 库

官方文档：https://wiki.dfrobot.com/DFPlayer_Mini_SKU_DFR0299

把音乐文件夹导入SD卡中

音乐链接：https://pan.baidu.com/s/15qRZ5JTXNoIE5nzJVDV0Vw?pwd=dmkf

Arduino_MP3代码：链接：https://pan.baidu.com/s/1Si77ZYmtONfURM49FJ6aQw?pwd=dpfk

![](https://ipai-saas.obs.cn-east-3.myhuaweicloud.com:443/4914b2a4-c0db-440c-ace7-bd53e88707b7.png)

```
                           Mini Mp3Player模块                    Arduino UNO 开发板

                                  VCC      --------------------------      5V

                                  RX       --------------------------      2

                                  TX       --------------------------      3

                                  DAC_R    --------------------------    不接线

                                  DAC_1    --------------------------    不接线  
                                  
        喇叭1  ----------------   SPK_1    

                                  GND      --------------------------      GND

        喇叭2  ----------------   SPK_2   


```



##### 4.5 红色激光 

```
 红色激光头                               ESP32 C3

   +       --------------------------      IO10

  GND      --------------------------      GND

```



#### 问题Q&A

为了更快的排查问题，解决问题，按一下图中的定义来约定一二三段通信。

首先看控制链路的原理图：

![](https://ipai-saas.obs.cn-east-3.myhuaweicloud.com:443/81ce8665-134e-4528-b9ca-a2116960c94c.png)



整体连接图：

![](https://ipai-saas.obs.cn-east-3.myhuaweicloud.com:443/84074bb4-8cf3-4a6d-a3c8-c5e5ad229085.png)



##### 控制不了？没反应？

（1）首先确保H5控制端和ESP32都连接上了mqtt服务器。

（2）一段通信：H5控制端连接mqtt服务器后，点击发送指令，如果页面自己能收到指令并打印日志，则证明一段通信没问题。

（3）二段通信：H5发送指令后，ESP32串口有相应日志输出，并且板载Led交替闪烁。

（4）三段通信：H5发送指令后，UNO串口有相应日志输出。

##### ESP32连接不上MQTT服务器？

1.mqtt的topic是否已替换

2.WIFI和密码是否已替换

#### 如何提问？

先确保以上链路的通信没问题之后，在检查模块接线。

1.开发板代码版本、问题描述

2.ESP32串口日志截图

3.UNO串口日志截图



第一次写教程录视频，难免有些卡壳和不足，如有错误之处还请指正，请见谅。

如果感觉对你有帮助的话，请帮忙点个一键三连，谢谢。

#### 未完待续。视频教程还在陆续制作中，文档会同步更新，敬请期待！

