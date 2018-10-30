# Debug
----------
## 基本调试控制
	1. 在进行断点设置时应注意避免循环体内断点
	2. Debug时注意前导0相对应的情况
	3. Debug 当发生有LUW时，后台数据库底层会进行Update，需要进行外部断点的设置才能够跟踪debug
## 远程调试
	1. 远程调用SAP功能时，需要启用远程debug功能，Utilities->settings->Debugging->External Debugging(Users)设置外部调用账号
## 断言

## RAL

## 后台作业调试

## 更新远程调试
