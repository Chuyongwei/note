提供者

com.atguigu.gmall.config.MyDubboConfig

```java
package com.atguigu.gmall.config;

import java.util.ArrayList;
import java.util.List;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.alibaba.dubbo.config.ApplicationConfig;
import com.alibaba.dubbo.config.ConsumerConfig;
import com.alibaba.dubbo.config.MethodConfig;
import com.alibaba.dubbo.config.MonitorConfig;
import com.alibaba.dubbo.config.ProtocolConfig;
import com.alibaba.dubbo.config.RegistryConfig;
import com.alibaba.dubbo.config.ServiceConfig;
import com.atguigu.gmall.service.UserService;

@Configuration
public class MyDubboConfig {
	
	@Bean
	public ApplicationConfig applicationConfig() {
		ApplicationConfig applicationConfig = new ApplicationConfig();
		applicationConfig.setName("boot-user-service-provider");
		return applicationConfig;
		
	}
	
	//<dubbo:registry protocol="zookeeper" address="127.0.0.1:2181"></dubbo:registry>
	public RegistryConfig registryConfig() {
		RegistryConfig registryConfig = new RegistryConfig();
		registryConfig.setProtocol("zookeeper");
		registryConfig.setAddress("127.0.0.1:2181");
		return registryConfig;
	}
	
	//<dubbo:protocol name="dubbo" port="20880"></dubbo:protocol>
	public ProtocolConfig protocolConfig() {
		ProtocolConfig protocolConfig = new ProtocolConfig();
		protocolConfig.setName("dubbo");
		protocolConfig.setPort(20882);
		return protocolConfig;
	}
	
	//<dubbo:service  interface="com.atguigu.gmall.service.UserService" ref="userServeiceImpl"></dubbo:service>
	public ServiceConfig<UserService> userserviceConfig(UserService userService){
		ServiceConfig<UserService> serviceConfig = new ServiceConfig<UserService>();
		
		serviceConfig.setInterface(UserService.class);
		serviceConfig.setRef(userService);
		serviceConfig.setVersion("1.0.0");
		
		MethodConfig methodConfig = new MethodConfig();
		methodConfig.setName("getUserAddressList");
		methodConfig.setTimeout(1000);
		
		//将method保存到srevice
		List<MethodConfig> methods = new ArrayList<MethodConfig>();
		methods.add(methodConfig);
		
		serviceConfig.setMethods(methods);
//		ProtocolConfig
//		MonitorConfig
		return serviceConfig;
	}
	
	@Bean
	public ConsumerConfig consumerConfig() {
	   ConsumerConfig consumerConfig = new ConsumerConfig();
	   consumerConfig.setCheck(false);
	   consumerConfig.setTimeout(20000);
	   return consumerConfig;
	}

}

```

