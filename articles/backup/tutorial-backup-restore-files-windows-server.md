---
title: 教程：将项目恢复到 Windows Server
description: 本教程介绍如何使用 Azure 恢复服务 (MARS) 代理将项目从 Azure 恢复到 Windows Server。
ms.topic: tutorial
author: Johnnytechn
ms.date: 09/22/2020
ms.custom: mvc
ms.author: v-johya
ms.openlocfilehash: fe63dd126f2cb789a948affaad77553631fe1879
ms.sourcegitcommit: cdb7228e404809c930b7709bcff44b89d63304ec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2020
ms.locfileid: "91402364"
---
# <a name="recover-files-from-azure-to-a-windows-server"></a>将文件从 Azure 恢复到 Windows Server

使用 Azure 备份可以从 Windows Server 备份中恢复个别项目。 如果必须快速还原意外删除的文件，恢复个别文件会很有帮助。 本教程介绍如何使用 Azure 恢复服务 (MARS) 代理从已在 Azure 中执行的备份中恢复项目。 本教程介绍如何执行下列操作：

> [!div class="checklist"]
>
> * 启动个别项目的恢复
> * 选择恢复点
> * 从恢复点还原项目

本教程假设你已执行[将 Windows Server 备份到 Azure](backup-windows-with-mars-agent.md) 的步骤，并且在 Azure 中拥有至少一个 Windows Server 文件的备份。

## <a name="initiate-recovery-of-individual-items"></a>启动个别项目的恢复

使用 Azure 恢复服务 (MARS) 代理安装名为“Azure 备份”的有用用户界面向导。 Azure 备份向导与 Azure 恢复服务 (MARS) 代理协同工作，从存储在 Azure 中的恢复点检索备份数据。 使用 Azure 备份向导确定要还原到 Windows Server 的文件或文件夹。

1. 打开“Azure 备份”管理单元。 可以通过在计算机中搜索“Azure 备份”找到该代理。

    ![Azure 备份管理单元](./media/tutorial-backup-restore-files-windows-server/mars.png)

2. 在向导中，选择代理控制台的“操作窗格”中的“恢复数据”，以启动“恢复数据”向导  。

    ![选择恢复数据](./media/tutorial-backup-restore-files-windows-server/mars-recover-data.png)

3. 在“入门”页上，选择“此服务器(服务器名称)”，然后选择“下一步”  。

4. 在“选择恢复模式”页上，选择“单个文件和文件夹”，然后选择“下一步”以开始恢复点选择过程  。

5. 在“选择卷和日期”页上，选择包含想要还原的文件或文件夹的卷，然后选择“装载” 。 选择日期，然后从下拉菜单中选择与恢复点相对应的时间。 以**粗体**显示的日期指示当天至少有一个可用的恢复点。

    ![选择卷和日期](./media/tutorial-backup-restore-files-windows-server/mars-select-date.png)

    选择“装载”时，Azure 备份会以磁盘的形式提供恢复点。 从磁盘浏览和恢复文件。

## <a name="restore-items-from-a-recovery-point"></a>从恢复点还原项目

1. 装载恢复卷后，选择“浏览”以打开 Windows 资源管理器，并查找希望恢复的文件和文件夹。

    ![选择浏览](./media/tutorial-backup-restore-files-windows-server/mars-browse-recover.png)

    可以从恢复卷直接打开文件，并验证这些文件。

2. 在 Windows 资源管理器中，复制想要还原的文件和文件夹，然后将它们粘贴到服务器的任何所需位置。

    ![复制文件和文件夹](./media/tutorial-backup-restore-files-windows-server/mars-final.png)

3. 完成还原文件和文件夹后，在“恢复数据”向导的“浏览和恢复文件”页上，选择“卸载”  。

    ![选择卸载](./media/tutorial-backup-restore-files-windows-server/unmount-and-confirm.png)

4. 选择“是”，以确认要卸载该卷。

    卸载快照后，代理控制台的“作业”窗格中将显示“作业已完成”。

## <a name="next-steps"></a>后续步骤

这将完成有关将 Windows Server 数据备份和还原到 Azure 的教程。 若要了解有关 Azure 备份的详细信息，请参阅备份加密虚拟机的 PowerShell 示例。

> [!div class="nextstepaction"]
> [备份加密 VM](./scripts/backup-powershell-sample-backup-encrypted-vm.md)

