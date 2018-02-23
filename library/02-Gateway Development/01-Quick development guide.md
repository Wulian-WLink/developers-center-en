# Quick development guide
## Development introduction
Wulian provide gateway SDK for accessing Wulian device. 3rd party cloud developer can develop their own cloud process base on gateway SDK and run that process on Wulian gateway, so that to implement the target of access Wulian device from their cloud directly to Wulian gateway. 
<br>
Applicable developer:
- Developer/Company that has their own cloud service.
- Want to use Wulian Wlink platform provided hardware.

## Gateway development platform description
Gateway is the controll center of Wulian devices, with the basic management and control functions for access, control, scenario, linkage, upgrade of Wulian devices.
<br>
The new generation (v6) gateway platform consists of device process, SmartHome_v6 process, SmartHome_v5 process and 3rd party cloud process. Each process is communicating based on MQTT protocol. 3rd party cloud developer has no need to understand internal implementation detail, only need to develop 3rd cloud process based on SDK. SDK will be provided as dynamic library and contains mips, arm and x86 version.

  ![Architecture fram work](WLink/images/gwFrame.png "架构框图")   
  To connect 3rd party cloud process to Wulian gateway, need to apply for development gateway to Wulian. Currently the available gateway for development is vertical gateway. Gateway introduction and manual can be checked via below URL.

[Vertical gateway introduction](http://www.wuliangroup.com/cn/index.php/product/info/145)  
[Vertical gateway manual](http://www.wuliangroup.com/cn/index.php/service/download/1539)

## Preparation
- ubuntu x86 host machine
- vertical gateway (development version)
- mips-openwrt-linux, arm-openwrt-linux toolchain
- Labrary: boost, curl, sqlite, jsoncpp, openssl, mosquitto etc.

## Development process
1. Connect gateway and development host machine with same router. Gateway WAN port by defalt does not enable telnet, need to connect router LAN cable to LAN port of the gateway and make sure the development host machine is configured in subnet 192.168.188.x.
2. Use tools like putty and telnet access 192.168.188.1 on development host machine. By default user name is "root" and password is "wulian".
3. Download [Gateway SDK](https://github.com/Wulian-WLink/GatewaySDK_v6.git).
4. Refer to [Device data protocol](https://wulian-wlink.github.io/developers-center-en/?file=05-Device%20Protocol/01-Overview) and develop 3rd party cloud process firmware.
5. After development is completed, download the firmware (the 3rd party cloud process) onto the gateway. The gateway supports tft and wget method for file transfer.
6. Run the firmware (the 3rd party cloud process).

## ubuntu debug
To improve develop efficiency and realize breakpoint debugging, user can run 3rd party cloud process on ubuntu, while device process running on the gateway. In this case ubuntu and gateway need to be within the same LAN. So under this condition, user can use QT or VS to do breakpoint debugging for 3rd party cloud process.  
Below config needs to be done:
1. In case mqtt server running on the gateway (default).  
Under current 3rd party cloud process directory create x86Debug_TP with below content:  
```
{
  "gwinfo":{
    "option":{
        "gwid":"50294DFF1289",
        "gwip":"172.18.2.159"
        }
    }
}
//gwid is the current debugging gateway ID. gwIp is gateway's WAN IP.
```
2. In case mqtt server running on bubuntu. (if ubuntu cannot ping gateway successfully but gateway can ping ubuntu successfully, please use this mode)  
a. stop mqtt service on the gateway and start mqtt service on ubuntu.  
b. do as above step 1, but gwip is ubuntu's IP.  
c. under gateway device process directory create x86Debug_UP with below content:
```
{
   "gwinfo" : {
      "option" : {
         "gwip" : "172.18.2.159"
      }
   }
}
//gwip is ubuntu's IP
```
