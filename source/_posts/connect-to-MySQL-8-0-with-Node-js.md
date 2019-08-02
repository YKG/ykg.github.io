---
title: Node.js连接MySQL 8.0 
date: 2019-08-02 18:35:28
tags: [MySQL]
---

开始使用`require('mysql')`的库，但没连成功，似乎只对5.x有效，stackoverflow上说是因为8.0修改了登录方式，可以将mysql指定为原来的模式，但我就按照8.0的方式来，以后估计还是会继承新的登录方式吧。

insert例子：

```javascript
var mysqlx = require('@mysql/xdevapi');
var client = mysqlx.getClient(
    { user: 'root', host: 'hostname', port: 33060, password : 'rootPass', schema: 'CAN-OMIT' }, // 需要是默认的33060端口，而不是3306，估计是8.0开始多出来的33060
    { pooling: { enabled: true, maxIdleTime: 30000, maxSize: 10, queueTimeout: 10000 } }
);

function saveToDB(busNo, direction, vehicleNo, ts, stationNo) {
    client.getSession()
        .then(session => {
            console.log('INSERT', busNo, direction, vehicleNo, ts, stationNo)

            let res = session.sql("insert into log(bus_no, direction, vehicle_no, ts, station_no) values (" + busNo
                + ", " + direction + ", '" + vehicleNo + "', " + ts + ", " + stationNo +")").execute(function (r) {
                console.log(r)
            }, console.error)

            return session.close() // the connection becomes idle in the client pool
        })
}
```
