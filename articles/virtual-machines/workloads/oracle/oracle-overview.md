---
title: Microsoft Azure Oracle çözümleri | Microsoft Docs
description: Tamamen Azure altyapısında çalışan veya Oracle bulut altyapısı (OCı) ile platformlar arası bağlantı kullanma dahil olmak üzere Microsoft Azure ' de Oracle uygulamalarını ve çözümlerini dağıtmaya yönelik seçenekler hakkında bilgi edinin.
services: virtual-machines-linux
documentationcenter: ''
author: romitgirdhar
manager: gwallace
tags: azure-resource-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/04/2019
ms.author: rogirdh
ms.openlocfilehash: e9e37c54668ec0343cbfd45e51e90216955b46c4
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70100015"
---
# <a name="overview-of-oracle-applications-and-solutions-on-azure"></a>Azure 'da Oracle uygulamalarına ve çözümlerine genel bakış

Bu makalede, Azure altyapısını kullanarak Oracle çözümlerini çalıştırmaya yönelik yetenekler açıklanır. Ayrıca bkz. Azure Marketi 'nde kullanılabilir [Oracle VM görüntülerine](oracle-vm-solutions.md) yönelik ayrıntılı tanıtımları ve [Oracle bulut altyapısı (OCI) ile Azure bağlantısı](oracle-oci-overview.md)için Önizleme özelliği.

## <a name="oracle-databases-on-azure-infrastructure"></a>Azure altyapısında Oracle veritabanları

Azure Marketi 'nde bulunan Linux görüntülerini kullanarak Azure altyapısında Oracle veritabanlarını çalıştırın:

* Oracle Database 12,1, 12,2 ve 18,3 Enterprise Edition 

* Oracle Database 12,1, 12,2 ve 18,3 Standard Edition 

Ayrıca, Azure 'daki sıfırdan oluşturduğunuz özel bir görüntüye bir çözüm temelinizi veya şirket içi ortamınızdan özel bir görüntü yüklemeyi tercih edebilirsiniz.

İsteğe bağlı olarak birden çok bağlı disk ile yapılandırın ve Oracle otomatik depolama yönetimi (ASM) yükleyerek veritabanı performansını geliştirebilirsiniz.

## <a name="applications-on-oracle-linux-and-weblogic-server"></a>Oracle Linux ve WebLogic Server 'daki uygulamalar

Desteklenen Oracle işletim sistemlerinde Azure 'da kurumsal uygulamaları çalıştırın. Aşağıdaki görüntüler Azure Marketi 'nde mevcuttur:

* Oracle WebLogic Server 12.1.2

* Oracle Linux (UEK) 6,8, 6,9, 6,10, 7,3, 7,4, 7,5 ve 7,6

## <a name="high-availability-and-disaster-recovery-options"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma seçenekleri

* Azure altyapısında, yüksek kullanılabilirlik için [kullanılabilirlik alanları](../../../availability-zones/az-overview.md) birlikte Oracle Data Guard, Active Data Guard veya GoldenGate 'i yapılandırın.

* Azure 'da ve şirket içi veya fiziksel sunucularınızdaki Oracle Linux sanal makinelerinize yönelik olağanüstü durum kurtarmayı düzenlemek ve yönetmek için [Azure Site Recovery](../../../site-recovery/site-recovery-overview.md) kullanın. 

* Azure 'da [Flashgrid ufuk kümesi](https://www.flashgrid.io/oracle-rac-in-azure/)kullanarak Oracle gerçek uygulama KÜMELERINI (RAC) etkinleştirin.

## <a name="integration-of-azure-with-oci-preview"></a>OCı ile Azure tümleştirmesi (Önizleme)

Oracle bulut altyapısında (OCı) arka uç veritabanlarına bağlı olan Azure altyapısında Oracle uygulamaları çalıştırın. Bu çözüm aşağıdaki özellikleri kullanır: 

* **Platformlar arası ağ** -uygulama ve veritabanı katmanı arasında yüksek bant genişliğine sahip, özel ve düşük gecikme süreli bağlantılar oluşturmak Için Azure ExpressRoute ve Oracle FastConnect arasında bulunan doğrudan Interconnect 'i kullanın.
* **Tümleşik kimlik** -çözümler için tek bir kimlik kaynağı oluşturmak üzere Azure AD ile Oracle IDCs arasında federasyon kimliği ayarlayın. OCı ve Azure arasında kaynakları yönetmek için çoklu oturum açmayı etkinleştirin.

### <a name="deploy-oracle-applications-on-azure"></a>Azure 'da Oracle uygulamaları dağıtma

Azure altyapısını ayarlamak için Teraform şablonlarını kullanın ve platformlar arası yapılandırmada çalışmak üzere doğrulanan ve desteklenen Oracle uygulamalarını yükler:

* E-Iş paketi
* JD edi EnterpriseOne
* PeopleSoft
* Oracle perakende uygulamaları
* Oracle Hyperion finans yönetimi

Ayrıca, Azure 'da OCı ve diğer Azure hizmetleriyle bağlantı sağlayan özel uygulamalar dağıtın.

### <a name="set-up-oracle-databases-in-oci"></a>OCı 'da Oracle veritabanlarını ayarlama

Azure 'da çalışan Oracle uygulamalarıyla birlikte Oracle Database Cloud Services (özerk veritabanı, RAC, sınavı verileri, DBaaS, tek düğüm) kullanın. [OCI veritabanı seçenekleri](https://docs.cloud.oracle.com/iaas/Content/Database/Concepts/databaseoverview.htm)hakkında daha fazla bilgi edinin. 
 

## <a name="licensing"></a>Lisanslama

Oracle uygulamalarının Azure 'da dağıtılması, "kendi lisansını getir" modelini temel alır. Oracle yazılımını kullanmak için doğru lisansa sahip olduğunuz ve Oracle ile geçerli bir destek sözleşmenizin olduğu varsayılır. Oracle 'ın Şirket içinden Azure 'a lisans taşınabilirlik garantisi vardır. Bkz. Oracle-Azure [SSS](https://www.oracle.com/cloud/technologies/oracle-azure-faq.html).

## <a name="next-steps"></a>Sonraki adımlar

* Azure altyapısına [Oracle VM görüntülerini](oracle-vm-solutions.md) dağıtma hakkında daha fazla bilgi edinin.

* [OCI Ile Azure bağlantısı](oracle-oci-overview.md)oluşturma hakkında daha fazla bilgi edinin.