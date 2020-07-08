---
title: SQL Server VM 'Ler için otomatik düzeltme eki uygulama (Kaynak Yöneticisi) | Microsoft Docs
description: Bu makalede Kaynak Yöneticisi kullanarak Azure üzerinde çalışan SQL Server sanal makineler için otomatik düzeltme eki uygulama özelliği açıklanmaktadır.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
editor: ''
tags: azure-resource-manager
ms.assetid: 58232e92-318f-456b-8f0a-2201a541e08d
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/07/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: ed973b6ea5bbcd2b23e30d381e909ef2ab03b917
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85921668"
---
# <a name="automated-patching-for-sql-server-on-azure-virtual-machines-resource-manager"></a>Azure sanal makinelerinde SQL Server için otomatik düzeltme eki uygulama (Kaynak Yöneticisi)
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

> [!div class="op_single_selector"]
> * [Resource Manager](automated-patching.md)
> * [Klasik](../../../virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-sql-automated-patching.md)

Otomatik düzeltme eki uygulama SQL Server çalıştıran bir Azure sanal makinesine yönelik bir bakım penceresi oluşturur. Otomatik Güncelleştirmeler yalnızca bu bakım penceresi sırasında yüklenebilir. SQL Server için bu kısıtlama, sistem güncelleştirmelerinin ve ilişkili tüm yeniden başlatmaların veritabanı için mümkün olan en uygun zamanda yapılmasını sağlar. 

> [!IMPORTANT]
> Yalnızca Windows ve SQL Server, **önemli** veya **kritik** olarak işaretlenen güncelleştirmeler yüklenir. **Önemli** veya **kritik**olarak işaretlenmemiş hizmet paketleri ve toplu güncelleştirmeler gibi diğer SQL Server güncelleştirmelerin el ile yüklenmesi gerekir. 

Otomatik düzeltme eki uygulama [SQL Server hizmet olarak altyapı (IaaS) aracı uzantısına](sql-server-iaas-agent-extension-automate-management.md)bağlıdır.

## <a name="prerequisites"></a>Ön koşullar
Otomatik düzeltme eki kullanmak için aşağıdaki önkoşulları göz önünde bulundurun:

**İşletim sistemi**:

* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server sürümü**:

* SQL Server 2008 R2
* SQL Server 2012
* SQL Server 2014
* SQL Server 2016
* SQL Server 2017

**Azure PowerShell**:

* PowerShell ile otomatik düzeltme eki uygulamayı yapılandırmayı planlıyorsanız [en son Azure PowerShell komutlarını yükleyebilirsiniz](/powershell/azure/overview) .

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]

> [!NOTE]
> Otomatik düzeltme eki uygulama SQL Server IaaS Aracısı uzantısına dayanır. Geçerli SQL sanal makine galeri görüntüleri varsayılan olarak bu uzantıyı ekler. Daha fazla bilgi için bkz. [IaaS Aracısı uzantısı SQL Server](sql-server-iaas-agent-extension-automate-management.md).
> 
> 

## <a name="settings"></a>Ayarlar
Aşağıdaki tabloda otomatik düzeltme eki uygulama için yapılandırılabilecek seçenekler açıklanmaktadır. Gerçek yapılandırma adımları Azure portal veya Azure Windows PowerShell komutlarını kullanıp kullanmayacağınızı bağlı olarak değişir.

| Ayar | Olası değerler | Açıklama |
| --- | --- | --- |
| **Otomatik Düzeltme Eki Uygulama** |Etkinleştir/devre dışı bırak (devre dışı) |Bir Azure sanal makinesi için otomatik düzeltme eki uygulamayı etkinleştirilir veya devre dışı bırakır. |
| **Bakım zamanlaması** |Günlük, Pazartesi, Salı, Çarşamba, Perşembe, Cuma, Cumartesi, Pazar |Sanal makineniz için Windows, SQL Server ve Microsoft güncelleştirmelerini indirme ve yükleme zamanlaması. |
| **Bakım başlangıç saati** |0-24 |Sanal makineyi güncelleştirmek için yerel başlangıç saati. |
| **Bakım penceresi süresi** |30-180 |Güncelleştirmelerin indirilmesini ve yüklenmesini tamamlamaya izin verilen dakika sayısı. |
| **Düzeltme Eki kategorisi** |Önemli | İndirilecek ve yüklenecek Windows güncelleştirmelerinin kategorisi.|

## <a name="configure-in-the-azure-portal"></a>Azure portal yapılandırma
Azure portal, sağlama sırasında veya var olan VM 'Lerde otomatik düzeltme eki uygulamayı yapılandırmak için kullanabilirsiniz.

