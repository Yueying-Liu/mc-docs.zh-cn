---
title: 在 HDInsight 上为 Hadoop 服务启用堆转储 - Azure | Azure
description: 为基于 Linux 的 HDInsight 群集中的 Hadoop 服务启用堆转储，以便进行调试和分析。
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 8f151adb-f687-41e4-aca0-82b551953725
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
origin.date: 01/02/2020
ms.date: 02/24/2020
ms.author: v-yiso
ms.openlocfilehash: 8203731b97b834f8739bbce8ded48b543990429a
ms.sourcegitcommit: 3a8a7d65d0791cdb6695fe6c2222a1971a19f745
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2020
ms.locfileid: "85516572"
---
# <a name="enable-heap-dumps-for-apache-hadoop-services-on-linux-based-hdinsight"></a>在基于 Linux 的 HDInsight 上为 Apache Hadoop 服务启用堆转储

[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

堆转储包含应用程序的内存快照，其中包括创建转储时各变量的值。 因此，它们在诊断发生在运行时的问题时很有用。

## <a name="services"></a><a name="whichServices"></a>服务

可以启用以下服务的堆转储：

* **Apache hcatalog** - tempelton
* **Apache hive** - hiveserver2、metastore、derbyserver
* **mapreduce** - jobhistoryserver
* **Apache yarn** - resourcemanager、nodemanager、timelineserver
* **Apache hdfs** - datanode、secondarynamenode、namenode

还可以启用映射的堆转储，并减少由 HDInsight 运行的流程数。

## <a name="understanding-heap-dump-configuration"></a><a name="configuration"></a>了解堆转储配置

在某项服务启动时，可以通过将选项（有时称为 opts 或参数）传递到 JVM 来启用堆转储。 对于大多数 [Apache Hadoop](https://hadoop.apache.org/) 服务，可以修改用于启动该服务的 shell 脚本来传递这些选项。

在每个脚本中，有一个针对 **\*\_OPTS** 的导出，其中包含传递到 JVM 的选项。 例如，在 **hadoop-env.sh** 脚本中，以 `export HADOOP_NAMENODE_OPTS=` 开头的行包含用于 NameNode 服务的选项。

映射和化简进程稍有不同，因为这些操作是 MapReduce 服务的子进程。 每个映射或化简进程都在子容器中运行，并且有两个包含 JVM 选项的条目。 二者均包含在 **mapred-site.xml** 中：

* **mapreduce.admin.map.child.java.opts**
* **mapreduce.admin.reduce.child.java.opts**

> [!NOTE]  
> 我们建议使用 [Apache Ambari](https://ambari.apache.org/) 来修改脚本和 mapred-site.xml 设置，因为 Ambari 负责在群集中跨节点复制更改。 请参阅[使用 Apache Ambari](#using-apache-ambari) 部分以了解具体的步骤。

### <a name="enable-heap-dumps"></a>启用堆转储

发生 OutOfMemoryError 时，可以使用以下选项来启用堆转储：

    -XX:+HeapDumpOnOutOfMemoryError

+ 指示是否启用了此选项。 默认为禁用。

> [!WARNING]
> 默认情况下，在 HDInsight 上不为 Hadoop 服务启用堆转储，因为转储文件可能很大。 如果启用了堆转储来进行故障诊断，请记住在重现问题并收集转储文件后禁用堆转储。

### <a name="dump-location"></a>转储位置

转储文件的默认位置是当前的工作目录。 可以使用以下选项来控制文件的存储位置：

    -XX:HeapDumpPath=/path

例如，使用 `-XX:HeapDumpPath=/tmp` 可以将转储存储在 /tmp 目录中。

### <a name="scripts"></a>脚本

还可以在发生 **OutOfMemoryError** 时触发一个脚本。 例如，可以触发一个通知，这样你就知道发生了错误。 使用以下选项在 __OutOfMemoryError__ 上触发某个脚本：

    -XX:OnOutOfMemoryError=/path/to/script

> [!NOTE]  
> 由于 Apache Hadoop 是分布式系统，任何使用的脚本都必须放置在服务运行时所在的群集的所有节点上。
> 
> 该脚本还必须位于可供帐户（服务以该帐户的身份运行）访问的位置，并且必须提供执行权限。 例如，你可能希望将脚本存储在 `/usr/local/bin` 中，并通过 `chmod go+rx /usr/local/bin/filename.sh` 来授予读取和执行权限。

## <a name="using-apache-ambari"></a>使用 Apache Ambari

若要修改服务配置，请使用以下步骤：

1. 在 Web 浏览器中导航到 `https://CLUSTERNAME.azurehdinsight.cn`，其中的 `CLUSTERNAME` 是群集的名称。

2. 使用左侧的列表，选择你想要修改的服务区。 例如，**HDFS**。 在中心区域，选择“配置”  选项卡。

    ![“HDFS 配置”选项卡已选定的 Ambari 网站的图像](./media/hdinsight-hadoop-collect-debug-heap-dump-linux/hdi-service-config-tab.png)

3. 使用“筛选...”  条目，输入“opts”  。 仅显示包含此文本的项。

    ![筛选的列表](./media/hdinsight-hadoop-collect-debug-heap-dump-linux/hdinsight-filter-list.png)

4. 查找需为其启用堆转储的服务的 **\*\_OPTS** 条目，并添加希望启用的选项。 在下图中，已将 `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` 添加到 **HADOOP\_NAMENODE\_OPTS** 条目：

    ![HADOOP_NAMENODE_OPTS with -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/](./media/hdinsight-hadoop-collect-debug-heap-dump-linux/hadoop-namenode-opts.png)

   > [!NOTE]  
   > 为映射或化简子进程启用堆转储时，需查找名为 **mapreduce.admin.map.child.java.opts** 和 **mapreduce.admin.reduce.child.java.opts** 的字段。

    使用“保存”  按钮保存所做的更改。 可以输入简短的说明，描述所做的更改。

5. 一旦应用了所做的更改，“需要重启”  图标会显示在一个或多个服务旁边。

    ![需要重新启动图标和重新启动按钮](./media/hdinsight-hadoop-collect-debug-heap-dump-linux/restart-required-icon.png)

6. 选择需要重启的每个服务，并使用“服务操作”  按钮以“打开维护模式”  。 维护模式可以防止重启服务时从该服务生成警报。

    ![打开维护模式菜单](./media/hdinsight-hadoop-collect-debug-heap-dump-linux/hdi-maintenance-mode.png)

7. 一旦启用维护模式，使用服务的“重启”  按钮即可“重启所有受影响的服务” 

    ![重新启动受影响的所有条目](./media/hdinsight-hadoop-collect-debug-heap-dump-linux/hdi-restart-all-button.png)

   > [!NOTE]  
   > 其他服务的“重启”  按钮条目可能会有所不同。

8. 一旦重启服务，可使用“服务操作”  按钮“关闭维护模式”  。 这样一来，Ambari 就可以继续监视服务的警报。

<!--Update_Description: update meta data-->