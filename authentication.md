The API is authenticated to using RFC2617 Http Basic Authentication. This requires a username and password to be submitted as a HTTP-Header using the following scheme.

```
Authorization: Basic QWxhZGRpbjpPcGVuU2VzYW1l
```

Where the username:password is UTF-8 Base64 encoded. The following example shows the username and password encoded as an Authorization HTTP-Header.

```
Username: test
password: testpassword
```

The resultant HTTP-Header would be include the encoded test:testpassword

```
Authorization: Basic dGVzdDp0ZXN0cGFzc3dvcmQ=
```

The username:password is then authenticated against those defined by the APIs configuration. The user details are then constructed to include those organisations that user is allowed to act in behalf of - which is either one or all organisations - as well as the role of ADMIN or normal USER.

**Note:** _Only ADMINS can use the approvals API._
