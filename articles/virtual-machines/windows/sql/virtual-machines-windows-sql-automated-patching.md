---
title: SQL Server VM'ler için Otomatik Yama (Kaynak Yöneticisi) | Microsoft Dokümanlar
description: Kaynak Yöneticisi'ni kullanarak Azure'da çalışan SQL Server Sanal Makineler için Otomatik Yama özelliğini açıklar.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
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
ms.openlocfilehash: 292f3e8819f6f9f4b2989423814e02dfcfb4bfdb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127674"
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Azure Virtual Machines’de (Resource Manager) SQL Server için Otomatik Düzeltme Eki Uygulama
> [!div class="op_single_selector"]
> * [Kaynak Yöneticisi](virtual-machines-windows-sql-automated-patching.md)
> * [Klasik](../sqlclassic/virtual-machines-windows-classic-sql-automated-patching.md)

Otomatik Yama Uygulama, SQL Server çalıştıran Azure Sanal Makinesinde bir bakım penceresi oluşturur. Otomatik Güncelleştirmeler yalnızca bu bakım penceresi sırasında yüklenebilir. SQL Server için bu kısıtlama, sistem güncelleştirmelerinin ve ilişkili tüm yeniden başlatmaların veritabanı için mümkün olan en uygun zamanda yapılmasını sağlar. 

> [!IMPORTANT]
> Yalnızca **Önemli** veya **Kritik** olarak işaretlenmiş Windows ve SQL Server güncelleştirmeleri yüklenir. Hizmet paketleri ve **Önemli** veya **Kritik** olarak işaretlenmemiş kümülatif güncelleştirmeler gibi diğer SQL Server güncelleştirmeleri el ile yüklenmelidir. 

Otomatik Yama Uygulama [SQL Server IaaS Aracı Uzantısı](virtual-machines-windows-sql-server-agent-extension.md)'na bağımlıdır.

## <a name="prerequisites"></a>Ön koşullar
Otomatik Yama kullanmak için aşağıdaki ön koşulları göz önünde bulundurun:

**İşletim Sistemi**:

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

* Otomatik Yama'yı PowerShell ile yapılandırmayı planlıyorsanız [en son Azure PowerShell komutlarını yükleyin.](/powershell/azure/overview)

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]

> [!NOTE]
> Otomatik Yama, SQL Server IaaS Agent Extension'a dayanır. Geçerli SQL sanal makine galerisi görüntüleri varsayılan olarak bu uzantıyı ekleyin. Daha fazla bilgi için [SQL Server IaaS Aracı Uzantısı'na](virtual-machines-windows-sql-server-agent-extension.md)bakın.
> 
> 

## <a name="settings"></a>Ayarlar
Aşağıdaki tabloda Otomatik Yama için yapılandırılabilen seçenekler açıklanmaktadır. Gerçek yapılandırma adımları, Azure portalını veya Azure Windows PowerShell komutlarını kullanıp kullanmadığınıza bağlı olarak değişir.

| Ayar | Olası değerler | Açıklama |
| --- | --- | --- |
| **Otomatik Yama** |Etkinleştirme/Devre Dışı Bırakma (Devre Dışı) |Azure sanal makinesi için Otomatik Yama etkinleştirilir veya devre dışı kılabilir. |
| **Bakım zamanlaması** |Her gün, Pazartesi, Salı, Çarşamba, Perşembe, Cuma, Cumartesi, Pazar |Sanal makineniz için Windows, SQL Server ve Microsoft güncelleştirmelerini indirme ve yükleme çizelgesi. |
| **Bakım başlangıç saati** |0-24 |Sanal makineyi güncelleştirmek için yerel başlangıç zamanı. |
| **Bakım penceresi süresi** |30-180 |Güncelleştirmelerin karşıdan yüklenmesi ve yüklenmesi için izin verilen dakika sayısı. |
| **Yama Kategorisi** |Önemli | İndirmek ve yüklemek için Windows güncelleştirmeleri kategorisi.|

## <a name="configuration-in-the-portal"></a>Portalda Yapılandırma
Azure portalını, sağlama sırasında veya mevcut VM'ler için Otomatik Yama yapılandırmak için kullanabilirsiniz.

