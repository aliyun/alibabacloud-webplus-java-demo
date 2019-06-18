# 创建 Demo 应用教程

尊敬的用户，感谢您选择 Web+（Web Plus）作为您在阿里云上的应用托管服务，此篇教程将以 Demo 向您演示，如何通过 Web+（Web Plus）创建您的第一个应用，步骤如下，请点击右下脚的“下一步”：

## 第一步：下载并安装命令行工具

Web+ 在控制台上的所有功能，都会有对应的命令行工具提供，目前的命令行版本支持 Linux 与 Mac 两个系统，执行以下命令下载安装命令行工具：

```bash
eval "$(curl -s -L https://webplus-cn-shenzhen.oss-cn-shenzhen.aliyuncs.com/cli/install.sh)"
```



## 第二步：配置命令行工具

下载安装命令行工具之后，接下来需要对环境进行配置，配置的具体参数请参考[文档](#)，其中 `profile` 是环境名，且必须填入 `access_key` 与 `access_secret`。

```
wpctl configure --access-key-id "$ALICLOUD_ACCESS_KEY" --access-key-secret "$ALICLOUD_SECRET_KEY" --region "$ALICLOUD_REGION"  --profile demo
```

> 如果您是在 Cloud Shell 中运行，我们不推荐设置此步骤，如果您在自己的机器上，请确保已设置 `ALICLOUD_ACCESS_KEY` ,`ALICLOUD_SECRET_KEY`, `ALICLOUD_REGION` 三个环境变量。如果在 Cloud Shell 中强行设置以上命令，默认区域为杭州，此处不会跟着您的控制台选择的区域而变化。

## 第三步：克隆 Java 样例工程

如果您有自己的 Java 工程，可以使用您自己的工程即可，您也可以执行以下命令，将我们的 Demo 工程克隆下来，并进入到工程根目录中。 

```bash
git clone https://github.com/aliyun/alibabacloud-webplus-demo-java.git && cd alibabacloud-webplus-demo-java
```



## 第四步：编译并创建 Web+ 应用

当 Demo 工程克隆完成之后，我们需要针对工程进行编译：

```bash
mvn package
```

上传好程序包，同时创建一个 Web+ 的环境，将此应用运行起来，在这一步，您可以通过控制台和命令行工具均可以达到目的，其中：

- 在命令行中您可以执行以下命令：

   ```bash
   wpctl env:apply --package target/webplus-*-exec.jar --label webplusVersion0.1 --category Java --env webplusEnvDemo --app webplusAppDemo --create-on-absent
   ```
   > 在这一步，指定对应的应用名称与环境名称，同时指定"—create-on-absent"，用以标明如果不存在此应用或者环境时，将创建相应的资源。

- 在Web+控制台中的步骤如下：

   1. 进入到 Web+ [控制台](webplus.console.aliyun.com)，在概览页的最近更新的部署环境区域的右上角单击 **新建** 的按钮。
   2. 在 **应用基本信息** 页签选择 **技术栈** 为 **Java** ，并输入 **应用名称** 和 **应用描述** ，设置完成后单击 **下一步** 。<br>**说明** ：因为提供的Demo是一个 **FatJar** 应用，故 **技术栈** 选择 **Java**。
   3. 在 **部署环境信息** 页签设置环境名称并上传部署包，然后输入 **部署包版本** 和 **版本描述**。
      * 如果您愿意使用Web+的推荐配置（以按量的方式为您购买一台 1vCPU 1GiB 的ECS实例）为您创建部署环境，请单击 **完成创建**
      * 如果您希望自定义环境的配置：反向代理、VPC、ECS实例规格或SLB等，请单击 **下一步**，然后在 **配置** 页签设置环境信息。





## 第五步：访问创建好的环境

当应用创建好后，本次 Web+ 将为您代购配置中所声明的资源信息，在整个的资源准备的过程中，Web+ 大概需要花费**两**分钟左右，您可以通过控制台或命令行工具来查看运行过程中的日志：

- 在命令行中，可以依据以下命令进行查看：

   1. 先需要对当前工作目录与 Web+ 中的部署环境进行绑定（使用刚刚创建的部署环境的应用和名字：Web+Env ）。

      ```bash
      wpctl env:use webplusEnvDemo --app webplusAppDemo
      ```

   2. 其次，执行查看事件的命令，确认变更事件已经完成。

      ```bash
      wpctl env:events
      ```

   3. 最后，获取打开应用的连接。

      ```bash
      wpctl env:info
      ```

- 在 Web+ 控制台中：

   1. 进入到 **应用列表**，选择对应的应用与部署环境，进入到环境详情中。
   2. 如果当前有运行中的变更，在**部署环境概览**页面的顶端，会有正在更新环境的提示，您可单击提示连接进行实时的事件更新。
   3. 确保变更事件完成之后，在**部署环境概览**页面中的 *访问地址* 处，点击打开相应的您所托管的应用的页面。


## 第六步：释放刚刚创建好的环境

由于 Web+ 的生成所有环境都是按量付费的模式购买，如果您想保留创建好的实例，您暂时可以跳转至 ECS 的控制台，将对应的实例转成包年包月的 ECS 实例。

默认情况下，当我们的环境释放后，Web+ 将停止为您代购的 ECS，由于 VPC 内的实例停止之后，不再收取费用，所以默认情况下 Web+ 将环境释放之后将不再收取您的 ECS 实例。释放环境的方式分别在控制台上与 CLI 里的方式为：

- 在 CLI 中：

   ~~~bash
   wpctl env:terminate --app webplusAppDemo --env webplusEnvDemo
   ~~~

- 在控制台中，进入到相应的环境首页中后，单击右上角按钮组中的**释放**按钮。



## 结束语

恭喜您，完成了 Web+ 的第一个应用托管！Web+，您在阿里云上托管应用的最便捷的助手 :-)

