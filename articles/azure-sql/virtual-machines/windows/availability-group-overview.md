---
title: Kullanılabilirlik gruplarına genel bakış
description: Bu makalede, Azure sanal makinelerinde SQL Server kullanılabilirlik grupları tanıtılmaktadır.
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
ms.openlocfilehash: 62dce0204f77ab65473fc1735015e41f483dddb1
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84049137"
---
# <a name="introducing-sql-server-availability-groups-on-azure-virtual-machines"></a>Azure sanal makinelerinde SQL Server kullanılabilirlik grupları ile tanışın
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Bu makalede, Azure sanal makinelerinde SQL Server kullanılabilirlik grupları tanıtılmaktadır. 

Azure sanal makinelerinde her zaman açık kullanılabilirlik grupları, şirket içinde her zaman açık kullanılabilirlik gruplarına benzerdir. Daha fazla bilgi için bkz. [Always on kullanılabilirlik grupları (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx). 

Diyagramda, Azure sanal makineler 'de tüm SQL Server kullanılabilirlik grubunun bölümleri gösterilmektedir.

![Kullanılabilirlik Grubu](./media/availability-group-overview/00-EndstateSampleNoELB.png)

Azure sanal makinelerinde bir kullanılabilirlik grubunun temel farkı, Azure sanal makinelerinin bir [yük dengeleyici](../../../load-balancer/load-balancer-overview.md)gerektiridir. Yük dengeleyici, kullanılabilirlik grubu dinleyicisinin IP adreslerini tutar. Birden fazla kullanılabilirlik grubunuz varsa her grup için bir dinleyici gerekir. Bir yük dengeleyici birden çok dinleyicisi destekleyebilir.

Ayrıca, bir Azure IaaS VM konuk yük devretme kümesinde, sunucu başına (küme düğümü) ve tek bir alt ağ için tek bir NIC önerilir. Azure ağındaki fiziksel yedeklilik nedeniyle Azure IaaS VM konuk kümesinde ek NIC ve alt ağ kullanılmasına gerek yoktur. Küme doğrulama raporu, düğümlerin yalnızca tek bir ağ üzerinde erişilebilir durumda olduğuna dair bir uyarı gösterse de bu uyarı Azure IaaS VM konuk yük devretme kümelerinde güvenli bir şekilde yoksayılabilir. 

Yedeklilik ve yüksek kullanılabilirliği artırmak için SQL Server VM 'Lerin aynı [kullanılabilirlik kümesinde](availability-group-manually-configure-prerequisites-tutorial.md#create-availability-sets)veya farklı [kullanılabilirlik bölgelerinde](/azure/availability-zones/az-overview)olması gerekir. 

|  | Windows Server sürümü | SQL Server sürümü | SQL Server sürümü | WSFC çekirdek yapılandırması | Çok bölgeli DR | Çoklu alt ağ desteği | Mevcut bir AD için destek | Birden çok bölgeye sahip DR aynı bölge | AD etki alanı olmadan Dist-AG desteği | Küme olmadan Dist-AG desteği |  
| :------ | :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----|
| [SQL VM CLı](availability-group-az-cli-configure.md) | 2016 | 2017 </br>2016   | Görünmeyen | Bulut tanığı | No | Yes | Yes | Yes | Hayır | Hayır |
| [Hızlı başlangıç şablonları](availability-group-quickstart-template-configure.md) | 2016 | 2017</br>2016  | Görünmeyen | Bulut tanığı | No | Yes | Yes | Yes | Hayır | Hayır |
| [Portal şablonu](availability-group-azure-marketplace-template-configure.md) | 2016 </br>2012 R2 | 2016</br>2014 | Görünmeyen | Dosya paylaşımı | Hayır | Hayır | Hayır | Hayır | Hayır | Hayır |
| [El ile](availability-group-manually-configure-prerequisites-tutorial.md) | Tümü | Tümü | Tümü | Tümü | Yes | Yes | Yes | Yes | Yes | Yes |
| &nbsp; | &nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |

Azure sanal makinelerinde SQL Server kullanılabilirlik grubu oluşturmaya hazır olduğunuzda, Bu öğreticilere bakın.

## <a name="manually-with-azure-cli"></a>Azure CLı ile el ile
Kullanılabilirlik grubunu yapılandırmak ve dağıtmak için Azure CLı kullanmak önerilen seçenektir, bu da basitlik ve dağıtım hızı bakımından en iyisidir. Azure CLı ile Windows Yük devretme kümesi oluşturma, SQL Server VM 'Leri kümeye katma ve dinleyicinin oluşturulması ve Iç Load Balancer tüm 30 dakika içinde elde edilebilir. Bu seçenek hala kullanılabilirlik grubunun el ile oluşturulmasını gerektirir, ancak diğer tüm gerekli yapılandırma adımlarını otomatik hale getirir. 

Daha fazla bilgi için bkz. Azure [SQL VM CLI kullanarak Azure VM 'de SQL Server Için Always on kullanılabilirlik grubunu yapılandırma](availability-group-az-cli-configure.md). 

## <a name="automatically-with-azure-quickstart-templates"></a>Azure hızlı başlangıç şablonlarıyla otomatik olarak
Azure hızlı başlangıç şablonları, Windows Yük devretme kümesini dağıtmak, VM SQL Server birleştirmek, dinleyiciyi oluşturmak ve Iç Load Balancer yapılandırmak için SQL VM kaynak sağlayıcısını kullanır. Bu seçenek, kullanılabilirlik grubunun ve Iç Load Balancer (ıLB) el ile oluşturulmasını gerektirir, ancak tüm gerekli yapılandırma adımlarını (ıLB 'nin yapılandırması dahil) otomatikleştirir ve basitleştirir. 

Daha fazla bilgi için bkz. Azure [hızlı başlangıç şablonunu kullanarak Azure VM 'de SQL Server Için Always on kullanılabilirlik grubunu yapılandırma](availability-group-quickstart-template-configure.md).


## <a name="automatically-with-an-azure-portal-template"></a>Azure portal şablonuyla otomatik olarak

[Azure VM 'de Always on kullanılabilirlik grubunu otomatik olarak yapılandırma-Kaynak Yöneticisi](availability-group-azure-marketplace-template-configure.md)


## <a name="manually-in-azure-portal"></a>Azure portal el ile

Ayrıca, şablon olmadan sanal makineleri kendiniz de oluşturabilirsiniz. İlk olarak, önkoşulları tamamlayıp kullanılabilirlik grubunu oluşturun. Aşağıdaki konulara bakın: 

- [Azure sanal makinelerinde SQL Server Always on kullanılabilirlik grupları için önkoşulları yapılandırma](availability-group-manually-configure-prerequisites-tutorial.md)

- [Kullanılabilirliği ve olağanüstü durum kurtarmayı geliştirmek için Always on kullanılabilirlik grubu oluşturma](availability-group-manually-configure-tutorial.md)

## <a name="next-steps"></a>Sonraki adımlar

[Farklı bölgelerde Azure sanal makinelerinde SQL Server Always on kullanılabilirlik grubu yapılandırma](availability-group-manually-configure-multiple-regions.md)