### <a name="new-vms"></a>Yeni VM'ler
Kaynak Yöneticisi dağıtım modelinde yeni bir SQL Server Virtual Machine oluştururken Otomatik Yama yapılandırmak için Azure portalını kullanın.

SQL **Server ayarları** sekmesinde, **Otomatik yama**altında **yapılandırmayı değiştir'i** seçin. Aşağıdaki Azure portalı ekran görüntüsü **SQL Otomatik Yama** bıçağını gösterir.

![Azure portalında SQL Otomatik Yama](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

Bağlam için, Azure'da bir SQL Server sanal makine sağlama konusunun [tamamına](virtual-machines-windows-portal-sql-server-provision.md)bakın.

### <a name="existing-vms"></a>Varolan VM'ler

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Mevcut SQL Server sanal makineleri için [SQL sanal makine kaynağınızı](virtual-machines-windows-sql-manage-portal.md#access-the-sql-virtual-machines-resource) açın ve **Ayarlar**altında **Yama'yı** seçin. 

![Mevcut VM'ler için SQL Otomatik Yama](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)


Bittiğinde, değişikliklerinizi kaydetmek için SQL **Server yapılandırma** bıçağının altındaki **Tamam** düğmesini tıklatın.

Otomatik Yama'yı ilk kez etkinleştiriyorsanız, Azure arka planda SQL Server IaaS Aracısını yapılandırır. Bu süre zarfında, Azure portalı Otomatik Yama yapılandırıldığından görünmeyebilir. Aracının yüklenmesi, yapılandırılması için birkaç dakika bekleyin. Bundan sonra Azure portalı yeni ayarları yansıtır.

## <a name="configuration-with-powershell"></a>PowerShell ile Yapılandırma
SQL VM'nizi kullandıktan sonra Otomatik Yama yapılandırmak için PowerShell'i kullanın.

Aşağıdaki örnekte, PowerShell varolan bir SQL Server VM üzerinde Otomatik Yama yapılandırmak için kullanılır. **New-AzVMSqlServerAutoPatchingConfig** komutu otomatik güncelleştirmeler için yeni bir bakım penceresi yapılandırır.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = New-AzVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"
s Set-AzVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

> [!IMPORTANT]
> Uzantı zaten yüklenmediyse, uzantıyükleme SQL Server hizmetini yeniden başlatır.

Bu örneğe dayanarak, aşağıdaki tabloda hedef Azure VM üzerindeki pratik etkisi açıklanmaktadır:

| Parametre | Etki |
| --- | --- |
| **Dayofweek** |Yamalar her Perşembe yüklenir. |
| **MaintenanceWindowStartingHour** |Güncellemeler saat 11:00'de başlayın. |
| **BakımWindowsDuration** |Yamalar 120 dakika içinde kurulmalıdır. Başlangıç saatine göre saat 13:00'e kadar tamamlanması gerekmektedir. |
| **Yama Kategorisi** |Bu parametre için mümkün olan tek ayar **önemlidir.** Bu önemli işaretlenmiş Windows güncelleştirmesi yükler; bu kategoride yer almayan herhangi bir SQL Server güncelleştirmesi yüklemez. |

SQL Server IaaS Aracısını yüklemek ve yapılandırmak birkaç dakika sürebilir.

Otomatik Yama devre dışı kalmak için, **New-AzVMSqlSqlAutoPatchingConfig**için **-Enable** parametresi olmadan aynı komut dosyasını çalıştırın. **-Enable** parametresinin yokluğu, özelliği devre dışı bırakabilmek için komutu bildirir.

## <a name="next-steps"></a>Sonraki adımlar
Mevcut diğer otomasyon görevleri hakkında daha fazla bilgi için [SQL Server IaaS Agent Extension'a](virtual-machines-windows-sql-server-agent-extension.md)bakın.

Azure VM'lerde SQL Server'ı çalıştırma hakkında daha fazla bilgi için [Azure Sanal Makinelere genel bakışta SQL Server'a](virtual-machines-windows-sql-server-iaas-overview.md)bakın.

