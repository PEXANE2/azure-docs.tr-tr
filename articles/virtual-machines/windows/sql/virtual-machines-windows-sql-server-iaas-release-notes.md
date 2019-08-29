---
title: Azure sanal makinelerinde SQL Server için sürüm notları | Microsoft Docs
description: Azure VM 'de SQL Server yönelik yeni özellikler ve iyileştirmeler hakkında bilgi edinin
services: virtual-machines-windows
author: MashaMSFT
ms.author: mathoma
manager: craigg
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/01/2019
ms.openlocfilehash: e58b01c6a01a22ef2dd45dc49d741f0287d603a0
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70100368"
---
# <a name="release-notes-for-sql-server-on-azure-virtual-machines"></a>Azure sanal makinelerinde SQL Server için sürüm notları

Azure, içinde yerleşik SQL Server bir görüntü ile sanal makine (VM) dağıtmanıza olanak tanır. Bu makalede, [Azure sanal makinelerinde son SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/)sürümlerindeki yeni özellikler ve geliştirmeler özetlenmektedir. Makalede ayrıca, yayınla doğrudan ilgili olmayan ancak aynı zaman çerçevesinde yayınlanan önemli içerik güncelleştirmeleri de listelenir. Diğer Azure hizmetlerine yönelik iyileştirmeler için bkz. [hizmet güncelleştirmeleri](https://azure.microsoft.com/updates).

## <a name="july-2019"></a>2019 Temmuz

### <a name="documentation-improvements"></a>Belge geliştirmeleri

| Belgeler | Ayrıntılar |
| --- | --- |
| **SQL VM 'yi farklı bir bölgeye taşıma** | [SQL Server VM bir bölgeden diğerine geçirmek](virtual-machines-windows-sql-move-different-region.md)için Azure Site Recovery kullanın. |
| &nbsp; | &nbsp; |

## <a name="june-2019"></a>Haziran 2019

### <a name="service-improvements"></a>Hizmet geliştirmeleri

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **Yeni SQL IaaS yükleme modları** | SQL Server hizmetinin yeniden başlatılmasını önlemek için SQL Server IaaS uzantısını [hafif modda](virtual-machines-windows-sql-server-agent-extension.md) yüklemek mümkündür.  |
| **SQL Server sürümü değişikliği** | Artık SQL Server VM için [sürüm özelliğini](virtual-machines-windows-sql-change-edition.md) değiştirebilirsiniz. |
| **SQL VM kaynak sağlayıcısında yapılan değişiklikler** | [SQL VM kaynak sağlayıcısı ile SQL Server VM](virtual-machines-windows-sql-register-with-resource-provider.md) yeni SQL IaaS modlarını kullanarak kaydedebilirsiniz. Bu özellik [Windows 2008 görüntülerini](virtual-machines-windows-sql-register-with-resource-provider.md#register-sql-server-2008-or-2008-r2-on-windows-server-2008-vms)içerir.|
| **Azure Hibrit Avantajı kullanarak kendi lisans görüntülerini getirin** | Azure Marketi 'nden dağıtılan kendi lisans görüntülerini getir, şimdi de [Lisans türlerini Kullandıkça Öde olarak](virtual-machines-windows-sql-ahb.md#remarks)değiştirebilir.| 
| &nbsp; | &nbsp; |


## <a name="may-2019"></a>Mayıs 2019

### <a name="service-improvements"></a>Hizmet geliştirmeleri

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **Azure portal yeni SQL Server VM yönetimi** | Artık Azure portal SQL Server VM yönetmenin bir yolu vardır. Daha fazla bilgi için bkz. [Azure portal SQL Server VM 'Leri yönetme](virtual-machines-windows-sql-manage-portal.md).  | 
| &nbsp; | &nbsp; |

### <a name="documentation-improvements"></a>Belge geliştirmeleri

| Belgeler | Ayrıntılar |
| --- | --- |
| **Yeni SQL Server VM yönetim portalı** | Yeni SQL Server VM yönetim portalı deneyimine bir düzine makale güncelleştirildi. | 
| &nbsp; | &nbsp; |




## <a name="april-2019"></a>2019 Nisan

### <a name="service-improvements"></a>Hizmet geliştirmeleri

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **SQL Server 2008/2008 R2 için genişletilmiş destek** | SQL Server 2008 ve SQL Server 2008 R2 desteğini bir Azure VM 'ye *olduğu gibi* geçirerek [genişletin](virtual-machines-windows-sql-server-2008-eos-extend-support.md) . | 
| &nbsp; | &nbsp; |


## <a name="march-2019"></a>Mart 2019

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **Özel görüntü desteklenebilirliği** | Artık [SQL Server IaaS uzantısını](virtual-machines-windows-sql-server-agent-extension.md#installation) özel işletim SISTEMI ve SQL yansımalarına yükleyebilirsiniz ve bu da [Esnek lisansın](virtual-machines-windows-sql-ahb.md)sınırlı işlevlerini sunar. Özel görüntünüzü SQL kaynak sağlayıcısı ile kaydederken, lisans türünü "AHUB" olarak belirtin. Aksi takdirde, kayıt başarısız olur. | 
| **Adlandırılmış örnek desteklenebilirliği** | Varsayılan örnek düzgün şekilde kaldırılmışsa, artık [SQL Server IaaS uzantısını](virtual-machines-windows-sql-server-agent-extension.md#installation) adlandırılmış bir örnekle kullanabilirsiniz. | 
| **Portal geliştirmesi** | SQL Server VM dağıtmaya yönelik Azure portal deneyim, kullanılabilirliği iyileştirmeye yönelik olarak yeniden belirlenmiştir. Daha fazla bilgi için bkz. kısa [Başlangıç](quickstart-sql-vm-create-portal.md) ve daha kapsamlı [nasıl yapılır Kılavuzu](virtual-machines-windows-portal-sql-server-provision.md) SQL Server VM dağıtma.|
| &nbsp; | &nbsp; |


## <a name="february-2019"></a>Şubat 2019

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **Portal geliştirme** | Bir SQL Server VM için lisans modelini, [Azure Portal](virtual-machines-windows-sql-ahb.md#change-the-license-for-vms-already-registered-with-the-resource-provider)kullanarak kendi lisansınıza sahip olmak üzere Kullandıkça Öde için değiştirmek mümkündür.|
|**Azure SQL Server VM CLı ile kullanılabilirlik grubu dağıtımının basitleştirmesi** | Artık bir kullanılabilirlik grubunu Azure 'da bir SQL Server VM dağıtmaya hiç zamankinden daha kolay. Windows Yük devretme kümesi, iç yük dengeleyici ve kullanılabilirlik grubu dinleyicilerini komut satırından oluşturmak için [Azure CLI](/cli/azure/sql/vm?view=azure-cli-2018-03-01-hybrid) 'yı kullanabilirsiniz. Daha fazla bilgi için bkz. Azure [SQL Server VM CLI kullanarak bir Azure VM 'de SQL Server Için her zaman açık kullanılabilirlik grubu yapılandırma](virtual-machines-windows-sql-availability-group-cli.md). | 
| &nbsp; | &nbsp; |


## <a name="december-2018"></a>Aralık 2018

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **SQL Server kümesi için yeni kaynak sağlayıcısı** | Yeni bir kaynak sağlayıcısı (Microsoft. SqlVirtualMachine/SqlVirtualMachineGroups), Windows Yük devretme kümesinin meta verilerini tanımlar. Windows Server yük devretme kümesi (WSFC) hizmeti önyükleme ve VM 'yi kümeyle birleştiren *Sqlvirtualmachinegroups* 'a katılma SQL Server VM.  |
|**Azure hızlı başlangıç şablonları ile bir kullanılabilirlik grubu dağıtımının otomatik kurulumu** |Windows Yük devretme kümesi oluşturmak, bu kümeye SQL Server VM 'Leri birleştirmek, dinleyiciyi oluşturmak ve iç yük dengeleyiciyi iki Azure hızlı başlangıç şablonu ile yapılandırmak mümkündür. Daha fazla bilgi için bkz. Azure [Hızlı Başlangıç şablonlarını kullanarak bir Azure VM 'de SQL Server Için her zaman açık kullanılabilirlik grubu yapılandırma](virtual-machines-windows-sql-availability-group-quickstart-template.md). | 
| **SQL VM kaynak sağlayıcısına otomatik kayıt** | Bu aydan sonra dağıtılan SQL Server VM 'Ler yeni SQL Server kaynak sağlayıcısına otomatik olarak kaydedilir. Bu aya göre dağıtılan SQL Server sanal makinelerin yine de el ile kaydedilmesi gerekir. Daha fazla bilgi için bkz. [Azure 'da SQL Server sanal MAKINESINI SQL VM kaynak sağlayıcısıyla kaydetme](virtual-machines-windows-sql-register-with-resource-provider.md).|
| &nbsp; | &nbsp; |


## <a name="november-2018"></a>2018 Kasım

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **Yeni SQL VM kaynak sağlayıcısı** |  Yeni bir kaynak sağlayıcısı (Microsoft. SqlVirtualMachine) SQL Server sanal makinelerinizin daha iyi yönetilmesini sağlar. VM 'lerinizi kaydetme hakkında daha fazla bilgi için bkz. [Azure 'da SQL Server sanal MAKINESINI SQL VM kaynak sağlayıcısıyla kaydetme](virtual-machines-windows-sql-register-with-resource-provider.md). |
|**Lisans modelini Değiştir** | Artık Azure CLı veya PowerShell kullanarak SQL Server VM için, kullanım başına ödeme ve kendi lisans modelleriniz arasında geçiş yapabilirsiniz. Daha fazla bilgi için bkz. [Azure 'da SQL Server sanal makinesi için lisans modelini değiştirme](virtual-machines-windows-sql-ahb.md). | 
| &nbsp; | &nbsp; |


## <a name="additional-resources"></a>Ek kaynaklar

**Windows VM 'leri**:

* [Windows VM 'de SQL Server genel bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server Windows VM sağlama](virtual-machines-windows-portal-sql-server-provision.md)
* [Bir veritabanını Azure VM 'de SQL Server geçirme](virtual-machines-windows-migrate-sql.md)
* [Azure sanal makinelerinde SQL Server için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](virtual-machines-windows-sql-high-availability-dr.md)
* [Azure sanal makinelerinde SQL Server için en iyi performans uygulamaları](virtual-machines-windows-sql-performance.md)
* [Azure sanal makinelerinde SQL Server için uygulama desenleri ve geliştirme stratejileri](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Linux VM 'leri**:

* [Linux VM 'de SQL Server genel bakış](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [SQL Server Linux sanal makinesi sağlama](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [SSS (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [Linux üzerinde SQL Server belgeleri](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
