---
title: 微信支付签名算法（JavaScript版）
date: 2019-05-21 22:16:30
tags: [微信支付]
---

MD5算法[参考][1]
另外可能存在的问题：似乎没有测试中文（utf8编码）

晚上看到社区的[这篇文章][2]，用于实现微信支付的，明天争取也能跑通

```javascript
const MD5 = hex_md5 // http://pajhome.org.uk/crypt/md5/md5.html

const openid = 'oUpF8EMuAJO_M2Xxb1Q3zNjWeS33'

const params = {
  appid: "wxd9309a5d2a258f31",
  mch_id: "10000100",
  device_info: "WEB",
  body: "JSAPI支付测试",
  notify_url: 'httts://www.google.com/',
  openid: openid,
  out_trade_no: '100000000001',
  spbill_create_ip: '111.194.196.105',
  total_fee: '1',
  trade_type: 'JSAPI',
  nonce_str: getNonceStr(),
}

const body = genRequstBody(params)
console.log(body)

// ----------- genRequstBody
function genRequstBody(params) {
  const stringA = Object.keys(params).sort().map(k => {
    return k + '=' + params[k]
  }).join('&')

  console.log(stringA)

  const stringSignTemp=stringA+"&key=192006250b4c09247ec02edce69f6a2d" //注：key为商户平台设置的密钥key
  const sign = MD5(stringSignTemp).toUpperCase() // ="9A0A8659F005D6984697E2CA0A9CF3B7" //注：MD5签名方式

  console.log(sign)

  let body = Object.keys(params).sort().map(k => {
    return '<' + k + '>' + params[k] + '</' + k + '>'
  }).join('\n')
  body += '\n<sign>' + sign + '</sign>\n'
  const xml = '<xml>\n' + body + '</xml>'
  return xml
}

// ----------- nonce_str
function makeid(length) {
  var result           = '';
  var characters       = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
  var charactersLength = characters.length;
  for ( var i = 0; i < length; i++ ) {
     result += characters.charAt(Math.floor(Math.random() * charactersLength));
  }
  return result;
}

function getNonceStr() {
  return makeid(31)
}
```


[1]: http://pajhome.org.uk/crypt/md5/md5.html
[2]: https://developers.weixin.qq.com/community/develop/doc/000620ec5acb482103b7bf41d51804
