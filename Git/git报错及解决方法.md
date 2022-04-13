报错：`error: RPC failed; curl 28 OpenSSL SSL_read: Connection was reset, errno 10054`

解决方法：`git config [--global] http.sslVerify "false"`