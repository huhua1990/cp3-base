## cp3-cache-starter

将分布式缓存redis 和 内存caffeine 的一些基本方法抽象出来，达到只有切换的目的。业务系统使用时，注入 CacheRepository 类即可。
但这样做又有几个缺陷： redis 的一些特殊方法将无法使用(如： list、set、hash)。

### 要用Redis的特有方法怎么办？
配置文件配置cp3.cache.type=redis后，注入 RedisRepositoryImpl 或者 RedisTemplate， 但必须保证系统每次启动都依赖redis即可。


###为啥要抽象？
- 1, 项目比较小（基本都是CRUD功能），而且团队中会优雅使用redis的比较少，而且会频繁的复制代码到N个项目，每个项目随时都可能会重新部署或者迁移一套环境用于演示，
这里就是想让一些部署去演示的项目，直接用内存缓存即可，少部署一个redis。
- 2, 开发电脑配置比较低，启动太多中间件会很卡，对于专心编码的用户来说，少启动一个中间件，对开发的体验比较好




###如何切换缓存使用redis 还是caffeine？
1, redis
```
pom.xml
<dependency>
    <groupId>com.cp3.cloud</groupId>
    <artifactId>cp3-cache-starter</artifactId>
</dependency>

application.yml
cp3:
  ip:
    redis: 127.0.0.1
  port:
    redis: 16379
  cache:
    type: REDIS
```
2, caffeine
```
pom.xml
<dependency>
    <groupId>com.cp3.cloud</groupId>
    <artifactId>cp3-cache-starter</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </exclusion>
    </exclusions>
</dependency>

application.yml
cp3:
  cache:
    type: CAFFEINE
```
