---
title: Azure VM sürüm notları 'nda SQL Server | Microsoft Docs
description: Azure VM 'de SQL Server yeni özellikler ve iyileştirmeler hakkında bilgi edinin
services: virtual-machines-windows
author: MashaMSFT
ms.author: mathoma
manager: craigg
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 2/13/2019
ms.openlocfilehash: e7dea69b507117ba8a3765b5e9bc1fad46be4c53
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68444201"
---
# <a name="sql-server-on-azure-virtual-machine-release-notes"></a>Azure sanal makinesi sürüm notlarında SQL Server

Azure, içinde yerleşik SQL Server bir görüntü ile sanal makine dağıtmanıza olanak tanır. Bu makalede, [Azure sanal makinelerinde son SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/)sürümlerindeki yeni özellikler ve geliştirmeler özetlenmektedir. Makalede ayrıca, yayınla doğrudan ilgili olmayan ancak aynı zaman çerçevesinde yayınlanan önemli içerik güncelleştirmeleri de listelenir. Diğer Azure hizmetlerine yönelik iyileştirmeler için bkz. [hizmet güncelleştirmeleri](https://azure.microsoft.com/updates)


## <a name="june-2019"></a>Haziran 2019

### <a name="service-improvements"></a>Hizmet geliştirmeleri

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **Yeni SQL IaaS yükleme modları** | SQL Server hizmeti yeniden başlatmanıza engel olmak için SQL IaaS uzantısını [hafif modda](virtual-machines-windows-sql-server-agent-extension.md) yüklemek artık mümkündür.  |
| **SQL Server sürümü değişikliği** | Artık SQL Server VM için [sürüm özelliğini](virtual-machines-windows-sql-change-edition.md) değiştirebilirsiniz. |
| **SQL VM RP değişiklikleri** | Yeni SQL IaaS modlarını kullanarak, [SQL Server VM SQL VM kaynak sağlayıcısı](virtual-machines-windows-sql-register-with-resource-provider.md#register-with-sql-vm-resource-provider) -hatta [Windows 2008 görüntüleri](virtual-machines-windows-sql-register-with-resource-provider.md#register-sql-server-2008r2-on-windows-server-2008-vms) ile kaydedebilirsiniz. |
| **AHUB kullanan KLG görüntüleri** | Market 'ten dağıtılan KLG görüntüleri artık [Lisans türlerini ' PAYG ' olarak](virtual-machines-windows-sql-ahb.md#remarks)değiştirebilir.| 
| &nbsp; | &nbsp; |


## <a name="may-2019"></a>Mayıs 2019

### <a name="service-improvements"></a>Hizmet geliştirmeleri

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **Azure portal yeni SQL VM yönetimi** | Artık Azure portal SQL Server VM yönetmenin yeni bir yolu vardır. Daha fazla bilgi için [Azure portal SQL Server VM yönetme](virtual-machines-windows-sql-manage-portal.md)bölümüne bakın.  |
| &nbsp; | &nbsp; |

### <a name="documentation-improvements"></a>Belge geliştirmeleri

| Belgeler | Ayrıntılar |
| --- | --- |
| **Yeni SQL VM Portal yönetimi** | Yeni SQL VM yönetim portalı deneyimine bir düzine makale güncelleştirildi. | 
| &nbsp; | &nbsp; |




## <a name="april-2019"></a>2019 Nisan

### <a name="service-improvements"></a>Hizmet geliştirmeleri

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **SQL Server 2008/2008R2 desteğini uzat** | SQL Server 2008 ve SQL Server 2008 R2 desteğini bir Azure VM 'sine *geçiş yaparak* [genişletin](virtual-machines-windows-sql-server-2008-eos-extend-support.md) . | 
| &nbsp; | &nbsp; |


## <a name="march-2019"></a>Mart 2019

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **Özel görüntü desteklenebilirliği** | Artık, [Esnek lisanslamanın](virtual-machines-windows-sql-ahb.md)sınırlı işlevlerini sunan özel işletim SISTEMI ve SQL görüntülerine [SQL IaaS uzantısını](virtual-machines-windows-sql-server-agent-extension.md#installation) yükleyebilirsiniz. Özel görüntünüzü SQL kaynak sağlayıcısına kaydederken, lisans türünü ' AHUB ' olarak belirtin, aksi takdirde kayıt başarısız olur. | 
| **Adlandırılmış örnek desteklenebilirliği** | Artık varsayılan örnek doğru şekilde kaldırılmışsa [SQL IaaS uzantısını](virtual-machines-windows-sql-server-agent-extension.md#installation) adlandırılmış bir örnekle kullanabilirsiniz. | 
| **Portal geliştirmesi** | SQL Server VM dağıtmaya yönelik Azure portal deneyim, kullanılabilirliği iyileştirmeye yönelik olarak yeniden belirlenmiştir. Daha fazla bilgi için bkz. kısa [Başlangıç](quickstart-sql-vm-create-portal.md) ve daha fazla kapsamlı [nasıl yapılır](virtual-machines-windows-portal-sql-server-provision.md) Kılavuzu SQL Server VM dağıtma.|
| &nbsp; | &nbsp; |


## <a name="february-2019"></a>Şubat 2019

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **Portal geliştirme** | Bir SQL Server VM için lisans modelini, Kullandıkça Öde 'den kendi lisansını getir ' e kadar değiştirmek mümkündür [Azure Portal](virtual-machines-windows-sql-ahb.md#change-license-for-vms-already-registered-with-resource-provider)|
|**Azure SQL VM CLı ile AG dağıtımı basitleştirmesi** | Artık bir kullanılabilirlik grubunu Azure 'daki bir SQL Server VM dağıtmak her zamankinden daha kolay. [Azure SQL VM CLI](/cli/azure/sql/vm?view=azure-cli-2018-03-01-hybrid) , komut satırından ve kayıt zamanında wsfc, ILB ve AG dinleyicisi oluşturmanızı sağlar! Daha fazla bilgi için bkz. Azure [SQL VM CLI kullanarak Azure VM 'de SQL Server Için Always on kullanılabilirlik grubunu yapılandırma](virtual-machines-windows-sql-availability-group-cli.md). | 
| &nbsp; | &nbsp; |


## <a name="december-2018"></a>Aralık 2018

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **Yeni SQL cluster grubu kaynak sağlayıcısı** | Windows Yük devretme kümesinin meta verilerini tanımlayan yeni bir kaynak sağlayıcısı (Microsoft. SqlVirtualMachine/SqlVirtualMachineGroups). Windows Yük devretme Küme hizmeti önyükleme SQL Server VM *Sqlvirtualmachinegroups* 'a katılma ve VM 'yi kümeyle birleştirme.  |
|**Azure hızlı başlangıç şablonlarıyla bir kullanılabilirlik grubu dağıtımı ayarlamayı otomatikleştirme** |Artık Windows Yük devretme kümesi oluşturmak, bu VM 'ye SQL Server VM 'ye katmak, dinleyiciyi oluşturmak ve Iç Load Balancer iki Azure hızlı başlangıç şablonlarıyla yapılandırmak mümkündür. Daha fazla bilgi için bkz. Azure [hızlı başlangıç şablonunu kullanarak Azure VM 'de SQL Server Için Always on kullanılabilirlik grubunu yapılandırma](virtual-machines-windows-sql-availability-group-quickstart-template.md). | 
| **Otomatik SQL VM kaynak sağlayıcısı kaydı** | Bu aydan sonra dağıtılan SQL Server VM 'Ler yeni SQL Server kaynak sağlayıcısına otomatik olarak kaydedilir. Bu aydan önce dağıtılan SQL Server sanal makinelerin yine de el ile kaydedilmesi gerekir. Daha fazla bilgi için bkz. [SQL VM kaynak sağlayıcısı ile var olan SQL VM 'Yi kaydetme](virtual-machines-windows-sql-register-with-resource-provider.md).|
| &nbsp; | &nbsp; |


## <a name="november-2018"></a>2018 Kasım

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **Yeni SQL VM kaynak sağlayıcısı** |  SQL Server VM daha iyi yönetim sağlayan SQL Server VM 'Ler (Microsoft. SqlVirtualMachine) için yeni bir kaynak sağlayıcısı. VM 'nizi kaydetme hakkında daha fazla bilgi için bkz. [var olan SQL VM 'yi yeni kaynak sağlayıcısına kaydetme](virtual-machines-windows-sql-register-with-resource-provider.md). |
|**Lisans modelini Değiştir** | Artık Azure CLı veya PowerShell kullanarak SQL sanal makinenizin kullanım başına ödeme ve kendi lisans modeli arasında geçiş yapabilirsiniz. Daha fazla bilgi için bkz. [SQL VM için lisans modelini değiştirme](virtual-machines-windows-sql-ahb.md). | 
| &nbsp; | &nbsp; |


## <a name="additional-resources"></a>Ek kaynaklar

**Windows VM 'leri**:

* [WINDOWS VM 'de SQL Server genel bakış](virtual-machines-windows-sql-server-iaas-overview.md).
* [SQL Server Windows VM sağlama](virtual-machines-windows-portal-sql-server-provision.md)
* [Bir veritabanını Azure VM 'de SQL Server geçirme](virtual-machines-windows-migrate-sql.md)
* [Azure sanal makinelerinde SQL Server için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](virtual-machines-windows-sql-high-availability-dr.md)
* [Azure Sanal Makineler’de SQL Server için performansa yönelik en iyi uygulamalar](virtual-machines-windows-sql-performance.md)
* [Azure Sanal Makineler'de SQL Server için Uygulama Desenleri ve Geliştirme Stratejileri](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Linux VM 'leri**:

* [Linux VM 'de SQL Server genel bakış](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [SQL Server Linux sanal makinesi sağlama](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [SSS (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [Linux üzerinde SQL Server belgeleri](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
