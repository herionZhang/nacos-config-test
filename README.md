# Nacos 2.0.1 Spring Cloud 2020.0.2 项目快速启用配置服务
## nacos server 安装
参考[Linux 下部署 nacos 1.4](https://blog.csdn.net/zh452647457/article/details/109852432)，此处需要安装nacos 2.0.1
## 环境说明
依赖     | 版本
-------- | -----
spring boot| 2.4.5
spring cloud  |2020.0.2
Spring Cloud Alibaba| 2021.1
nacos server|2.0.1

## 添加POM依赖
```
	<properties>
		<alibaba.cloud.version>2021.1</alibaba.cloud.version>
		<nacos.client.version>2.0.1</nacos.client.version>
	</properties>
	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>com.alibaba.cloud</groupId>
				<artifactId>spring-cloud-alibaba-dependencies</artifactId>
				<version>${alibaba.cloud.version}</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>
	<dependencies>
	    <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>com.alibaba.nacos</groupId>
                    <artifactId>nacos-client</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
	    <dependency>
			<groupId>com.alibaba.nacos</groupId>
			<artifactId>nacos-client</artifactId>
			<version>${nacos.client.version}</version>
		</dependency>
	    <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-bootstrap</artifactId>
        </dependency>
	</dependencies>
```
## bootstrap.properties 添加如下配置

```
spring.cloud.nacos.config.server-addr=127.0.0.1:8848
# Nacos 控制台添加配置：
# Data ID：spring-cloud-legends-nacos-example.properties
# Group：DEFAULT_GROUP
# 配置内容：useLocalCache=true
spring.application.name=spring-cloud-legends-nacos-example
# 指定配置的后缀，支持 properties、yaml、yml，默认为 properties
spring.cloud.nacos.config.file-extension=properties
#spring.cloud.nacos.config.file-extension=yaml
```
## nacos  控制台中添加配置
### nacos 控制台点击创建
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210511210634112.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poNDUyNjQ3NDU3,size_16,color_FFFFFF,t_70)
### 新建配置
- Data ID：spring-cloud-legends-nacos-example.properties
- Group：DEFAULT_GROUP
- 配置内容：useLocalCache=true

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210511215601519.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poNDUyNjQ3NDU3,size_16,color_FFFFFF,t_70)


## 测试代码

```
@RestController
@RequestMapping("/config")
@RefreshScope
public class ConfigController {

    @Value("${useLocalCache:false}")
    private boolean useLocalCache;

    /**
     * http://localhost:8080/config/get
     */
    @RequestMapping("/get")
    public boolean get() {
        return useLocalCache;
    }
}
```
## 测试验证
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021051322341960.png)

## 注意
- Spring Cloud Alibaba 2021.1 默认nacos client 为1.4.1，需要配置nacos client 2.01，否则读取不到配置
-  需在bootstrap.properties 中配置spring.main.allow-bean-definition-overriding=true
-  需依赖 spring-cloud-starter-bootstrap，否则读取不到bootstrap 中配置

```
 <dependency>
     <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-bootstrap</artifactId>
   </dependency>
```
## 源码地址
[https://github.com/herionZhang/nacos-config-test](https://github.com/herionZhang/nacos-config-test)
