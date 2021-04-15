CAS Overlay Template [![Build Status](https://travis-ci.org/apereo/cas-overlay-template.svg?branch=master)](https://travis-ci.org/apereo/cas-overlay-template)
=======================

通用的 CAS WAR overlay 是用来练习最新版的 CAS 的。这个overlay 可以作为本地 CAS war 覆盖的启动模板自由使用。

# 版本

- CAS `6.1.x`
- JDK `11`

# 概要

要构建项目，使用：

```bash
# Use --refresh-dependencies to force-update SNAPSHOT versions
./gradlew[.bat] clean build
```

To see what commands are available to the build script, run:

```bash
./gradlew[.bat] tasks
```

To launch into the CAS command-line shell:

```bash
./gradlew[.bat] downloadShell runShell
```

To fetch and overlay a CAS resource or view, use:

```bash
./gradlew[.bat] getResource -PresourceName=[resource-name]
```

To list all available CAS views and templates:

```bash
./gradlew[.bat] listTemplateViews
```

To unzip and explode the CAS web application file and the internal resources jar:

```bash
./gradlew[.bat] explodeWar
```

# Configuration

- The `etc` directory contains the configuration files and directories that need to be copied to `/etc/cas/config`.

```bash
./gradlew[.bat] copyCasConfiguration
```

- The specifics of the build are controlled using the `gradle.properties` file.

## 添加模块

CAS 模块可以在[Gradle build script](build.gradle) 的 `dependencies` 块下指定:

```gradle
dependencies {
    compile "org.apereo.cas:cas-server-some-module:${project.casVersion}"
    ...
}
```

To collect the list of all project modules and dependencies:

```bash
./gradlew[.bat] allDependencies
```

### 清理 Gradle 缓存

在 Linux/Unix 系统, 如果你需要，你可以删除所有已存在的 Gradle 已下载的工件 (artifacts and metadata)，使用下面的方式:

```bash
# Only do this when absolutely necessary
rm -rf $HOME/.gradle/caches/
```

如果你在上面的命令中切换 `$HOME` 到同样的位置，同样的策略也适用于 Windows。

# 部署

- 在 `/etc/cas` 目录下创建 keystore 文件 `thekeystore` 。 密码 `changeit` 用于 keystore 和 key/certificate 实体。可以使用 JDK 的 `keytool` 工具，或者通过下面的命令完成：

```bash
./gradlew[.bat] createKeystore
```

- 确保 keystore 被服务器的 keys 和 certificates 加载。

成功部署后，CAS 在下面地址可用：

* `https://cas.server.name:8443/cas`

## 可执行 WAR

以可执行 WAR 运行 CAS web 应用：

```bash
./gradlew[.bat] run
```

Debug the CAS web application as an executable WAR:

```bash
./gradlew[.bat] debug
```

Run the CAS web application as a *standalone* executable WAR:

```bash
./gradlew[.bat] clean executable
```

## External

Deploy the binary web application file `cas.war` after a successful build to a servlet container of choice.

## Docker

The following strategies outline how to build and deploy CAS Docker images.

### Jib

The overlay embraces the [Jib Gradle Plugin](https://github.com/GoogleContainerTools/jib) to provide easy-to-use out-of-the-box tooling for building CAS docker images. Jib is an open-source Java containerizer from Google that lets Java developers build containers using the tools they know. It is a container image builder that handles all the steps of packaging your application into a container image. It does not require you to write a Dockerfile or have Docker installed, and it is directly integrated into the overlay.

```bash
./gradlew build jibDockerBuild
```

### Dockerfile

You can also use the native Docker tooling and the provided `Dockerfile` to build and run CAS.

```bash
chmod +x *.sh
./docker-build.sh
./docker-run.sh
```
