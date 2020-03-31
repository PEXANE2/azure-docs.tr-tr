---
title: SQL Server VM'ler için Otomatik Yama (Klasik) | Microsoft Dokümanlar
description: Klasik dağıtım modunu kullanarak Azure'da çalışan SQL Server Sanal Makineler için Otomatik Yama özelliğini açıklar.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 737b2f65-08b9-4f54-b867-e987730265a8
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/07/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: efc6d0c25c5186b391deb08ee0e41dcb8ae6edf0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75978078"
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a>Azure Sanal Makinelerde SQL Server için Otomatik Yama (Klasik)
> [!div class="op_single_selector"]
> * [Kaynak Yöneticisi](../sql/virtual-machines-windows-sql-automated-patching.md)
> * [Klasik](../classic/sql-automated-patching.md)
> 
> 

Otomatik Yama Uygulama, SQL Server çalıştıran Azure Sanal Makinesinde bir bakım penceresi oluşturur. Otomatik Güncelleştirmeler yalnızca bu bakım penceresi sırasında yüklenebilir. SQL Server için bu, sistem güncelleştirmelerinin ve ilişkili yeniden başlatmaların veritabanı için mümkün olan en iyi zamanda gerçekleşmesini sağlar. 

> [!IMPORTANT]
> Yalnızca **Önemli** olarak işaretlenmiş Windows güncelleştirmeleri yüklenir. Toplu Güncelleştirmeler gibi diğer SQL Server güncelleştirmelerinin el ile yüklenmesi gerekir. 

Otomatik Yama Uygulama [SQL Server IaaS Aracı Uzantısı](../classic/sql-server-agent-extension.md)'na bağımlıdır.

> [!IMPORTANT] 
> Azure'un kaynakları oluşturmak ve onlarla çalışmak için iki farklı dağıtım modeli vardır: [Kaynak Yöneticisi ve Klasik.](../../../azure-resource-manager/management/deployment-models.md) Bu makalede, Klasik dağıtım modeli kullanılarak kapsar. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Bu makalenin Kaynak Yöneticisi sürümünü görüntülemek [için Azure Sanal Makineler Kaynak Yöneticisi'nde SQL Server için Otomatik Yama bölümüne](../sql/virtual-machines-windows-sql-automated-patching.md)bakın.

## <a name="prerequisites"></a>Ön koşullar
Otomatik Yama kullanmak için aşağıdaki ön koşulları göz önünde bulundurun:

**İşletim Sistemi**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server sürümü**:

* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

**Azure PowerShell**:

* [En son Azure PowerShell komutlarını yükleyin.](/powershell/azure/overview)

**SQL Server IaaS Uzantısı**:

* [SQL Server IaaS Uzantısı'nı yükleyin.](../classic/sql-server-agent-extension.md)

## <a name="settings"></a>Ayarlar
Aşağıdaki tabloda Otomatik Yama için yapılandırılabilen seçenekler açıklanmaktadır. Klasik VM'ler için bu ayarları yapılandırmak için PowerShell'i kullanmanız gerekir.

| Ayar | Olası değerler | Açıklama |
| --- | --- | --- |
| **Otomatik Yama** |Etkinleştirme/Devre Dışı Bırakma (Devre Dışı) |Azure sanal makinesi için Otomatik Yama etkinleştirilir veya devre dışı kılabilir. |
| **Bakım zamanlaması** |Her gün, Pazartesi, Salı, Çarşamba, Perşembe, Cuma, Cumartesi, Pazar |Sanal makineniz için Windows, SQL Server ve Microsoft güncelleştirmelerini indirme ve yükleme çizelgesi. |
| **Bakım başlangıç saati** |0-24 |Sanal makineyi güncelleştirmek için yerel başlangıç zamanı. |
| **Bakım penceresi süresi** |30-180 |Güncelleştirmelerin karşıdan yüklenmesi ve yüklenmesi için izin verilen dakika sayısı. |
| **Yama Kategorisi** |Önemli |İndirmek ve yüklemek için güncelleştirmekategorisi. |

## <a name="configuration-with-powershell"></a>PowerShell ile Yapılandırma
Aşağıdaki örnekte, PowerShell varolan bir SQL Server VM üzerinde Otomatik Yama yapılandırmak için kullanılır. **Yeni-AzureVMSqlSqlAutoPatchingConfig** komutu otomatik güncelleştirmeler için yeni bir bakım penceresi yapılandırır.

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

Bu örneğe dayanarak, aşağıdaki tabloda hedef Azure VM üzerindeki pratik etkisi açıklanmaktadır:

| Parametre | Etki |
| --- | --- |
| **Dayofweek** |Yamalar her Perşembe yüklenir. |
| **MaintenanceWindowStartingHour** |Güncellemeler saat 11:00'de başlayın. |
| **BakımPenceresiSüresi** |Yamalar 120 dakika içinde kurulmalıdır. Başlangıç saatine göre saat 13:00'e kadar tamamlanması gerekmektedir. |
| **Yama Kategorisi** |Bu parametre için mümkün olan tek ayar "Önemli"dir. |

SQL Server IaaS Aracısını yüklemek ve yapılandırmak birkaç dakika sürebilir.

Otomatik Yama'yı devre dışı kılmış için, Yeni AzureVMSqlSqlAutoPatchingConfig'e -Etkinleştir parametresi olmadan aynı komut dosyasını çalıştırın. Yüklemede olduğu gibi, Otomatik Yama'yı devre dışı bırakın birkaç dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
Mevcut diğer otomasyon görevleri hakkında daha fazla bilgi için [SQL Server IaaS Agent Extension'a](../classic/sql-server-agent-extension.md)bakın.

Azure VM'lerde SQL Server'ı çalıştırma hakkında daha fazla bilgi için [Azure Sanal Makinelere genel bakışta SQL Server'a](../sql/virtual-machines-windows-sql-server-iaas-overview.md)bakın.

