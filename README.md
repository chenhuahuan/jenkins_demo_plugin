# jenkins_demo_plugin
jenkins_demo_plugin, see https://www.jenkins.io/doc/developer/tutorial/prepare/

目前看样子，与平台无关
# demo-plugin_macos_m1
IMPORTANT: 必须用 openjdk 11 
注：https://www.jenkins.io/doc/developer/tutorial/prepare/ 文档中已经指明推荐JDK 11..

```console
export JAVA_HOME=/opt/homebrew/opt/openjdk@11
mvn verify
```

JDK 17不允许internal API访问, see the error prompts:https://www.bennyhuo.com/2021/10/02/Java17-Updates-06-internals/
"xxx to unnamed module...."

如果坚持用JDK17,可以配置pom.xml中关于maven-surefire-plugin插件（mvn test插件主要用该插件运行）的配置，更多详细知识参考：https://www.runoob.com/maven/maven-build-life-cycle.html
```console
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>3.0.0-M3</version>
            <dependencies>
                <dependency>
                    <groupId>org.apache.maven.surefire</groupId>
                    <artifactId>surefire-junit4</artifactId>
                    <version>3.0.0-M3</version>
                </dependency>
            </dependencies>
            <configuration>
                <argLine>
                    --add-opens java.base/java.lang=ALL-UNNAMED
                    --add-opens java.base/java.lang.reflect=ALL-UNNAMED
                    --add-opens java.base/java.util=ALL-UNNAMED
                    --add-opens java.base/java.text=ALL-UNNAMED
                </argLine>
                <skipTests>true</skipTests>
            </configuration>
        </plugin>
    </plugins>
</build>
```
