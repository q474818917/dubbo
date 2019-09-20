## 流程-暴露服务
+ URL协议串
```$xslt
dubbo://10.0.108.46:20880/com.alibaba.dubbo.demo.DemoService?anyhost=true&application=demo-provider&bean.name=com.alibaba.dubbo.demo.DemoService&bind.ip=10.0.108.46&bind.port=20880&channel.readonly.sent=true
&codec=dubbo&dubbo=2.0.2&generic=false&heartbeat=60000&interface=com.alibaba.dubbo.demo.DemoService&methods=sayHello&pid=5392&qos.port=22222&side=provider&timestamp=1568792813623
```
+ DubboNamespaceHandler
```$xslt
registerBeanDefinitionParser("service", new DubboBeanDefinitionParser(ServiceBean.class, true));
```

+ ServiceBean-export
+ ServiceConfig-export-doExportUrls-doExportUrlsFor1Protocol
+ StubProxyFactoryWrapper -> JavassistProxyFactory - getInvoker
+ ProtocolFilterWrapper -> ProtocolListenerWrapper - export
+ RegistryProtocol -> DubboProtocol -export
+ DubboProtoco-openServer-createServer
+ HeaderExchanger-> HeaderExchangerServer(NettyTransporter(NettyTransporter))

## 传输层：
1、通过HeaderExchanger创建HeaderExchangerServer，其中HeaderExchangerServer包装着NettyServer
2、创建Netty Server和Client都是有NettyTransporter完成 
3、Transports、Exchangers都是facade类，静态创建(NettyServer、NettyClient)、(Exchanger、ExchangeServer、ExchangeClient)
```$xslt
Netty
```

## 服务export
```
1、RegistryProtocol先调用DubboProtocol到处到本地
2、拿到本地的export对应的providerUrl，再传到远程注册中心：zk
3、providerUrl会在包装成NettyChannel，NettyChannel对应着原生的Channel，所以根据不同的URL会有对应不同Channel
4、序列化方式:默认Hessian、Java、fastJson、fst(非fst树，而是fast java)、kryo
```

## 负载均衡
```
1、策略包含：默认random、roundrobin、leastActive、Consistenthash

```

## 相关参考
+ https://blog.csdn.net/dachengxi/article/details/62567065