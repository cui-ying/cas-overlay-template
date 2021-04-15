# 我的经验

## windows 下运行命令：

1. 生成 SSL 证书及密钥库等文件

   下面这个是官方提供的，我改了一部分源码，感觉太麻烦，直接用上面的命令了。 
   ```bash
    ./gradlew.bat createKeystore
    ```


1. 每次修改配置文件后，需要执行以下命令，将配置文件拷贝到指定目录，默认为：`x:\etc\cas\config\cas.properties` 。
    ```bash
    ./gradlew.bat copyCasConfiguration
    ```
    如果提示有警告信息，可用使用下面的命令显示警告的详细信息：
    ```bash
    ./gradlew.bat copyCasConfiguration --warning-mode all
    ```

1. 运行
    ```bash
    ./gradlew.bat run
    ```

1. 自带的 `tasks.gradle` 脚本文件中，生成 SSL 证书的脚本对windows 不够友好，并且有些配置写死了。我给重构了下。支持自定义，可在`gradle.properties` 中定义。

