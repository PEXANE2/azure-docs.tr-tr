---
title: Azure sanal makinelerinde SQL Server için belge değişiklikleri | Microsoft Docs
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
ms.date: 01/06/2020
ms.openlocfilehash: b9d2b3916e4509e5ce3aa8a9ddd66f1cf14a8827
ms.sourcegitcommit: 2f8ff235b1456ccfd527e07d55149e0c0f0647cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75690867"
---
# <a name="documentation-changes-for-sql-server-on-azure-virtual-machines"></a>Azure sanal makinelerinde SQL Server için belge değişiklikleri

Azure, içinde yerleşik SQL Server bir görüntü ile sanal makine (VM) dağıtmanıza olanak tanır. Bu makalede, [Azure sanal makinelerinde son SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/)sürümlerindeki yeni özellikler ve geliştirmelerle ilişkili belge değişiklikleri özetlenmektedir. 


## <a name="january-2020"></a>Ocak 2020

| Değişiklikler | Ayrıntılar |
| --- | --- |
| **Azure Kamu desteği** | Artık [Azure Kamu](https://azure.microsoft.com/global-infrastructure/government/) bulutunda barındırılan sanal MAKINELER IÇIN SQL VM kaynak sağlayıcısı ile SQL Server sanal makineleri kaydetmek mümkündür. | 
| &nbsp; | &nbsp; |

## <a name="2019"></a>2019

|Değişiklikler | Ayrıntılar |
 --- | --- |
| **Azure 'da ücretsiz DR çoğaltması** | [Yazılım güvencesi](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?rtc=1&activetab=software-assurance-default-pivot:primaryr3)'niz varsa şirket içi SQL Server Örneğiniz için Azure 'da olağanüstü durum kurtarma için [ücretsiz bir pasif örnek](virtual-machines-windows-sql-high-availability-dr.md#free-dr-replica-in-azure) barındırabilirsiniz. | 
| **Toplu kaynak sağlayıcısı kaydı** | Artık SQL sanal makinelerini kaynak sağlayıcıyla [toplu olarak kaydedebilirsiniz](virtual-machines-windows-sql-bulk-register-with-resource-provider.md) . | 
|**Performans için Iyileştirilmiş depolama yapılandırması** | Artık yeni bir SQL Server VM oluştururken [depolama yapılandırmanızı tamamen özelleştirebilirsiniz](virtual-machines-windows-sql-server-storage-configuration.md#new-vms) . |
|**FCı için Premium dosya paylaşma** | Artık [depolama alanları doğrudan](virtual-machines-windows-portal-sql-create-failover-cluster.md)orijinal yöntemi yerine [Premium dosya paylaşma](virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-share.md) kullanarak bir yük devretme kümesi örneği oluşturabilirsiniz. 
| **Azure adanmış ana bilgisayar** | SQL Server VM [Azure adanmış bir konakta](virtual-machines-windows-sql-dedicated-host.md)çalıştırabilirsiniz. | 
| **SQL VM 'yi farklı bir bölgeye taşıma** | [SQL Server VM bir bölgeden diğerine geçirmek](virtual-machines-windows-sql-move-different-region.md)için Azure Site Recovery kullanın. |
|  **Yeni SQL IaaS yükleme modları** | SQL Server hizmetinin yeniden başlatılmasını önlemek için SQL Server IaaS uzantısını [hafif modda](virtual-machines-windows-sql-server-agent-extension.md) yüklemek mümkündür.  |
| **SQL Server sürümü değişikliği** | Artık SQL Server VM için [sürüm özelliğini](virtual-machines-windows-sql-change-edition.md) değiştirebilirsiniz. |
| **SQL VM kaynak sağlayıcısında yapılan değişiklikler** | [SQL VM kaynak sağlayıcısı ile SQL Server VM](virtual-machines-windows-sql-register-with-resource-provider.md) yeni SQL IaaS modlarını kullanarak kaydedebilirsiniz. Bu özellik [Windows Server 2008](virtual-machines-windows-sql-register-with-resource-provider.md#management-modes) görüntülerini içerir.|
| **Azure Hibrit Avantajı kullanarak kendi lisans görüntülerini getirin** | Azure Marketi 'nden dağıtılan kendi lisans görüntülerini getir, şimdi de [Lisans türlerini Kullandıkça Öde olarak](virtual-machines-windows-sql-ahb.md#remarks)değiştirebilir.| 
| **Azure portal yeni SQL Server VM yönetimi** | Artık Azure portal SQL Server VM yönetmenin bir yolu vardır. Daha fazla bilgi için bkz. [Azure portal SQL Server VM 'Leri yönetme](virtual-machines-windows-sql-manage-portal.md).  | 
| **SQL Server 2008/2008 R2 için genişletilmiş destek** | SQL Server 2008 ve SQL Server 2008 R2 desteğini bir Azure VM 'ye *olduğu gibi* geçirerek [genişletin](virtual-machines-windows-sql-server-2008-eos-extend-support.md) . | 
| **Özel görüntü desteklenebilirliği** | Artık [SQL Server IaaS uzantısını](virtual-machines-windows-sql-server-agent-extension.md#installation) özel işletim SISTEMI ve SQL yansımalarına yükleyebilirsiniz ve bu da [Esnek lisansın](virtual-machines-windows-sql-ahb.md)sınırlı işlevlerini sunar. Özel görüntünüzü SQL kaynak sağlayıcısı ile kaydederken, lisans türünü "AHUB" olarak belirtin. Aksi takdirde, kayıt başarısız olur. | 
| **Adlandırılmış örnek desteklenebilirliği** | Varsayılan örnek düzgün şekilde kaldırılmışsa, artık [SQL Server IaaS uzantısını](virtual-machines-windows-sql-server-agent-extension.md#installation) adlandırılmış bir örnekle kullanabilirsiniz. | 
| **Portal geliştirmesi** | SQL Server VM dağıtmaya yönelik Azure portal deneyim, kullanılabilirliği iyileştirmeye yönelik olarak yeniden belirlenmiştir. Daha fazla bilgi için bkz. kısa [Başlangıç](quickstart-sql-vm-create-portal.md) ve daha kapsamlı [nasıl yapılır Kılavuzu](virtual-machines-windows-portal-sql-server-provision.md) SQL Server VM dağıtma.|
|  **Portal geliştirme** | Bir SQL Server VM için lisans modelini, [Azure Portal](virtual-machines-windows-sql-ahb.md#change-the-license-for-vms-already-registered-with-the-resource-provider)kullanarak kendi lisansınıza sahip olmak üzere Kullandıkça Öde için değiştirmek mümkündür.|
| **Azure SQL Server VM CLı ile kullanılabilirlik grubu dağıtımının basitleştirmesi** | Artık bir kullanılabilirlik grubunu Azure 'da bir SQL Server VM dağıtmaya hiç zamankinden daha kolay. Windows Yük devretme kümesi, iç yük dengeleyici ve kullanılabilirlik grubu dinleyicilerini komut satırından oluşturmak için [Azure CLI](/cli/azure/sql/vm?view=azure-cli-2018-03-01-hybrid) 'yı kullanabilirsiniz. Daha fazla bilgi için bkz. Azure [SQL Server VM CLI kullanarak bir Azure VM 'de SQL Server Için her zaman açık kullanılabilirlik grubu yapılandırma](virtual-machines-windows-sql-availability-group-cli.md). | 
| &nbsp; | &nbsp; |

## <a name="2018"></a>2018 

 Değişiklikler | Ayrıntılar |
| --- | --- |
|  **SQL Server kümesi için yeni kaynak sağlayıcısı** | Yeni bir kaynak sağlayıcısı (Microsoft. SqlVirtualMachine/SqlVirtualMachineGroups), Windows Yük devretme kümesinin meta verilerini tanımlar. Windows Server yük devretme kümesi (WSFC) hizmeti önyükleme ve VM 'yi kümeyle birleştiren *Sqlvirtualmachinegroups* 'a katılma SQL Server VM.  |
| **Azure hızlı başlangıç şablonları ile bir kullanılabilirlik grubu dağıtımının otomatik kurulumu** |Windows Yük devretme kümesi oluşturmak, bu kümeye SQL Server VM 'Leri birleştirmek, dinleyiciyi oluşturmak ve iç yük dengeleyiciyi iki Azure hızlı başlangıç şablonu ile yapılandırmak mümkündür. Daha fazla bilgi için bkz. Azure [Hızlı Başlangıç şablonlarını kullanarak bir Azure VM 'de SQL Server Için her zaman açık kullanılabilirlik grubu yapılandırma](virtual-machines-windows-sql-availability-group-quickstart-template.md). | 
| **SQL VM kaynak sağlayıcısına otomatik kayıt** | Bu aydan sonra dağıtılan SQL Server VM 'Ler yeni SQL Server kaynak sağlayıcısına otomatik olarak kaydedilir. Bu aya göre dağıtılan SQL Server sanal makinelerin yine de el ile kaydedilmesi gerekir. Daha fazla bilgi için bkz. [Azure 'da SQL Server sanal MAKINESINI SQL VM kaynak sağlayıcısıyla kaydetme](virtual-machines-windows-sql-register-with-resource-provider.md).|
|**Yeni SQL VM kaynak sağlayıcısı** |  Yeni bir kaynak sağlayıcısı (Microsoft. SqlVirtualMachine) SQL Server sanal makinelerinizin daha iyi yönetilmesini sağlar. VM 'lerinizi kaydetme hakkında daha fazla bilgi için bkz. [Azure 'da SQL Server sanal MAKINESINI SQL VM kaynak sağlayıcısıyla kaydetme](virtual-machines-windows-sql-register-with-resource-provider.md). |
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
