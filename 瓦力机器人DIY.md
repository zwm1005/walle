### 瓦力机器人DIY



#### 概述

简介：

​		相信童年看过《机器人瓦力》电影的小伙伴一定都想拥有一台属于自己的瓦力，本教程就DIY瓦力所需的物料和控制程序开始，带着大家一起DIY一个瓦力机器人。我想没有男孩子能抵抗的住这集成声、光、电、动的小机器人玩具吧，寒假带着娃做一个，把隔壁小孩馋哭  哈哈。

#### 物料清单(BOM)

![](http://cdn.werfamily.fun/images/20240114_5411043.png)

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

3. Linux + Nginx + Docker + EMQTT  后台服务器

   

#### 一、Web控制端

##### 1.1 Arduino IDE开发环境搭建

Arduino官网  https://www.arduino.cc/

Arduino中文社区 https://arduino.me/

1. 安装Arduino软件
   
   * 中文社区提供的安装教程 https://arduino.me/s/arduino-getting-started?aid=125
   
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

  5.安装USB串口驱动，这个要看你所使用的开发板上下载芯片的型号，常见的是CH34X、CP21XX芯片等，文件里   	 分享了这两种，如果没有的话请搜索你开发板的串口芯片型号，下载对应驱动后安装。

##### 1.2 使用Blinker App 作为控制端

* 新建Blinker设备、复制Blinker设备秘钥替换到代码中，替换

* 瓦力界面配置代码：

```
{¨version¨¨2.0.0¨¨config¨{¨headerColor¨¨transparent¨¨headerStyle¨¨dark¨¨background¨{¨img¨¨assets/img/headerbg.jpg¨¨isFull¨«}}¨dashboard¨|{¨type¨¨tex¨¨t0¨¨blinker入门示例¨¨t1¨¨文本2¨¨bg¨Ë¨ico¨´´¨cols¨Í¨rows¨Ê¨key¨¨tex-272¨´x´É´y´É¨speech¨|÷¨lstyle¨Ê¨clr¨¨#FFF¨}{ßC¨btn¨ßJ¨fas fa-arrow-alt-down¨¨mode¨ÉßE¨后退¨ßGßHßIÉßKËßLËßM¨btn-back¨´x´Ì´y´¤FßPÉßQ¨#076EEF¨}{ßCßSßJ¨fas fa-arrow-alt-up¨ßUÉßE¨前进¨ßGßHßIÉßKËßLËßM¨btn-go¨´x´Ì´y´¤BßQßXßPÉ}{ßCßSßJ¨fal fa-power-off¨ßUÉßE¨急停¨ßGßHßIÉßKËßLËßM¨btn-stop¨´x´Ì´y´¤DßQ¨#EA0909¨ßPÉ}{ßCßSßJ¨fas fa-arrow-alt-right¨ßUÉßE¨向右¨ßGßHßIÉßKËßLËßM¨btn-right¨´x´Î´y´¤DßPÉßQßX}{ßCßSßJ¨fas fa-arrow-alt-left¨ßUÉßE¨向左¨ßGßHßIÉßKËßLËßM¨btn-left¨´x´Ê´y´¤DßQßXßPÉ}{ßCßSßJ¨fad fa-arrow-alt-circle-up¨ßUÉßE¨抬脖子¨ßGßHßIÉßKËßLËßM¨btn-nick-up¨´x´Ì´y´ÏßQ¨#FBA613¨}{ßCßSßJ¨fad fa-arrow-alt-circle-down¨ßUÉßE¨低脖子¨ßGßHßIÉßKËßLËßM¨btn-nick-down¨´x´Ì´y´ÑßQßoßPÉ}{ßCßSßJ¨fad fa-arrow-alt-up¨ßUÉßE¨左手上¨ßGßHßIÉßKËßLËßM¨btn-left-hand-up¨´x´É´y´ÒßQ¨#00A90C¨ßPÉ}{ßCßSßJßsßUÉßE¨右手上¨ßGßHßIÉßKËßLËßM¨btn-right-hand-up¨´x´Ï´y´ÒßQßv}{ßCßSßJ¨fad fa-arrow-alt-down¨ßUÉßE¨左手下¨ßGßHßIÉßKËßLËßM¨btn-left-hand-down¨´x´É´y´¤BßPÉßQßv}{ßCßSßJßyßUÉßE¨右手下¨ßGßHßIÉßKËßLËßM¨btn-right-hand-down¨´x´Ï´y´¤BßQßv}{ßCßSßJ¨fad fa-arrow-alt-circle-left¨ßUÉßE¨左转头¨ßGßHßIÉßKËßLËßM¨btn-head-left¨´x´Ê´y´ÐßQßo}{ßCßSßJ¨fad fa-arrow-alt-circle-right¨ßUÉßE¨右转头¨ßGßHßIÉßKËßLËßM¨btn-head-right¨´x´Î´y´ÐßPÉßQßo}{ßCßSßJßyßUÉßE´低头´ßGßHßIÉßKËßLËßM¨btn-head-down¨´x´Ì´y´ÍßQßoßPÉ}{ßCßSßJßsßUÉßE´抬头´ßGßHßIÉßKËßLËßM¨btn-head-up¨´x´Ì´y´ËßQßo}{ßCßSßJ¨fad fa-user-robot¨ßUÉßE¨动作1¨ßGßHßIÉßKËßLËßM¨btn-act1¨´x´É´y´ÎßQßXßPÉ}{ßCßSßJ¨fad fa-redo-alt¨ßUÉßE¨动作复位¨ßGßHßIÉßKËßLËßM¨btn-reset¨´x´Ï´y´ÎßPÉßQßX}{ßCßSßJ¨fad fa-lightbulb-on¨ßUÉßE´激光´ßGßHßIÉßKËßLËßM¨btn-led¨´x´É´y´ÌßPÉßQße}{ßCßSßJ¨fad fa-thermometer-three-quarters¨ßUÉßE´温度´ßGßHßIÉßKËßLËßM¨btn-tem¨´x´Ï´y´ÌßPÉßQßv}{ßC¨deb¨ßUÉßIÉßKÑßLÌßM¨debug¨´x´É´y´¤H}÷¨actions¨|¦¨cmd¨¦¨switch¨‡¨text¨‡´on´¨打开?name¨¨off¨¨关闭?name¨—÷¨triggers¨|{¨source¨ß1P¨source_zh¨¨开关状态¨¨state¨|´on´ß1S÷¨state_zh¨|´打开´´关闭´÷}÷´rt´|÷}

```

##### 1.3 烧写控制代码到ESP32 C3开发板

