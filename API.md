
TrustNote Service API
---

此方案为开发者提供支持，使用接口可快速实现查询余额及支付功能。

#### 1. 注册

使用钱包资产需要先在服务上注册钱包公钥，方便钱包之后查询余额及转账。

请求接口：https://beta.itoken.top/api/v1/asset/register

请求方式：POST

提交数据：


参数名称     |  类型    |  是否必需  |  说明
-------------|----------|------------|--------
pubkey       | string   |   是       | 钱包公钥


示例：

```bash
curl -H "Content-type: application/json" -X POST -d '{"pubkey":"AyIiqBX8LaRaBLrvoms3rZ5vivkK63Zy+tTSiSPCtrXK"}' https://beta.itoken.top/api/v1/asset/register
```

响应结果：

```json
{
    "errCode": 0,
    "errMsg": "success",
    "data": {
        "address": "YAZTIHFC7JS43HOYKGNAU7A5NULUUG5T"
    }
}

```

#### 2. 查询余额

请求接口：https://beta.itoken.top/api/v1/query/balance/{address}[/{asset}]

请求方式：GET

示例：

- 查询地址中TTT资产

```
https://beta.itoken.top/api/v1/query/balance/YAZTIHFC7JS43HOYKGNAU7A5NULUUG5T/base
```
- 查询地址中iToken资产

```
// 需要对资产ID进行URL编码
https://beta.itoken.top/api/v1/query/balance/YAZTIHFC7JS43HOYKGNAU7A5NULUUG5T/GYh07uIbuy8yEYNZnCsYvd1pq%2Fw6mYeUslJt5uf5fwg%3D
```

- 查询地址中所有资产

```
https://beta.itoken.top/api/v1/query/balance/YAZTIHFC7JS43HOYKGNAU7A5NULUUG5T
```

响应结果：

参数名称     |  类型    | 说明
-------------|----------|---------
amount       | number   | 余额
asset        | string   | 资产ID

```json
{
    "network": "testnet",
    "errCode": 0,
    "errMsg": "success",
    "data": [
        {
            "amount": 501629,
            "asset": "base"
        },
        {
            "amount": 1000,
            "asset": "GYh07uIbuy8yEYNZnCsYvd1pq/w6mYeUslJt5uf5fwg="
        },
        {
            "amount": 80,
            "asset": "KJZb/bMf7Nk4J3dIWSEUnV+pUe+nUgIRVzrIh1Taq0Q="
        },
        {
            "amount": 1877,
            "asset": "N4r1C7d2gI53QejTs5bx1wm27IrfD9mN2SrPeAoWU4k="
        },
        {
            "amount": 1,
            "asset": "Y0D+oSVqHh0an8vRstwLBxRCG7DCAqoCp/hDeO7ANQk="
        }
    ]
}
```

#### 3. 支付TTT请求

请求接口：https://beta.itoken.top/api/v1/asset/transferttt

请求方式：POST

提交数据：


参数名称     |  类型    |  是否必需  | 说明
-------------|----------|------------|---------
payer        | string   |   是       | 付款者地址
message      | string   |   否       | 附加消息
outputs      | array    |   是       | 收款人列表


示例：

```bash
curl -H "Content-type: application/json" -X POST -d '{"message":"test", "payer":"YAZTIHFC7JS43HOYKGNAU7A5NULUUG5T","outputs":[{"address":"X6DWZUEW4IBFR77I46CAKTJVK4DBPOHE","amount":5},{"address":"2SATGZDFDXNNJRVZ52O4J6VYTTMO2EZR","amount":10},{"address":"JRB6APAHQ4YEKQJ73WMYL3GAXW6GROPQ","amount":5}] }' https://beta.itoken.top/api/v1/asset/transferttt
```

响应结果：

参数名称     |  类型    | 说明
-------------|----------|---------
b64_to_sign  | string   | 待签名unitHash
txid         | string   | 交易ID

```
{
    "errCode": 0,
    "errMsg": "success",
    "data": {
        "b64_to_sign": "iQjSol75QjDLtzapgxZBWPMgxJnRj2IoOO6pt41eBW8=",
        "txid": "XQ/8hrHpYgtVZxAHSdScUaQGjyVaKwsv52q2qmqLtQE="
    }
}
```

#### 4. 提交签名结果

请求接口：https://beta.itoken.top/api/v1/asset/submittx

请求方式：POST

提交数据：


参数名称     |  类型    |  是否必需  | 说明
-------------|----------|------------|---------
txid         | string   |   是       | 交易ID
sig          | string   |   是       | 签名


示例：

```bash
curl -H "Content-type: application/json" -X POST -d '{"txid":"Pr8NxqooWa0NWBhsWTW5MBmer4En8UG7TPhKjtmombk=","sig":"G4O558eJ3mTs0cQTXE2S3nbzdoUyI1sUGE/IaTtfZOFowtvLjkWVg7DWw3jbdZ6sb7uzmgiQNG4cwl5nsL2Ibw=="}' https://beta.itoken.top/api/v1/asset/submittx
```

响应结果：

参数名称     |  类型    | 说明
-------------|----------|---------
txid         | string   | 交易ID
unit         | string   | 交易单元hash

```
{
    "errCode": 0,
    "errMsg": "success",
    "data": {
        "txid": "iQjSol75QjDLtzapgxZBWPMgxJnRj2IoOO6pt41eBW8=",
        "unit": "XQ/8hrHpYgtVZxAHSdScUaQGjyVaKwsv52q2qmqLtQE="
    }
}
```

#### 5. 获取交易历史

请求接口：https://beta.itoken.top/api/v1/query/readabletxs/{address}[/{asset}][/start][/end]

请求方式：GET

示例：

- 查询地址中TTT资产历史

```
curl https://beta.itoken.top/api/v1/query/readabletxs/YAZTIHFC7JS43HOYKGNAU7A5NULUUG5T/base/1/10
```
- 查询地址中iToken资产

```
// 需要对资产ID进行URL编码
curl https://beta.itoken.top/api/v1/query/readabletxs/YAZTIHFC7JS43HOYKGNAU7A5NULUUG5T/GYh07uIbuy8yEYNZnCsYvd1pq%2Fw6mYeUslJt5uf5fwg%3D/1/10
```

响应结果：

参数名称     |  类型    | 说明
-------------|----------|---------
payer        | string   | 支付方地址
payee        | string   | 收款方
fee          | number   | 交易费用
type         | stirng   | 付款或收款
timestamp    | number   | 时间戳
unit         | string   | 交易单元Hash
msg          | string   | 附加消息

```
{
    "network": "testnet",
    "errCode": 0,
    "errMsg": "success",
    "data": [{
        "payer": "VP5IQETBYVNDIE7NSMFQEQEJFEBOYUID",
        "payees": [
            { "address": "VP5IQETBYVNDIE7NSMFQEQEJFEBOYUID", "amount": 863702 }
        ],
        "fee": 958,
        "type": "received",
        "timestamp": 1532618333,
        "unit": "dcFOdhOiJUsbJskM/LWE+H4EOpA5jwiMvvYGS6ksIUc=",
        "msg": "GYh07uIbuy8yEYNZnCsYvd1pq/w6mYeUslJt5uf5fwg= 资产发行"
    }]
}

```
