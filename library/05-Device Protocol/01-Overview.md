## 概念解释与约定



**上行、下行：**

指数据流向，从pc等控制端发送到终端设备接收为下行，反之为上行。例如设置幻彩灯开启幻彩模式，控制端发送下行命令”91”到终端，设备返回上行数据”0901”。

**ep：**

EndPoint，在ZigBee中将各网关、终端设备都统称为ep，在物联协议中起一种区分功能、端口属性。

在单个设备中通常默认为14；复合设备中则取消自身整体的ep，转而标识每个子端口，从14开始，依次递增。若复合设备的ep=0，则表示该设备接收一条数据时，所属各个子端口平分该条数据。例如设三路开关ep=0，当其收到一条数据”101”，则其三个子端口（子开关）分别收到”1”、”0”、”1”。

**epType：**

端口的类型，既相当于设备类型，用于区分端口属于何种类型。在复合设备中包含该复合设备的主type和所属子设备的epType。例如红外感应器--移动侦测/光强，主type=40、端口1
epType=02、端口2
epType=19，标识了该设备类型为DeviceType=40的复合设备，包含epType=02的红外感应器和epType=19的光强检测器两个子端口（子设备）。

**epData：**

端口的数据，设备的状态数据和控制数据主要都以epData形式传输。其协议分别为返回数据协议、发送数据协议。

**epStatus：**

端口的状态，在安防设备中0表示撤防，1表示布防，在其他设备中无意义。

## 设备介绍


物联设备由设备属性和设备行为两部分构成。

设备属性包括网关id、设备id、设备类型等基本信息：

**{**

>   **网关id（gwid）：**”22AAC503004B1200”——设备所接入的网关id

>   **设备id（chDeviceID）**：” FA392405004B1200”——设备自身id

>   **设备类型（chDeviceType）**：” 90”——90表示幻彩灯，区分设备类型

**}**

设备行为包括设备上线、设备下线、上报数据、上报配置信息等行为：

**{**

>   **设备上线：**在设备上线时回调，同时回调网关id、设备id、设备类型、设备数据等信息

>   **设备下线：**在设备下线时回调，同时回调网关id、设备id等信息

>   **上报数据：**在控制端发送控制命令、设备状态变更等情况下回调（数据意义参照后续章节各详细设备的协议说明），同时回调网关id、设备id、上报数据等信息

>   **上报配置信息：**在主动向设备（被动）获取下行信号值（数据”02”开头），设备主动上报上行信号值（数据”03”开头）时回调，同时回调网关id、设备id、设备类型、设备数据等信息

**}**

在使用过程中，设备出现各种行为或功能时，都会与控制端之间产生数据交互。数据交互在上行和下行的协议各有不同，上行数据应遵照返回数据协议解析，下行控制命令应遵照发送数据协议发送命令。

## 具体设备协议


物联智能家居平台的设备主要分为以下七大类，安防类、照明类、电器类、环境监测类、健康类、综合类、家电类。

### 安防类


安防类设备，主要包括声光报警器、红外感应器、门窗磁感应器、紧急按钮、电子栅栏、漏水感应器、烟雾感应器、可燃气感应器、可燃气阀门、自动门、道闸、红外感应器--移动侦测/光强、烟雾感应器--SR、普通门锁、纽扣门锁、指纹门锁、密码门锁、玻璃破碎感应器、门铃按钮、门铃声光器、IPAD防盗器、多功能人体运动感应器等。

### 照明类


照明类设备，主要包括调光开关、二路调光开关、一路照明开关、二路照明开关、三路照明开关、四路照明开关、LED炫彩灯、无线智能调色温调光LED灯、调光LED灯、调色调光模组、炫彩灯（梦想之花虚拟设备）等。

### 电器类


电器类设备，主要包括移动插座、红外转发器、万能红外转发器、机械手、水阀、墙面插座、二路墙面插座、窗帘控制器、百叶窗、移动计量插座--SR、温控器、一路可调窗帘、二路可调窗帘、电台（梦想之花和炫彩灯虚拟设备）、音乐播放器（吸顶灯虚拟设备）、电压力煲、无线音乐盒、大金中央空调、幕帘探测器等。

### 环境监测类


环境监测类设备，主、要包括温湿度检测器（温度、湿度）、光强检测器、空气质量、粉尘检测器、车位检测器、人流量检测器、空气质量--WLZJ、噪音检测器（梦想之花虚拟设备）、粉尘检测器（梦想之花虚拟设备）、VOC检测器（梦想之花虚拟设备）等。

### 健康类


健康类设备，主要包括电子秤、血压计等。

### 综合类


综合类设备，主要包括一键调光、四路输入转换器、二路输出转换器、一路有线无线翻译器、Mini网关（Mini网关虚拟设备）、车辆检测器（模拟，有GPS和车载检测组成，type事先定义好）、一键开关、二键开关

### 家电类

该类设备主要是其它厂家对接的设备，如海信空调、海信洗衣机、美的中央空调等。