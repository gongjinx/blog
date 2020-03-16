# Etcd API

etcd v3使用 [gRPC](https://www.grpc.io/)作为其消息传递协议。etcd项目包括一个基于gRPC的 [Go客户端](https://github.com/coreos/etcd/tree/master/clientv3)和一个命令行实用程序 [etcdctl](https://github.com/coreos/etcd/tree/master/etcdctl)，用于通过gRPC与etcd集群进行通信。对于不支持gRPC的语言，etcd提供了JSON [gRPC网关](https://github.com/grpc-ecosystem/grpc-gateway)。该网关提供一个RESTful代理，该代理将HTTP / JSON请求转换为gRPC消息。



etcdctl 默认使用 V2 版本的 API ，可以通过设置环境变量来开启 V3 版本的 API：

```bash
$ export ETCDCTL_API=3
```

在本地直接使用 etcd 命令可以启动一个单机 etcd，我们使用这个单机 etcd 来学习其 API 

写入一个键值对：

```bash
$ etcdctl put foo bar
$ etcdctl put foo1 bar1
$ etcdctl put foo2 bar2
$ etcdctl put foo3 bar3
```

目前找不到 etcdctl 命令的自动补全方案。

读取 key

```bash
$ etcdctl get foo
```

仅打印值：

```bash
$ etcdctl get foo --print-value-only
```

前缀查询：

```bash
$ etcdctl get --prefix foo --print-value-only
```

范围查询：

```bash
$ etcdctl get foo foo3 --print-value-only
```

上述的范围是半开区间：[foo, foo3)

限制结果输出数量：

```bash
$ etcdctl get foo foo3 --limit=2 --print-value-only
```

查询所有的key（从 https://blog.pytool.com/devops/etcd/etcdctl/ 学到的）：

```bash
$ etcdctl get "" --prefix --keys-only | sed '/^\s*$/d'
```



---



## Kubernetes 相关

查询所有 key ：

```bash
$ etcdctl get / --prefix --keys-only
```

查看所有命名空间的 key ：

```bash
$ etcdctl get /registry/namespaces --prefix --keys-only
```

