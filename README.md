# PTUNS API DOC


## 会员模块

### 1、登录接口

- RequestUrl：{{BaseUrl}}/wechat/quickLogin
- RequestMethod:Post
- Content-Type:application/json
- Request Parameters：

| 参数名 | 必填 | 数据类型 | 说明 |
| :-----| ----: | :----: | :----: |
| mobile | 是 | String | 手机号码 |
| verifyCode | 是 | String | 短信验证码 |
| code | 是 | String | 短信验证码 |
| invitationCode | 否 | String | 短信验证码 |
| recommendMobile | 否 | String | 推荐人手机号 |

- Response Datas

| 参数名 | 必填 | 数据类型 | 说明 |
| :-----| ----: | :----: | :----: |
| mobile | 是 | String | 手机号码 |
| smsCode | 是 | String | 短信验证码 |

- Response example

```
{
  "code": 1008,
  "message": "code 无效"
}
```

## 商品模块




## 订单模块





## 售后模块





## 通用模块








