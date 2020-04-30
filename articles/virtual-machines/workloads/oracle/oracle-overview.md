---
title: Microsoft Azure Oracle çözümleri | Microsoft Docs
description: Tamamen Azure altyapısında çalışan veya Oracle bulut altyapısı (OCı) ile platformlar arası bağlantı kullanma dahil olmak üzere Microsoft Azure ' de Oracle uygulamalarını ve çözümlerini dağıtmaya yönelik seçenekler hakkında bilgi edinin.
services: virtual-machines-linux
documentationcenter: ''
author: BorisB2015
tags: azure-resource-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/16/2020
ms.author: borisb
ms.openlocfilehash: c737189650d571fb62a770707e84ed15c5a37a57
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81870512"
---
# <a name="overview-of-oracle-applications-and-solutions-on-azure"></a>Azure 'da Oracle uygulamalarına ve çözümlerine genel bakış

Bu makalede, Azure altyapısını kullanarak Oracle çözümlerini çalıştırmaya yönelik yetenekler açıklanır. Ayrıca bkz. Azure Marketi 'nde kullanılabilir [Oracle VM görüntülerine](oracle-vm-solutions.md) yönelik ayrıntılı tanıtımları ve [Oracle bulut altyapısı (OCI) ile Azure bağlantısı](oracle-oci-overview.md)yapma özelliği.

## <a name="oracle-databases-on-azure-infrastructure"></a>Azure altyapısında Oracle veritabanları

Azure Marketi 'nde bulunan Oracle Linux görüntülerde Oracle Database kullanarak Oracle veritabanlarını Azure altyapısında çalıştırın:

* Oracle Database 12,1, 12,2 ve 18,3 Enterprise Edition 

* Oracle Database 12,1, 12,2 ve 18,3 Standard Edition 

Ayrıca, Azure 'da bulunan Oracle Linux olmayan bir görüntüde Oracle Database kurulumunu ve şirket içi ortamınızdan özel bir görüntü yüklemeyi tercih ettiğiniz özel bir görüntüye dayandırın.

İsteğe bağlı olarak birden çok bağlı disk ile yapılandırın ve Oracle otomatik depolama yönetimi (ASM) yükleyerek veritabanı performansını geliştirebilirsiniz.

## <a name="applications-on-oracle-linux-and-weblogic-server"></a>Oracle Linux ve WebLogic Server 'daki uygulamalar

Desteklenen Oracle işletim sistemlerinde Azure 'da kurumsal uygulamaları çalıştırın. Aşağıdaki görüntüler Azure Marketi 'nde mevcuttur:

* Oracle WebLogic Server 12.1.2

* Kırılabilir kurumsal çekirdek (UEK) 6,8, 6,9, 6,10, 7,3, 7,4, 7,5 ve 7,6 ile Oracle Linux 

## <a name="high-availability-and-disaster-recovery-options"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma seçenekleri

* Şirket içinde yüksek kullanılabilirlik için [kullanılabilirlik alanları](../../../availability-zones/az-overview.md) ile [birlikte, Azure](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/admin/sharding-overview.html) altyapısına yönelik fsfo, parçalı veya [altın kapıyla](https://www.oracle.com/middleware/technologies/goldengate.html) birlikte [Oracle Data Guard](https://docs.oracle.com/cd/B19306_01/server.102/b14239/concepts.htm#g1049956), [Active Data Guard](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/dgbkr/index.html)'ı yapılandırın. Ayrıca, bu konfigürasyonları, ek kullanılabilirlik ve olağanüstü durum kurtarma için birden çok Azure bölgesinde da ayarlayabilirsiniz.

* Azure 'da ve şirket içi veya fiziksel sunucularınızdaki Oracle Linux sanal makinelerinize yönelik olağanüstü durum kurtarmayı düzenlemek ve yönetmek için [Azure Site Recovery](../../../site-recovery/site-recovery-overview.md) kullanın. 

* Azure [VMware çözümünü](https://docs.microsoft.com/azure/vmware-cloudsimple/oracle-real-application-clusters/) veya [Flashgrid ufuk kümesini](https://www.flashgrid.io/oracle-rac-in-azure/)kullanarak Azure 'Da Oracle gerçek uygulama kümelerini (RAC) etkinleştirin.

## <a name="backup-oracle-workloads"></a>Oracle Iş yüklerini yedekleme

* [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview) kullanarak Oracle VM 'lerinizi yedekleme

* Oracle Database Oracle RMAN kullanarak yedekleyin ve isteğe bağlı olarak [Azure Blob sigortası](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-mount-container-linux) kullanarak [yüksek oranda Redudant bir Azure Blob depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-redundancy) takın ve ek dayanıklılık için RMAN yedeklemelerinizi bu depoya yazın.

## <a name="integration-of-azure-with-oci"></a>OCı ile Azure tümleştirmesi

Oracle bulut altyapısında (OCı) arka uç veritabanlarına bağlı olan Azure altyapısında Oracle uygulamaları çalıştırın. Bu çözüm aşağıdaki özellikleri kullanır: 

* **Platformlar arası ağ** -uygulama ve veritabanı katmanı arasında yüksek bant genişliğine sahip, özel ve düşük gecikme süreli bağlantılar oluşturmak Için Azure ExpressRoute ve Oracle FastConnect arasında bulunan doğrudan Interconnect 'i kullanın.
* **Tümleşik kimlik** -çözümler için tek bir kimlik kaynağı oluşturmak üzere Azure AD ile Oracle IDCs arasında federasyon kimliği ayarlayın. OCı ve Azure arasında kaynakları yönetmek için çoklu oturum açmayı etkinleştirin.

### <a name="deploy-oracle-applications-on-azure"></a>Azure 'da Oracle uygulamaları dağıtma

Azure altyapısını ayarlamak ve Oracle uygulamalarını yüklemek için Terrayform şablonlarını kullanın. 

> [!IMPORTANT]
> Oracle, bu uygulamaların Azure 'da 2020 Mayıs ' e kadar Azure/Oracle bulut Interconnect çözümünü kullanırken Azure 'da çalışmasını onaylandıracaktır.

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

* Ignite 2019 [' den Oracle 'ın Azure 'a genel bakış oturumuna](https://myignite.techcommunity.microsoft.com/sessions/82915) göz atın. 
