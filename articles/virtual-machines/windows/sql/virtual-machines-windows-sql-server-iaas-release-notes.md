---
title: Azure Sanal Makinelerde SQL Server için dokümantasyon değişiklikleri| Microsoft Dokümanlar
description: Azure VM'de SQL Server'ın yeni özellikleri ve geliştirmeleri hakkında bilgi edinin
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
ms.openlocfilehash: 27682863a96e2f190a0dafe6e4d783029e987453
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77201654"
---
# <a name="documentation-changes-for-sql-server-on-azure-virtual-machines"></a>Azure Sanal Makinelerde SQL Server için dokümantasyon değişiklikleri

Azure, yerleşik SQL Server görüntüsüne sahip sanal bir makine (VM) dağıtmanıza olanak tanır. Bu makalede, [Sql Server'ın Azure Sanal Makinelerdeki](https://azure.microsoft.com/services/virtual-machines/sql-server/)son sürümlerinde yeni özellikler ve geliştirmelerle ilişkili dokümantasyon değişiklikleri özetlenmiştir. 


## <a name="january-2020"></a>Ocak 2020

| Değişiklikler | Ayrıntılar |
| --- | --- |
| **Azure Devlet desteği** | Azure [Devlet](https://azure.microsoft.com/global-infrastructure/government/) bulutunda barındırılan sanal makineler için SQL VM kaynak sağlayıcısına SQL Server sanal makinelerini kaydetmek artık mümkün. | 
| &nbsp; | &nbsp; |

## <a name="2019"></a>2019

|Değişiklikler | Ayrıntılar |
 --- | --- |
| **Azure'da ücretsiz DR çoğaltma** | [Yazılım Güvenceniz](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?rtc=1&activetab=software-assurance-default-pivot:primaryr3)varsa, şirket içi SQL Server örneğiniz için Azure'da olağanüstü durum kurtarma için ücretsiz pasif bir [örnek](virtual-machines-windows-sql-high-availability-dr.md#free-dr-replica-in-azure) barındırabilirsiniz. | 
| **Toplu kaynak sağlayıcı kaydı** | Artık SQL sanal makinelerini kaynak sağlayıcısına [toplu](virtual-machines-windows-sql-bulk-register-with-resource-provider.md) olarak kaydedebilirsiniz. | 
|**Performans Optimize Edilmiş Depolama Yapılandırması** | Artık yeni bir SQL Server VM oluştururken [depolama yapılandırmanızı tamamen özelleştirebilirsiniz.](virtual-machines-windows-sql-server-storage-configuration.md#new-vms) |
|**FCI için premium dosya paylaşımı** | Artık [Depolama Alanları Direct'in](virtual-machines-windows-portal-sql-create-failover-cluster.md)özgün yöntemi yerine [Premium Dosya Paylaşımı'nı](virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-share.md) kullanarak bir başarısız küme örneği oluşturabilirsiniz. 
| **Azure'a özel ana bilgisayar** | SQL Server VM'nizi [Azure Özel Ana Bilgisayar'da](virtual-machines-windows-sql-dedicated-host.md)çalıştırabilirsiniz. | 
| **SQL VM'yi farklı bölgeye taşıma** | [SQL Server VM'nizi bir bölgeden diğerine geçirmek için](virtual-machines-windows-sql-move-different-region.md)Azure Site Kurtarma'yı kullanın. |
|  **Yeni SQL IaaS kurulum modları** | SQL Server hizmetini yeniden başlatmayı önlemek için SQL Server IaaS uzantısını [hafif modda](virtual-machines-windows-sql-server-agent-extension.md) yüklemek artık mümkün.  |
| **SQL Server sürüm değişikliği** | Artık SQL Server VM'nizin [sürüm özelliğini](virtual-machines-windows-sql-change-edition.md) değiştirebilirsiniz. |
| **SQL VM kaynak sağlayıcısında yapılan değişiklikler** | Yeni SQL IaaS modlarını kullanarak [SQL Server VM'inizi SQL VM kaynak sağlayıcısına](virtual-machines-windows-sql-register-with-resource-provider.md) kaydedebilirsiniz. Bu [özellik, Windows Server 2008](virtual-machines-windows-sql-register-with-resource-provider.md#management-modes) görüntülerini içerir.|
| **Azure Karma Avantajı'nı kullanarak kendi lisans resimlerinizi getirin** | Azure Marketi'nden dağıtılan kendi lisansınızı getir görüntüleri artık [lisans tiplerini istediğiniz kadar öde](virtual-machines-windows-sql-ahb.md#remarks)olarak değiştirebilir.| 
| **Azure portalında yeni SQL Server VM yönetimi** | Artık AZURE portalında SQL Server VM'nizi yönetmenin bir yolu var. Daha fazla bilgi için Azure [portalındaki SQL Server VM'lerini Yönet'e](virtual-machines-windows-sql-manage-portal.md)bakın.  | 
| **SQL Server 2008/2008 R2 için genişletilmiş destek** | SQL Server 2008 ve SQL Server 2008 R2 desteğini azure VM'ye *olduğu gibi* geçirerek [genişletin.](virtual-machines-windows-sql-server-2008-eos-extend-support.md) | 
| **Özel görüntü desteklenebilirliği** | Artık [sql server iaas uzantısını,](virtual-machines-windows-sql-server-agent-extension.md#installation) esnek [lisanslamanın](virtual-machines-windows-sql-ahb.md)sınırlı işlevselliğini sunan özel işletim sistemi ve SQL görüntülerine yükleyebilirsiniz. Özel resminizi SQL kaynak sağlayıcısına kaydederken, lisans türünü "AHUB" olarak belirtin. Aksi takdirde, kayıt başarısız olur. | 
| **Adlandırılmış örnek desteklenebilirliği** | Varsayılan örnek düzgün bir şekilde kaldırıldıysa, artık [SQL Server IaaS uzantısını](virtual-machines-windows-sql-server-agent-extension.md#installation) adlandırılmış bir örnekle kullanabilirsiniz. | 
| **Portal geliştirme** | Kullanılabilirliği artırmak için SQL Server VM dağıtmaya yönelik Azure portalı deneyimi yenilendi. Daha fazla bilgi için, sql server VM dağıtmak için kısa [hızlı başlatma](quickstart-sql-vm-create-portal.md) ve daha ayrıntılı [nasıl yapılır kılavuzuna](virtual-machines-windows-portal-sql-server-provision.md) bakın.|
| **Portal geliştirme** | Artık bir SQL Server VM'nin lisans modelini kullandıkça öde'den azure [portalını](virtual-machines-windows-sql-ahb.md#vms-already-registered-with-the-resource-provider)kullanarak kendi lisansınızı getire değiştirmek mümkündür.|
| **Azure SQL Server VM CLI ile kullanılabilirlik grubu dağıtımının basitleştirilmesi** | Kullanılabilirlik grubunu Azure'daki bir SQL Server VM'ye dağıtmak artık her zamankinden daha kolay. Windows failover kümesini, dahili yük bakiyesini ve kullanılabilirlik grubu dinleyicilerini komut satırından oluşturmak için [Azure CLI'yi](/cli/azure/sql/vm?view=azure-cli-2018-03-01-hybrid) kullanabilirsiniz. Daha fazla bilgi için, [SQL Server için Her Zaman Kullanılabilirlik grubunu bir Azure VM'de yapılandırmak için Azure SQL Server VM CLI'yi kullanın'](virtual-machines-windows-sql-availability-group-cli.md)a bakın. | 
| &nbsp; | &nbsp; |

## <a name="2018"></a>2018 

 Değişiklikler | Ayrıntılar |
| --- | --- |
|  **BIR SQL Server kümesi için yeni kaynak sağlayıcısı** | Yeni bir kaynak sağlayıcısı (Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups) Windows failover kümesinin meta verilerini tanımlar. SQL Server VM'yi *SqlVirtualMachineGroups'a* katılmak, Windows Server Failover Cluster (WSFC) hizmetini yakalar ve VM'yi kümeye katılır.  |
| **Azure hızlı başlatma şablonları ile kullanılabilirlik grubu dağıtımının otomatik kurulumu** |Artık Windows failover kümesini oluşturmak, SQL Server VM'lerine katılmak, dinleyiciyi oluşturmak ve dahili yük bakiyesini iki Azure hızlı başlatma şablonuyla yapılandırmak mümkündür. Daha fazla bilgi için, [SQL Server için Her Zaman Kullanılabilirlik grubunu bir Azure VM'de yapılandırmak için Azure hızlı başlatma şablonlarını kullan](virtual-machines-windows-sql-availability-group-quickstart-template.md)'a bakın. | 
| **SQL VM kaynak sağlayıcısına otomatik kayıt** | Bu aydan sonra dağıtılan SQL Server VM'ler otomatik olarak yeni SQL Server kaynak sağlayıcısına kaydedilir. Bu aydan önce dağıtılan SQL Server VM'lerin hala el ile kaydedilmesi gerekir. Daha fazla bilgi için [bkz.](virtual-machines-windows-sql-register-with-resource-provider.md)|
|**Yeni SQL VM kaynak sağlayıcısı** |  Yeni bir kaynak sağlayıcısı (Microsoft.SqlVirtualMachine), SQL Server VM'lerinizin daha iyi yönetilmesini sağlar. VM'lerinizi kaydetme hakkında daha fazla bilgi için [azure'da SQL Server sanal makinesini SQL VM kaynak sağlayıcısına kaydedin.](virtual-machines-windows-sql-register-with-resource-provider.md) |
|**Lisans modelini değiştir** | Artık Azure CLI veya PowerShell'i kullanarak SQL Server VM'iniz için kullanım başına ödeme ve kendi lisansını getir modelleri arasında geçiş yapabilirsiniz. Daha fazla bilgi için [Azure'daki bir SQL Server sanal makinesinin lisans modelini nasıl değiştireceğiniz](virtual-machines-windows-sql-ahb.md)e bakın. | 
| &nbsp; | &nbsp; |

## <a name="additional-resources"></a>Ek kaynaklar

**Windows VMs**:

* [Windows VM'de SQL Server'a Genel Bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server Windows VM sağlama](virtual-machines-windows-portal-sql-server-provision.md)
* [Azure VM'de veritabanını SQL Server'a geçirme](virtual-machines-windows-migrate-sql.md)
* [Azure Sanal Makinelerde SQL Server için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](virtual-machines-windows-sql-high-availability-dr.md)
* [Azure Sanal Makinelerde SQL Server için en iyi performans uygulamaları](virtual-machines-windows-sql-performance.md)
* [Azure Sanal Makinelerde SQL Server için uygulama modelleri ve geliştirme stratejileri](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Linux VMs**:

* [Linux VM'de SQL Server'a Genel Bakış](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [Sql Server Linux sanal makine sağlama](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [SSS (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [Linux üzerinde SQL Server belgeleri](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
