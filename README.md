# 简介
easy框架父pom，在spring-boot-dependencies基础上，增加常用util、三方组件、框架包等，业务系统只需继承该pom，即可免于绝大部分jar包的版本管理；业务系统的包版本需要升级时，仅需升级该父pom，而无需每个业务系统都进行升级。

# 约定
* 最小引用原则，比如 `mysql` 包只有 `dao` 层才会用到，应该将 `mysql` 放到 `dao` 的 `pom` 中。
* 日志推荐使用 `log4j2`，并使用 `slf4j` 做桥接。
* 文件上传推荐使用 `commons-fileupload`，在 `springMVC` 中使用非常简单方便。
* 接口大量入参字段需要校验，推荐使用 `hibernate-validator`。
* 模板引擎推荐使用 `freemarker`，`velocity` 官方近10年没有维护，新版 `spring` 也放弃了对其的兼容。
* 工具类库推荐使用 `apache commons` 包，集合校验推荐使用 `apache commons-collections4` 包，集合创建推荐使用 `google guava` 包，其他推荐使用 `hutool`工具库。

# 快速开始
## 脚手架一键生成方式
[见文档](https://github.com/xiongzhao1217/easy-framework-archetype)

## 继承方式
通过继承`easy-framework-parent` 来引用父 `pom` 中的包。
```xml
<parent>
    <groupId>com.easy.framework</groupId>
    <artifactId>easy-parent</artifactId>
    <version>1.0.0-SNAPSHOT</version>
</parent>

<!-- 由于常用类库的版本在 `easy-parent` 中都已申明，因此在业务系统中不需要再进行申明。 -->
<dependencies>
    <!-- apache工具包 -->
    <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-lang3</artifactId>
    </dependency>
    <!-- google guava -->
    <dependency>
        <groupId>com.google.guava</groupId>
        <artifactId>guava</artifactId>
    </dependency>
<dependencies>
```

## 依赖方式
由于 `pom` 不支持多继承，如果项目需要继承多个 `pom`，可以在 `dependencyManagement` 中引入 `pom`，作用相当于继承， `scope` 需要申明为 `import`。
```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.easy.framework</groupId>
            <artifactId>easy-parent</artifactId>
            <version>${easy.framework.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        <dependency>
            <groupId>com.xx</groupId>
            <artifactId>xxx</artifactId>
            <version>${xxx}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    <dependencies>
<dependencyManagement>

<!-- 由于常用类库的版本在 `easy-parent` 中都已申明，因此在业务系统中不需要再进行申明。 -->
<dependencies>
    <!-- apache工具包 -->
    <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-lang3</artifactId>
    </dependency>
    <!-- google guava -->
    <dependency>
        <groupId>com.google.guava</groupId>
        <artifactId>guava</artifactId>
    </dependency>
<dependencies>
```

# 经验总结
* `dependencies` 后面的依赖会覆盖前面的。
* `dependencyManagement` 中 `type` 为 `pom` 的依赖，前面的优先级更高，后面不能覆盖前面。
* `dependencyManagement` 中 `type` 为 `pom` 的依赖会覆盖普通依赖的间接引用，并且与顺序无关。
* `dependencyManagement` 中的普通依赖会覆盖 `type` 为 `pom` 的依赖，并且与顺序无关。
* `pom` 不支持多继承，可以通过在 `dependencyManagement` 中引入 `type` 为 `pom` 的依赖实现多继承。

