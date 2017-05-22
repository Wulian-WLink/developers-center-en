# Basic interface
## Register callback API
```c++
void RegisterCallBack(SDKOnline pfungatewayonline,
        SDKOffline pfungatewayoffline,
        ZigbeeSetupData pfunzigbeedata,
        DeviceOnline pfundeviceonline,
        DeviceOffline pfundeviceoffline,
        DeviceLeft pfundeviceleft,
        DeviceData pfundevicedata,
        DeviceSetupData pfundevicesetupdata,
        DeviceAlarmData pfundevicealarmdata,
        LogMessage pfunlogmessage);
```
Register callback API
### Callback description

#### sdk online callback
```java
void (*SDKOnline)();
```
When SDK connects normally,, this callback is invoked.
#### sdk offline callback
```java
void (*SDKOffline)()
```
When SDK is offline or cannot work nromally, this callback is invoke.

#### zigbee module parameter callback
```java
void (*ZigbeeSetupData)(const char *chData);
```  
zigbee module parameter return
Parameter  | Description
         --|--
chData  |  callback data: when chData string length is 2, it means zigbee signal channel number; when chData sting length is 4, it means zigbee join network(01XX)quit network(0100)

#### Device online callback
```java
void (*DeviceOnline)(const char *chDeviceID, const char *chDeviceType, const char *chData);
```  
Device online callback  

Parameter | Description
--|--
chDeviceID  | device id
chDeviceType  |  device type
chData  |  device online data (refer to data protocol)

#### Device offline callback
```java
void (*DeviceOffline)(const char *chDeviceID);
```  
Device offline callback (device cannot return normal data and determine by gateway as offline)

Parameter | Description
--|--
chDeviceID  | device id

#### Device offline callback (device quit network by iteself)
```java
void (*DeviceLeft)(const char *chDeviceID);
```  
Device offline callback (device quit network by iteself)

Parameter | Description
--|--
chDeviceID  | device id

#### Device data return callback
```java
void (*DeviceData)(const char *chDeviceID, const char *chData);
```  
Device data return callback  

Parameter | Description
--|--
chDeviceID  | deivice id
chData  |  device data (refer to data protocol)

#### Device signal return callback
```java
void (*DeviceSetupData)(const char *chDeviceID, const char *chData);
```  
Device signal return callback 

Parameter | Description
--|--
chDeviceID  | deivice id
chData  |  device signal level value

#### Device low battery voltage callback
```java
void (*DeviceAlarmData)(const char *chDeviceID, const char *chData);
```  
Device low battery voltage or destruction callback

Parameter | Description
--|--
chDeviceID  | deivice id
chData  |  "1" means low battery voltage

#### log callback```java
void (*LogMessage)(const char *chMsg)
```  
sdk log callback  

Parameter | Description
--|--
chMsg  | log message

## Unregister callback API
```c++
void UnRegisterCallBack();
```  
If need to refresh callback, need to unregister all callback and register again.

## Initialize SDK
```c++
void WulianSDK_Start();
```  
Start SDK invoke (non-choke)

## Build Zigbee network
```c++
int BuildNetwork();
```  
Invoke when gateway start to use for the first time. No need to invoke after power cycle.

## Add zigbee device
```c++
int EnableJoinGateway();
```  
Invoke when need to add zigbee device. After invoked, gateway will enter allow-join-network mode for 4 minutes.

## Forbid adding zigbee device
```c++
int DisableJoinGateway();
```  
If gateway is under all-join-network mode, invoking this API will quit this mode.

## Sync all device status 
```c++
int SyncAllDevOnlineStatus();
```  
If return -1, it means SDK is not ready. If return 0, it means SDK is ready and device data will be returned via callback.

## Sync single device status
```c++
int SyncOneDevOnlineStatus(const char *chDeviceID);
```  
Parameter | Description
--|--
chDeviceID  | deivice id
If return -1, it means SDK is not ready. If return 0, it means SDK is ready and device data will be returned via callback.

## Control device
```c++
int ControlDevice(const char *chDeviceID, const char *chCmd);
```  
Control hardware device. 

Parameter | Description
--|--
chDeviceID  | deivice id
chCmd  |  Control command (refer to device protocol)

return value  
If return -1, it means SDK is not ready. If return 0, it means success.


## Discover device
```c++
int FindDevice(const char *chDeviceID, int itimes);
```  
Discover device by forcing device LED flashing.

Parameter | Description
--|--
chDeviceID  | deivice id

return value  
If return -1, it means SDK is not ready. If return 0, it means success.

## Acquire device signal
```c++
int GetDeviceSignal(const char *chDeviceID);
```  
Acquire device signal value and return via callback. 

Parameter | Description
--|--
chDeviceID  | deivice id

return value  
If return -1, it means SDK is not ready. If return 0, it means success.

## Delete device
```c++
int DeleteDevice(const char *chDeviceID);
```  
Delete device from gateway

Parameter | Description
--|--
chDeviceID  | deivice id

return value  
If return -1, it means SDK is not ready. If return 0, it means success.

## Reset zigbee network
```c++
int ResetZigbee();
```
Reset zigbee. Note invoking this API will cause all devices offline.


## Acquire SDK version
```c++
const char *GetSdkVersion();
```  
Acquiring SDK version
