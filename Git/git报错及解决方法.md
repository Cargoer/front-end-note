报错：

`error: RPC failed; curl 28 OpenSSL SSL_read: Connection was reset, errno 10054`

解决方法：

```shell
git config [--global] http.sslVerify "false"
```



报错：

`fatal: unable to access 'https://github.com/Cargoer/notice-by-chrome-extension.git/': OpenSSL SSL_read: Connection was reset, errno 10054`

解决方法1：

```shell
git config http.proxy "127.0.0.1:10809"
git config https.proxy "127.0.0.1:10809"
git config --unset http.proxy
git config --unset https.proxy
```

解决方法2：（若解决方法1不起作用可用此方法）

```shell
git config http.sslVerify "false"
```

