## 如何获取命令行输出特定字符后的n行文字
`ifconfig |grep en0 -A 4`

```
localhost:~ seanren$ ifconfig |grep en0 -A 4
en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	ether f4:5c:89:8f:7d:9b
	inet6 fe80::f65c:89ff:fe8f:7d9b%en0 prefixlen 64 scopeid 0x4
	inet 192.168.16.124 netmask 0xfffffe00 broadcast 192.168.17.255
	nd6 options=1<PERFORMNUD>
	media: autoselect
	status: active 
```

## dig 命令
dig - DNS lookup utility

