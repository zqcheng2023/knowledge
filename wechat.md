# WeChat Authorization Related 
| OpenID | UnionID |
| ----------- | ----------- |
| per app | per user |

# Authorization Steps
| Url |
| -----------|
| https://open.weixin.qq.com/connect/oauth2/authorize?appid=APPID&redirect_uri=REDIRECT_URI&response_type=code&scope=SCOPE&state=STATE#wechat_redirect |
| https://api.weixin.qq.com/sns/oauth2/access_token?appid=APPID&secret=SECRET&code=CODE&grant_type=authorization_code |
| https://api.weixin.qq.com/sns/oauth2/refresh_token?appid=APPID&grant_type=refresh_token&refresh_token=REFRESH_TOKEN |

## Step 1: get code
### appid, redirect_uri, scope
this code will expire in 5 min
```
https://open.weixin.qq.com/connect/oauth2/authorize
?appid=APPID
&redirect_uri=REDIRECT_URI
&response_type=code
&scope=SCOPE
&state=STATE
#wechat_redirect

// Response
redirect_uri/?code=CODE&state=STATE
```
| Scope | Description |
| ----------- | ----------- |
| snsapi_base | openID. silently approve |
| snsapi_userinfo | user info. Need user action |

## Step 2: get access_token
### appid, secret, code, grant_type
```
https://api.weixin.qq.com/sns/oauth2/access_token
?appid=APPID
&secret=SECRET
&code=CODE
&grant_type=authorization_code

// Response
{
  "access_token":"ACCESS_TOKEN",
  "expires_in":7200,
  "refresh_token":"REFRESH_TOKEN",
  "openid":"OPENID",
  "scope":"SCOPE",
  "is_snapshotuser": 1,
  "unionid": "UNIONID" //只有当scope为"snsapi_userinfo"时返回
}
```
| Token | Description |
| ----------- | ----------- |
| access_token | short lived. Expire in 7200s |
| refresh_token | used to refresh access token. Expire in 30 days |

## Step 3: Refresh access_token
### appid, grant_type, refresh_token
```
https://api.weixin.qq.com/sns/oauth2/refresh_token
?appid=APPID
&grant_type=refresh_token
&refresh_token=REFRESH_TOKEN

// Response
{ 
  "access_token":"ACCESS_TOKEN",
  "expires_in":7200,
  "refresh_token":"REFRESH_TOKEN",
  "openid":"OPENID",
  "scope":"SCOPE" 
}
```


