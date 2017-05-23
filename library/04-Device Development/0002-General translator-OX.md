 ## 简介
- 该协议适用于所有串口型翻译器、Zigbee透传模块。它能将传统485/232设备通过zigbee 网络接入互联网，需配合网关使用。  

- 该设备支持两种工作模式：透传模式、配置模式。设备同时支持两种模式，如发送数据符合配置指令的规
则，则进入配置模式，执行相关配置行为；反之，则透传。  

- 配置模式支持远程配置、本地配置。app端远程配置时，需将epType设置为15。  

- 它支持更改设备类型，理论上可以配置成物联任何一款设备。设备类型更改后，按照该设备类型的数据协议通讯，即可自动适配物联网关、物联app、物联云，获得各类功能服务。设备数据协议文档另附文件。

  **默认配置**
  1. 设备类型：“OX”
  1. 波特率：9600
  1. 停止位：1
  1. 数据位：8
  1. 奇偶校验位：无

## 通信流程
该设备支持text、hex两种数据格式通信，通过epType字段区分，当epType为14时，数据格式为text；当epType为15时，数据格式为hex。  

    注：若使用网关SDK开发，则epType默认为14，且无法修改。  

**例：**  
1.app->device  

    epType为14时：  
    app发送：“123”  <->  0x31,0x32,0x33
    app发送：0xFF,0x00,0xFE
    device接收：“123”  <->  0x31,0x32,0x33  


    epType为15时：
    app发送：“313233”  <->  0x33,0x31,0x33,0x32,0x33,0x33
    device接收：“123”  <->  0x31,0x32,0x33
2.devcie->app  

    epType为14或15时，
    设备发送：“123”  <->  0x31,0x32,0x33
    app接收：“313233”

## 协议格式  
头部  |指令码   | 参数  | 校验
--|---|---|--
2字节  |2字节   | n字节  | 1字节
*除特殊说明，交互数据默认为16进制。*  

- 头部  
0xFE，0xDF  
- 指令码  
指令码分为两个字节。第一个字节为指令类型码，第二个字节为具体指令码。指令类型码分为控制类型、获取状态类型、异常类型。  
响应时的指令类型码为下发的指令类型码与0x80异或。如0x01->0x81。  
- 参数：指每个指令对应的具体参数值。当指令类型为获取状态时，参数值为空。  
- 校验：采用和校验。校验=头部(H) +头部(L) + 指令码(H) + 指令码(L) + 参数。  

## 指令码及参数值
### 指令类型码
指令  | 描述
--|--
0x01  | 控制类型
0x02  | 获取状态类型
0x03  | 保留
0x04  | 异常类型
### 具体指令码——控制类型
指令  | 描述 | 参数值
--|---|--
0x00  | 集中设置  |  
0x01  | 恢复出厂设置  |  0x01
0x02  | 设置波特率  |  0x01:2400、0x02:4800、0x03:9600、0x04:14400、 0x05:19200、0x06:38400、0x07:115200
0x03  | 设置数据位  |  
0x04  | 设置停止位  |  
0x05  | 设置校验位  |  
0x06  | 更改设备类型  | 详见<设备数据协议>

 当具体指令码为“集中控制”时，负载数据由以下格式构成：  