### <a name="new-vms"></a>Yeni VM 'Ler
Kaynak Yöneticisi dağıtım modelinde yeni bir SQL Server sanal makine oluşturduğunuzda otomatik düzeltme eki yapılandırmak için Azure portal kullanın.

**SQL Server ayarları** sekmesinde **Otomatik Düzeltme Eki**altında **yapılandırmayı Değiştir** ' i seçin. Aşağıdaki Azure portal ekran görüntüsünde **SQL otomatik düzeltme eki uygulama** dikey penceresi gösterilmektedir.

![Azure portal SQL otomatik düzeltme eki uygulama](./media/automated-patching/azure-sql-arm-patching.png)

Daha fazla bilgi için bkz. [Azure 'da SQL Server sanal makinesi sağlama](create-sql-vm-portal.md).

### <a name="existing-vms"></a>Mevcut VM 'Ler

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Mevcut SQL Server sanal makineler için [SQL sanal makineler](manage-sql-vm-portal.md#access-the-sql-virtual-machines-resource) kaynağınızı açın ve **Ayarlar**altında **Düzeltme Eki** ' ni seçin. 

![Mevcut VM 'Ler için SQL otomatik düzeltme eki uygulama](./media/automated-patching/azure-sql-rm-patching-existing-vms.png)


İşiniz bittiğinde, değişikliklerinizi kaydetmek için **SQL Server yapılandırma** dikey penceresinin altındaki **Tamam** düğmesine tıklayın.

Otomatik Düzeltme Eki uygulamayı ilk kez etkinleştirirseniz Azure SQL Server IaaS aracısını arka planda yapılandırır. Bu süre boyunca, Azure portal otomatik düzeltme eki uygulama 'nın yapılandırıldığını gösteremeyebilir. Aracının yüklenmesi ve yapılandırılması için birkaç dakika bekleyin. Azure portal sonra yeni ayarları yansıtır.

## <a name="configure-with-powershell"></a>PowerShell ile yapılandırma
SQL VM 'nizi sağlamaktan sonra otomatik düzeltme eki yapılandırmak için PowerShell 'i kullanın.

Aşağıdaki örnekte, PowerShell, mevcut bir SQL Server VM otomatik düzeltme eki yapılandırmak için kullanılır. **New-Azvmsqlserverotomatik Patchingconfig** komutu otomatik güncelleştirmeler için yeni bir bakım penceresi yapılandırır.

```azurepowershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"
$aps = New-AzVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"
s
Set-AzVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname
```

> [!IMPORTANT]
> Uzantı zaten yüklü değilse, yükleme yeniden başlatılır SQL Server.

Bu örneğe bağlı olarak, aşağıdaki tabloda hedef Azure VM üzerinde pratik etki açıklanmaktadır:

| Parametre | Efekt |
| --- | --- |
| **DayOfWeek** |Her Perşembe için düzeltme eki yüklendi. |
| **MaintenanceWindowStartingHour** |Güncelleştirmeler 11:00:00:00 ile başlatılır. |
| **MaintenanceWindowsDuration** |Düzeltme eklerinin 120 dakika içinde yüklü olması gerekir. Başlangıç zamanına bağlı olarak, 1:00pm ile tamamlanmaları gerekir. |
| **PatchCategory** |Bu parametre için olası tek ayar **önemlidir**. Bu, önemli olarak işaretlenmiş Windows Update 'i yüklüyor; Bu kategoriye dahil olmayan SQL Server güncelleştirmelerinin hiçbirini yüklemez. |

SQL Server IaaS aracısının yüklenmesi ve yapılandırılması birkaç dakika sürebilir.

Otomatik Düzeltme Eki uygulamayı devre dışı bırakmak için, **New-Azvmsqlserverotomatikpatchingconfig**için **-Enable** parametresi olmadan aynı betiği çalıştırın. **-Enable** parametresinin yokluğu, özelliği devre dışı bırakma komutuna işaret eder.

## <a name="next-steps"></a>Sonraki adımlar
Diğer kullanılabilir otomasyon görevleri hakkında daha fazla bilgi için bkz. [IaaS Aracısı uzantısı SQL Server](sql-server-iaas-agent-extension-automate-management.md).

Azure VM 'lerinde SQL Server çalıştırma hakkında daha fazla bilgi için bkz. [Azure sanal makinelerine genel bakış SQL Server](sql-server-on-azure-vm-iaas-what-is-overview.md).

