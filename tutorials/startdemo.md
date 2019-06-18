# 创建 Demo 应用教程

尊敬的用户，感谢您选择 WebX 作为您在阿里云上的应用托管服务，此篇教程将以我们的 demo 向您演示，如何通过 WebX 创建您的第一个应用，步骤如下：

## 首先：下载并安装命令行工具

WebX 在控制台上的所有功能，都会有对应的命令行工具提供，目前的命令行版本支持 Linux 与 Mac 两个系统，您可以通过一下的安装命令下载安装：

```bash
F=/usr/local/bin/webxctl; H=/etc/hosts; wget http://webx-cn-shenzhen.oss-cn-shenzhen.aliyuncs.com/webx-cli/webxctl-linux -O $F; chmod +x $F ; if [ -w $H ]; then echo "100.100.36.39 webx.cn-shenzhen.aliyuncs.com" >> $H ; export ALICLOUD_REGION=cn-shenzhen; export HOME=/home/shell; fi; webxctl auto-completion; source /root/.bashrc 
```

## 第二步：配置命令行工具

下载安装命令行工具之后，我们接下来需要对环境进行配置，配置的具体参数请参考[文档](#)，其中 `profile` 是环境名，且必须填入 `access_key` 与 `access_secret`，这一步如果您是在 cloudshell 中运行，可以省略。

```bash
webxctl configure --profile demo
```

## 第三步：克隆 Java 样例工程

在这一步，如果您有自己的 Java 工程，可以使用您自己的工程即可，您也可以执行以下命令，将我们的样例工程克隆下来，并进入到工程根目录中，如果您是在阿里云 `cloud shell` 中执行，默认已经 clone 好了，您无需额外执行以下命令。 

```bash
git clone https://code.aliyun.com/edas.guyiXs/webx-quickstart-java.git && cd webx-quickstart-java
```

## 第四步：编译并创建 WebX 应用

当 demo 工程 clone 完成之后，下一步，我们需要针对工程进行编译：

```bash
mvn package
```

上传好程序包，同时创建一个 webx 的环境，将此应用运行起来，在这一步，您可以通过控制台和命令行工具均可以达到目的，其中：

1. 在命令行中您可以执行以下命令，来达到同样的效果：

   ```bash
   webxctl apply --package target/webx-quickstart-java*.jar --label WebXVersion0.1 --category Java --newEnv WebXEnv --newApp WebXDemo 
   ```

2. 在WebX控制台中的步骤如下：

   1. 进入到webx[控制台](webx.console.aliyun.com)，同时点击 "**创建应用**"的按钮。
   2. 由于我们的 demo 是一个 FatJar 应用，在 **技术栈** 这一步请选择 **Java**。
   3. 填入相应的*应用名称*与，*应用描述* 然后点击 **下一步** 来创建一个新的部署环境。
   4. 在创建部署环境的地方，填入相应的名称信息，然后选择上传刚刚部署好的部署包。
   5. 在 *版本* 与 *版本描述* 创建的地方填入相应的信息。
   6. 如果您愿意使用 WebX 的推荐配置(以按量的方式为您购买一台 1 核 1 G 的 ECS 实例 )为您创建此次环境，请直接单击 **完成创建**。
   7. 如果您此次希望使用不同的配置如：反向代理、网络、机器实例数，请单击 **下一步** 后继续，并在配置页面完成。



## 第五步：访问创建好的环境

当应用创建好后，本次 WebX 将为您代购配置中所声明的资源信息，在整个的资源准备的过程中，WebX 大概需要花费 **两** 分钟左右，您可以通过控制台和命令行工具来查看运行过程中的日志：

1. 在命令行中，可以依据以下命令进行查看：

   1. 先需要对当前工作目录与 WebX 中的部署环境进行绑定(使用刚刚创建的部署环境的应用和名字：WebXEnv)。

      ```bash
      webxctl app:use WebXEnv; webxctl use WebXEnv 
      ```

   2. 其次，执行查看事件的命令，确认变更事件已经完成。

      ```bash
      webxctl events
      ```

   3. 最后，获取打开应用的连接。

      ```bash
      webxctl info
      ```

2. 在 WebX 控制台中：

   1. 进入到 **应用列表** -> 选择对应的应用与部署环境，进入到环境详情中。
   2. 如果当前有运行中的变更，在**部署环境概览**页面的顶端，会有正在更新环境的提示，您可单击提示连接进行实时的事件更新。
   3. 确保变更事件完成之后，在**部署环境概览**页面中的 *访问地址* 处，点击打开相应的您所托管的应用的页面。


恭喜您，完成了 WebX 的第一个应用托管！WebX，您在阿里云上托管应用的最便捷的助手 :-)

