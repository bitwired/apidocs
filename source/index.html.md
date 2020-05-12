---
title: BitFlex API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - java
  - python
  - javascript

toc_footers:
  - <a href='#'>English Version</a>
  - <a href='#'>BitFlex Exchange</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

# includes:
#   - errors

search: true
---

# 介绍

欢迎使用 BitFlex APIs 和 Websocket官方文档！

如果您在使用API的过程中遇到问题需要帮助，请联系我们！

# 更新日志

**2020-05-11**

**REST API**

* 增加API文档

# 基本信息

## API基本信息

* 本篇列出接口的baseurl: http://47.92.168.163:22280/v1
* 部分接口为需要登录接口，相关设置请见用户功能模块
* 所有接口的响应都是JSON格式

## 登录接口配置

* 本篇凡是以user开头的接口名称都为需要登录接口，非user开头为不需要登录
* 密码相关部分传输，如用户密码、资金密码传输过程中前端均用md5(密码+指定字符串)加密一下，测试环境下指定字符串先用DZO%0JW$JTJk，比如我的密码输入是123456A那么前端传输给后端的字符串结果为a5b8cb029d6828b1f4bb40c97a2a7b6a，计算过程为md5(123456ADZO%0JW$JTJk)
* header中添加lang参数代表语言如中文用zh-CN英文用en-US，另外app端header里面统一添加X-CLIENT，格式如：

```json
{
  "device_id": "015d7015-63dd-4ddb-b88b-528a269241f4",
  "app_version": "1.2.1",
  "os_version": "4.3.1",
  "client_type": "ANDRIOD"
}
```

字段 | 说明
------------ | ------------ 
device_id | 设备ID
app_version | 版本号
os_version | 系统版本号
client_type | ANDROID或IOS

# 用户功能

## 发送邮箱验证码（无登录状态下）

**HTTP Request**

`POST /message/public-email`

**请求参数**

参数 | 必选 | 类型 | 说明
------------ | ------------ | ------------ | ------------
email | 是 | String | 邮箱
type  | 是 | String | 只有REGISTER和RESET_LOGIN_PASSWD两个值，分别代表注册和忘记登录密码
session_id | 是  | String | 人机验证session_id app端只传此参数
sig   | 否  | String | 人机验证签名 web端必填
token | 否  | String | 人机验证token web端必填
scene | 否  | String | 人机验证使用场景 web端必填ic_message

```json
{
  "code": 0,
  "message": "ok",
  "data": true
}
```
**响应结果**

## 发送邮箱验证码（登录状态下）

**HTTP Request**

`POST /user/message/email`

**请求参数**

参数 | 必选 | 类型 | 说明
------------ | ------------ | ------------ | ------------ 
email | 否 | String | 绑定邮箱时必填
type  | 是 | String | 取值有BIND_EMAIL 绑定邮箱<br/>BIND_PHONE 绑定手机<br/>BIND_GOOGLE 绑定谷歌<br/>CHANGE_LOGIN_PASSWD 修改登录密码<br/> SET_ANTI_PHISHING_CODE 设置防钓鱼码<br/> WITHDRAW 提币<br/> ADD_WITHDRAW_ADDRESS 添加提币地址<br/>

```json
{
  "code": 0,
  "message": "ok",
  "data": true
}
```
**响应结果**

## 发送短信验证码(无登录状态下)

**HTTP Request**

`POST /message/public-sms`

**请求参数**

参数 | 必选 | 类型 | 说明
------------ | ------------ | ------------ | ------------ 
phone | 否 | String | 手机号 当type为LOGIN时无需填此参数
type  | 是 | String | 只有REGISTER LOGIN RESET_LOGIN_PASSWD三个值，分别代表注册 登录 忘记登录密码
area_code | 否 | String | 手机号区号 注册时此值必填
sms_token | 否 | String | 发送登录验证码时必填 取值来自登录接口中返回的sms_token
session_id | 是 | String | 人机验证session_id app端只传此参数
sig | 否 | String | 人机验证签名 web端必填
token | 否 | String | 人机验证token web端必填
scene | 否 | String | 人机验证使用场景 web端需传ic_message
<aside class="warning">
注意：短信验证码尚未接通运营商，调完发送之后不会真正收到短信，但后端统一做了临时处理，前端统一填654321即可
</aside>

```json
{
  "code": 0,
  "message": "ok",
  "data": true
}
```
**响应结果**