---
title: Kullanılabilirlik gruplarına genel bakış
description: Bu makalede, Azure sanal makinelerde SQL Server Kullanılabilirlik Grupları tanıtıştır.
services: virtual-machines
documentationCenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: 601eebb1-fc2c-4f5b-9c05-0e6ffd0e5334
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/13/2017
ms.author: mikeray
ms.custom: seo-lt-2019
ms.openlocfilehash: 8119990ab4ab4a918e325976092100086a547aa4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74037491"
---
# <a name="introducing-sql-server-availability-groups-on-azure-virtual-machines"></a>Azure sanal makinelerde SQL Server kullanılabilirlik gruplarını tanıtma

Bu makalede, Azure Sanal Makineler'de SQL Server kullanılabilirlik grupları tanıtıştır. 

Azure Sanal Makinelerdeki kullanılabilirlik grupları her zaman her zaman şirket içi kullanılabilirlik gruplarına benzer. Daha fazla bilgi için her [zaman kullanılabilirlik grupları (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx)bakın. 

Diyagram, Azure Sanal Makineler'deki eksiksiz bir SQL Server Kullanılabilirlik Grubu'nun bölümlerini gösterir.

![Kullanılabilirlik Grubu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

Azure Sanal Makinelerdeki Kullanılabilirlik Grubu için temel fark, Azure sanal makinelerinin bir [yük dengeleyicisi gerektirmesidir.](../../../load-balancer/load-balancer-overview.md) Yük dengeleyicisi kullanılabilirlik grubu dinleyicisinin IP adreslerini tutar. Birden fazla kullanılabilirlik grubunvarsa, her grup bir dinleyici gerektirir. Bir yük dengeleyicisi birden fazla dinleyiciyi destekleyebilir.

Ayrıca, bir Azure IaaS VM konuk failover kümesinde sunucu başına tek bir NIC (küme düğümü) ve tek bir alt ağ öneririz. Azure ağındaki fiziksel yedeklilik nedeniyle Azure IaaS VM konuk kümesinde ek NIC ve alt ağ kullanılmasına gerek yoktur. Küme doğrulama raporu, düğümlerin yalnızca tek bir ağ üzerinde erişilebilir durumda olduğuna dair bir uyarı gösterse de bu uyarı Azure IaaS VM konuk yük devretme kümelerinde güvenli bir şekilde yoksayılabilir. 

Fazlalık ve yüksek kullanılabilirlik artırmak için, SQL Server VM'ler aynı [kullanılabilirlik kümesinde](virtual-machines-windows-portal-sql-availability-group-prereq.md#create-availability-sets)veya farklı [kullanılabilirlik bölgelerinde](/azure/availability-zones/az-overview)olmalıdır. 

|  | Windows Server Sürümü | SQL Server Sürümü | SQL Server Sürümü | WSFC Çoğunluk Config | Multi-region ile DR | Çoklu alt ağ desteği | Varolan bir AD için destek | Çok bölgeli aynı bölgeli DR | AD etki alanı olmayan Dist-AG desteği | Kümesiz Dist-AG desteği |  
| :------ | :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----|
| [SQL VM CLI](virtual-machines-windows-sql-availability-group-cli.md) | 2016 | 2017 </br>2016   | Kbb | Bulut tanığı | Hayır | Evet | Evet | Evet | Hayır | Hayır |
| [Hızlı Başlangıç Şablonları](virtual-machines-windows-sql-availability-group-quickstart-template.md) | 2016 | 2017</br>2016  | Kbb | Bulut tanığı | Hayır | Evet | Evet | Evet | Hayır | Hayır |
| [Portal Şablonu](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) | 2016 </br>2012 R2 | 2016</br>2014 | Kbb | Dosya paylaşımı | Hayır | Hayır | Hayır | Hayır | Hayır | Hayır |
| [El ile](virtual-machines-windows-portal-sql-availability-group-prereq.md) | Tümü | Tümü | Tümü | Tümü | Evet | Evet | Evet | Evet | Evet | Evet |
| &nbsp; | &nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |

Azure Sanal Makinelerde bir SQL Server kullanılabilirlik grubu oluşturmaya hazır olduğunuzda, bu öğreticilere bakın.

## <a name="manually-with-azure-cli"></a>Azure CLI ile el ile
Kullanılabilirlik grubunu yapılandırmak ve dağıtmak için Azure CLI'yi kullanmak, basitlik ve dağıtım hızı açısından en iyisi olduğundan, önerilen seçenektir. Azure CLI ile, Windows Failover Cluster'ın oluşturulması, SQL Server VM'lerinin kümeye katılmasının yanı sıra dinleyici ve Dahili Yük Dengeleyicisi'nin oluşturulması 30 dakikanın altında bir sürede elde edilebilir. Bu seçenek hala kullanılabilirlik grubunun el ile oluşturulmasını gerektirir, ancak diğer tüm gerekli yapılandırma adımlarını otomatikleştirir. 

Daha fazla bilgi için, [SQL Server için Her Zaman kullanılabilirlik grubunu bir Azure VM'de yapılandırmak için Azure SQL VM CLI'yi kullanın'a](virtual-machines-windows-sql-availability-group-cli.md)bakın. 

## <a name="automatically-with-azure-quickstart-templates"></a>Azure Quickstart Şablonları ile otomatik olarak
Azure Quickstart Şablonları, Windows Failover Cluster'ı dağıtmak, SQL Server VM'lerine katılmak, dinleyiciyi oluşturmak ve Dahili Yük Bakiyesini yapılandırmak için SQL VM kaynak sağlayıcısını kullanır. Bu seçenek hala kullanılabilirlik grubunun ve Dahili Yük Dengeleyicisi'nin (ILB) el ile oluşturulmasını gerektirir, ancak diğer tüm gerekli yapılandırma adımlarını (ILB yapılandırması dahil) otomatikleştirir ve basitleştirir. 

Daha fazla bilgi için, [sql sunucusunun her zaman kullanılabilirlik grubunu bir Azure VM'de yapılandırmak için Azure Hızlı Başlatma](virtual-machines-windows-sql-availability-group-quickstart-template.md)Şablonu'nun kullan'ına bakın.


## <a name="automatically-with-an-azure-portal-template"></a>Azure portalı şablonuyla otomatik olarak

[Azure VM'de Her Zaman Kullanılabilirlik Grubunda otomatik olarak yapılandırın - Kaynak Yöneticisi](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)


## <a name="manually-in-azure-portal"></a>Azure portalında el ile

Ayrıca şablon olmadan sanal makineleri kendiniz oluşturabilirsiniz. Önce ön koşulları tamamlayın, ardından kullanılabilirlik grubunu oluşturun. Aşağıdaki konulara bakın: 

- [Azure Sanal Makinelerde Kullanılabilirlik Gruplarında SQL Server Her Zaman için ön koşulları yapılandırın](virtual-machines-windows-portal-sql-availability-group-prereq.md)

- [Kullanılabilirliği ve olağanüstü durum kurtarmayı iyileştirmek için Her Zaman Kullanılabilirlik Grubu oluşturun](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

## <a name="next-steps"></a>Sonraki adımlar

[Farklı Bölgelerdeki Azure Sanal Makinelerde Sql Server Always On Availability Group'u Yapılandırma](virtual-machines-windows-portal-sql-availability-group-dr.md)
