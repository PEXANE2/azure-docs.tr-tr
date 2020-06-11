---
title: Azure sanal makinelerinde SQL Server için belge değişiklikleri | Microsoft Docs
description: Azure sanal makinelerinde SQL Server yönelik yeni özellikler ve iyileştirmeler hakkında bilgi edinin.
services: virtual-machines-windows
author: MashaMSFT
ms.author: mathoma
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/06/2020
ms.openlocfilehash: 46bde8a075beccc0f9f6462823acfc66361caf05
ms.sourcegitcommit: eeba08c8eaa1d724635dcf3a5e931993c848c633
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84669078"
---
# <a name="documentation-changes-for-sql-server-on-azure-virtual-machines"></a>Azure sanal makinelerinde SQL Server için belge değişiklikleri
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Azure, içinde yerleşik SQL Server bir görüntü ile sanal makine (VM) dağıtmanıza olanak tanır. Bu makalede, [Azure sanal makinelerinde son SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/)sürümlerindeki yeni özellikler ve geliştirmelerle ilişkili belge değişiklikleri özetlenmektedir. 


## <a name="january-2020"></a>Ocak 2020

| Değişiklikler | Ayrıntılar |
| --- | --- |
| **Azure Kamu desteği** | Artık [Azure Kamu](https://azure.microsoft.com/global-infrastructure/government/) bulutunda barındırılan sanal MAKINELER IÇIN SQL VM kaynak sağlayıcısı ile SQL Server sanal makineleri kaydetmek mümkündür. | 
| &nbsp; | &nbsp; |

## <a name="2019"></a>2019

|Değişiklikler | Ayrıntılar |
 --- | --- |
| **Azure 'da ücretsiz DR çoğaltması** | [Yazılım güvencesi](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?rtc=1&activetab=software-assurance-default-pivot:primaryr3)'niz varsa şirket içi SQL Server Örneğiniz için Azure 'da olağanüstü durum kurtarma için [ücretsiz bir pasif örnek](business-continuity-high-availability-disaster-recovery-hadr-overview.md#free-dr-replica-in-azure) barındırabilirsiniz. | 
| **Toplu kaynak sağlayıcısı kaydı** | Artık SQL Server sanal makinelerini kaynak sağlayıcısına [toplu olarak kaydedebilirsiniz](sql-vm-resource-provider-bulk-register.md) . | 
|**Performans için iyileştirilmiş depolama yapılandırması** | Artık yeni bir SQL Server VM oluştururken [depolama yapılandırmanızı tamamen özelleştirebilirsiniz](storage-configuration.md#new-vms) . |
|**FCı için Premium dosya paylaşma** | Artık [depolama alanları doğrudan](failover-cluster-instance-storage-spaces-direct-manually-configure.md)orijinal yöntemi yerine [Premium dosya paylaşma](failover-cluster-instance-premium-file-share-manually-configure.md) kullanarak bir yük devretme kümesi örneği oluşturabilirsiniz. 
| **Azure Ayrılmış Konak** | SQL Server VM [Azure adanmış bir konakta](dedicated-host.md)çalıştırabilirsiniz. | 
| **SQL Server VM farklı bir bölgeye taşı** | [SQL Server VM bir bölgeden diğerine geçirmek](move-sql-vm-different-region.md)için Azure Site Recovery kullanın. |
|  **Yeni SQL IaaS yükleme modları** | SQL Server hizmetinin yeniden başlatılmasını önlemek için SQL Server IaaS uzantısını [hafif modda](sql-server-iaas-agent-extension-automate-management.md) yüklemek mümkündür.  |
| **SQL Server sürümü değişikliği** | Artık SQL Server VM için [sürüm özelliğini](change-sql-server-edition.md) değiştirebilirsiniz. |
| **SQL VM kaynak sağlayıcısında yapılan değişiklikler** | [SQL VM kaynak sağlayıcısı ile SQL Server VM](sql-vm-resource-provider-register.md) yeni SQL IaaS modlarını kullanarak kaydedebilirsiniz. Bu özellik [Windows Server 2008](sql-vm-resource-provider-register.md#management-modes) görüntülerini içerir.|
| **Azure Hibrit Avantajı kullanarak kendi lisans görüntülerini getirin** | Azure Marketi 'nden dağıtılan kendi lisans görüntülerini getir, şimdi de [Lisans türlerini Kullandıkça Öde olarak](licensing-model-azure-hybrid-benefit-ahb-change.md#remarks)değiştirebilir.| 
| **Azure portal yeni SQL Server VM yönetimi** | Artık Azure portal SQL Server VM yönetmenin bir yolu vardır. Daha fazla bilgi için bkz. [Azure portal SQL Server VM 'Leri yönetme](manage-sql-vm-portal.md).  | 
| **SQL Server 2008/2008 R2 için genişletilmiş destek** | SQL Server 2008 ve SQL Server 2008 R2 desteğini bir Azure VM 'ye *olduğu gibi* geçirerek [genişletin](sql-server-2008-extend-end-of-support.md) . | 
| **Özel görüntü desteklenebilirliği** | Artık [SQL Server IaaS uzantısını](sql-server-iaas-agent-extension-automate-management.md#installation) özel işletim sistemi ve SQL Server görüntülerine yükleyebilirsiniz ve bu da [Esnek lisansın](licensing-model-azure-hybrid-benefit-ahb-change.md)sınırlı işlevlerini sunar. Özel görüntünüzü SQL VM kaynak sağlayıcısı ile kaydederken, lisans türünü "AHUB" olarak belirtin. Aksi takdirde, kayıt başarısız olur. | 
| **Adlandırılmış örnek desteklenebilirliği** | Varsayılan örnek düzgün şekilde kaldırılmışsa, artık [SQL Server IaaS uzantısını](sql-server-iaas-agent-extension-automate-management.md#installation) adlandırılmış bir örnekle kullanabilirsiniz. | 
| **Portal geliştirmesi** | SQL Server VM dağıtmaya yönelik Azure portal deneyim, kullanılabilirliği iyileştirmeye yönelik olarak yeniden belirlenmiştir. Daha fazla bilgi için bkz. kısa [Başlangıç](sql-vm-create-portal-quickstart.md) ve daha kapsamlı [nasıl yapılır Kılavuzu](create-sql-vm-portal.md) SQL Server VM dağıtma.|
| **Portal geliştirme** | Bir SQL Server VM için lisans modelini, [Azure Portal](licensing-model-azure-hybrid-benefit-ahb-change.md#vms-already-registered-with-the-resource-provider)kullanarak kendi lisansınıza sahip olmak üzere Kullandıkça Öde için değiştirmek mümkündür.|
| **Azure SQL Server VM CLı ile kullanılabilirlik grubu dağıtımının basitleştirmesi** | Artık bir kullanılabilirlik grubunu Azure 'da bir SQL Server VM dağıtmaya hiç zamankinden daha kolay. Windows Yük devretme kümesi, iç yük dengeleyici ve kullanılabilirlik grubu dinleyicilerini komut satırından oluşturmak için [Azure CLI](/cli/azure/sql/vm?view=azure-cli-2018-03-01-hybrid) 'yı kullanabilirsiniz. Daha fazla bilgi için bkz. Azure [SQL Server VM CLI kullanarak bir Azure VM 'de SQL Server Için her zaman açık kullanılabilirlik grubu yapılandırma](availability-group-az-cli-configure.md). | 
| &nbsp; | &nbsp; |

## <a name="2018"></a>2018 

 Değişiklikler | Ayrıntılar |
| --- | --- |
|  **SQL Server kümesi için yeni kaynak sağlayıcısı** | Yeni bir kaynak sağlayıcısı (Microsoft. SqlVirtualMachine/SqlVirtualMachineGroups), Windows Yük devretme kümesinin meta verilerini tanımlar. Windows Server yük devretme kümesi (WSFC) hizmeti önyükleme ve VM 'yi kümeyle birleştiren *Sqlvirtualmachinegroups* 'a katılma SQL Server VM.  |
| **Azure hızlı başlangıç şablonları ile bir kullanılabilirlik grubu dağıtımının otomatik kurulumu** |Windows Yük devretme kümesi oluşturmak, bu kümeye SQL Server VM 'Leri birleştirmek, dinleyiciyi oluşturmak ve iç yük dengeleyiciyi iki Azure hızlı başlangıç şablonu ile yapılandırmak mümkündür. Daha fazla bilgi için bkz. Azure [Hızlı Başlangıç şablonlarını kullanarak bir Azure VM 'de SQL Server Için her zaman açık kullanılabilirlik grubu yapılandırma](availability-group-quickstart-template-configure.md). | 
| **SQL VM kaynak sağlayıcısına otomatik kayıt** | Bu aydan sonra dağıtılan SQL Server VM 'Ler, yeni SQL VM kaynak sağlayıcısına otomatik olarak kaydedilir. Bu aya göre dağıtılan SQL Server sanal makinelerin yine de el ile kaydedilmesi gerekir. Daha fazla bilgi için bkz. [Azure 'da SQL Server sanal MAKINESINI SQL VM kaynak sağlayıcısıyla kaydetme](sql-vm-resource-provider-register.md).|
|**Yeni SQL VM kaynak sağlayıcısı** |  Yeni bir kaynak sağlayıcısı (Microsoft. SqlVirtualMachine) SQL Server sanal makinelerinizin daha iyi yönetilmesini sağlar. VM 'lerinizi kaydetme hakkında daha fazla bilgi için bkz. [Azure 'da SQL Server sanal MAKINESINI SQL VM kaynak sağlayıcısıyla kaydetme](sql-vm-resource-provider-register.md). |
|**Lisans modelini Değiştir** | Artık Azure CLı veya PowerShell kullanarak SQL Server VM için, kullanım başına ödeme ve kendi lisans modelleriniz arasında geçiş yapabilirsiniz. Daha fazla bilgi için bkz. [Azure 'da SQL Server sanal makinesi için lisans modelini değiştirme](licensing-model-azure-hybrid-benefit-ahb-change.md). | 
| &nbsp; | &nbsp; |

## <a name="additional-resources"></a>Ek kaynaklar

**Windows VM 'leri**:

* [Windows VM 'de SQL Server genel bakış](sql-server-on-azure-vm-iaas-what-is-overview.md)
* [Windows VM 'de SQL Server sağlama](create-sql-vm-portal.md)
* [Bir veritabanını Azure VM 'de SQL Server geçirme](migrate-to-vm-from-sql-server.md)
* [Azure sanal makinelerinde SQL Server için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](business-continuity-high-availability-disaster-recovery-hadr-overview.md)
* [Azure sanal makinelerinde SQL Server için en iyi performans uygulamaları](performance-guidelines-best-practices.md)
* [Azure sanal makinelerinde SQL Server için uygulama desenleri ve geliştirme stratejileri](application-patterns-development-strategies.md)

**Linux VM 'leri**:

* [Linux VM 'de SQL Server genel bakış](../linux/sql-server-on-linux-vm-what-is-iaas-overview.md)
* [Linux sanal makinesinde SQL Server sağlama](../linux/sql-vm-create-portal-quickstart.md)
* [SSS (Linux)](../linux/frequently-asked-questions-faq.md)
* [Linux üzerinde SQL Server belgeleri](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
