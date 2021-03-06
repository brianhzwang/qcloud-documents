## 功能描述
Get Bucket ACL 接口用来获取 Bucket 的 ACL(access control list)， 即用户空间的访问权限控制列表。 此API接口只有 Bucket 的持有者有权限操作。

## 请求

语法示例：
```
GET /?acl HTTP/1.1
Host:<BucketName>-<AppID>.<Region>.myqcloud.com
Date: date
Authorization: Auth
```

> Authorization:  Auth (详细参见 [访问控制](http://gggggggg) 章节)

### 请求行
~~~
GET /?acl HTTP/1.1
~~~
该 API 接口接受 GET 请求。
#### 请求参数
**命令参数**
该 API 接口使用到的命令参数为 acl。

### 请求头

#### 公共头部
该请求操作的实现使用公共请求头,了解公共请求头详细请参见[公共请求头部]()章节。

#### 非公共头部
**必选头部**
该请求操作的实现使用如下必选头部：

|参数名称|描述|类型|必选|
|:---|:-- |:--|:--|
| Authorization | 签名串 |String| 是 |

### 请求体
该请求的请求体为空。

## 响应

#### 响应头
**公共响应头** 
该响应使用公共响应头,了解公共响应头详细请参见[公共响应头部]()章节。
**特有响应头**
该响应无特殊有响应头。
#### 响应体
该响应体返回为 **application/xml** 数据，包含完整节点数据的内容展示如下：

```
<AccessControlPolicy>
  <Owner>
    <uin>ID</uin>
  </Owner>
  <AccessControlList>
    <Grant>
      <Grantee type="SubAccount">
        <uin>ID</uin>
        <Subaccount> SUBID </Subaccount>
      </Grantee>
      <Permission>Permission</Permission>
    </Grant>
    <Grant>
      <Grantee type="RootAccount">
        <uin>ID</uin>
      </Grantee>
      <Permission>Permission</Permission>
    </Grant>
    <Grant>
      ...
    </Grant>
  </AccessControlList>
</AccessControlPolicy>
```

具体的数据内容如下：

|节点名称（关键字）|父节点|描述|类型|
|:---|:-- |:--|:--|
| AccessControlPolicy |无| 保存 Get Bucket ACL 结果的容器 | Container |

Container 节点 AccessControlPolicy 的内容：

|节点名称（关键字）|父节点|描述|类型|
|:---|:-- |:--|:--|
| Owner | AccessControlPolicy | Bucket 持有者信息 |  Container |
| AccessControlList | AccessControlPolicy | 被授权者信息与权限信息 |  Container |

Container 节点 Owner 的内容：

|节点名称（关键字）|父节点|描述|类型|
|:---|:-- |:--|:--|
| uin | AccessControlPolicy.Owner |  Bucket 持有者 ID |  String |

Container 节点 AccessControlList 的内容：

| 节点名称（关键字）          |父节点 | 描述                                    | 类型        |
| ------------ | ------------------------------------- | --------- |:--|
| Grant | AccessControlPolicy.AccessControlList | 单个 Bucket 的授权信息。一个 AccessControlList 可以拥有 100 条 Grant | Container    |

Container 节点 Grant 的内容：

| 节点名称（关键字）          |父节点 | 描述                                    | 类型        |
| ------------ | ------------------------------------- | --------- |:--|
| Grantee | AccessControlPolicy.AccessControlList.Grant | 被授权者资源信息。type类型可以为 RootAcount， SubAccount；当 type 类型为 RootAcount 时，可以在 UIN 中填写 QQ，也可以填写 anonymous（指代所有类型用户）。当 type 类型为 RootAcount 时，UIN 代表根账户账号，SubAccount 代表子账户账号  | Container    |
| Permission | AccessControlPolicy.AccessControlList.Grant | 指明授予被授权者的权限信息，枚举值：READ，WRITE，FULL_CONTROL  | String    |

Container 节点 Grantee 的内容：

| 节点名称（关键字）          |父节点 | 描述                                    | 类型        |
| ------------ | ------------------------------------- | --------- |:--|
| uin | AccessControlPolicy.AccessControlList.Grant.Grantee | 用户 QQ 号  | String    |
| Subaccount | AccessControlPolicy.AccessControlList.Grant.Grantee |  子账户的 QQ 账号  | String    |
## 实际案例

### 请求
```
GET /?acl HTTP/1.1
Host:zuhaotestnorth-1251668577.cn-north.myqcloud.com
Authorization:q-sign-algorithm=sha1&q-ak=AKIDWtTCBYjM5OwLB9CAwA1Qb2ThTSUjfGFO&q-sign-time=1484213027;32557109027&q-key-time=1484213027;32557109027&q-header-list=host&q-url-param-list=acl&q-signature=dcc1eb2022b79cb2a780bf062d3a40e120b40652
```

### 响应
```
HTTP/1.1 200 OK
Content-Type: application/xml
Content-Length: 266
Connection: keep-alive
Date: Thu Jan 12 17:23:49 2017
Server: tencent-cos
x-cos-request-id: NTg3NzRiMjVfYmRjMzVfMTViMl82ZGZmNw==

<AccessControlPolicy>
    <Owner>
        <uin>2779643970</uin>
    </Owner>
    <AccessControlList>
        <Grant>
            <Grantee type="RootAccount">
                <uin>2779643970</uin>
            </Grantee>
            <Permission>FULL_CONTROL</Permission>
        </Grant>
    </AccessControlList>
</AccessControlPolicy>
```
