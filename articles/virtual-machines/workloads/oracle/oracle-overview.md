---
title: Microsoft Azure'da Oracle çözümleri | Microsoft Dokümanlar
description: Tamamen Azure altyapısında çalışmak veya Oracle Cloud Infrastructure (OCI) ile bulutlar arası bağlantı kullanmak da dahil olmak üzere Oracle Applications'ı ve çözümlerini Microsoft Azure'da dağıtma seçenekleri hakkında bilgi edinin.
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
ms.date: 03/16/2020
ms.author: rogirdh
ms.openlocfilehash: 101cfc9edce8f4df1ad2388c08a5bd9702dffe68
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878230"
---
# <a name="overview-of-oracle-applications-and-solutions-on-azure"></a>Azure'da Oracle Uygulamalarına ve çözümlerine genel bakış

Bu makalede, Azure altyapısını kullanarak Oracle çözümlerini çalıştırmak için yetenekler tanıtıştır. Azure Marketi'ndeki mevcut [Oracle VM görüntülerinin](oracle-vm-solutions.md) ayrıntılı tanıtımlarına ve [Azure'u Oracle Cloud Infrastructure (OCI) ile birbirine bağlama](oracle-oci-overview.md)özelliğine de bakın.

## <a name="oracle-databases-on-azure-infrastructure"></a>Azure altyapısında Oracle veritabanları

Azure Marketi'nde bulunan Oracle Linux görüntülerinde Oracle Veritabanı'nı kullanarak Azure altyapısında Oracle veritabanlarını çalıştırın:

* Oracle Veritabanı 12.1, 12.2 ve 18.3 Enterprise Edition 

* Oracle Veritabanı 12.1, 12.2 ve 18.3 Standart Sürüm 

Oracle Veritabanı'nı Azure'da bulunan Oracle Linux olmayan bir görüntüye kurmayı, bir çözümü Azure'da sıfırdan oluşturduğunuz özel bir görüntüye dayandırmayı veya şirket içi ortamınızdan özel bir resim yüklemeyi de seçebilirsiniz.

İsteğe bağlı olarak birden çok bağlı diskle yapılandırın ve Oracle Otomatik Depolama Yönetimi'ni (ASM) yükleyerek veritabanı performansını artırın.

## <a name="applications-on-oracle-linux-and-weblogic-server"></a>Oracle Linux ve WebLogic Server uygulamaları

Desteklenen Oracle işletim sistemlerinde Azure'da kurumsal uygulamalar çalıştırın. Aşağıdaki resimler Azure Marketi'nde kullanılabilir:

* Oracle WebLogic Server 12.1.2

* Unbreakable Enterprise Çekirdek (UEK) 6.8, 6.9, 6.10, 7.3, 7.4, 7.5 ve 7.6 ile Oracle Linux 

## <a name="high-availability-and-disaster-recovery-options"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma seçenekleri

* Oracle [Data Guard,](https://docs.oracle.com/cd/B19306_01/server.102/b14239/concepts.htm#g1049956) [Active Data Guard ile FSFO,](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/dgbkr/index.html) [Sharding](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/admin/sharding-overview.html) veya [Golden Gate'i](https://www.oracle.com/middleware/technologies/goldengate.html) Azure altyapısında, bölge içinde yüksek kullanılabilirlik için [Kullanılabilirlik Bölgeleri](../../../availability-zones/az-overview.md) ile birlikte yapılandırın. Daha fazla kullanılabilirlik ve olağanüstü durum kurtarma için bu yapılandırmaları birden çok Azure bölgesinde de ayarlayabilirsiniz.

* Azure'daki Oracle Linux VM'leriniz ve şirket içi veya fiziksel sunucularınız için olağanüstü durum kurtarmayı düzenlemek ve yönetmek için [Azure Site Kurtarma'yı](../../../site-recovery/site-recovery-overview.md) kullanın. 

* [Azure VMWare Solution](https://docs.azure.cloudsimple.com/oracle-rac/) veya [FlashGrid SkyCluster](https://www.flashgrid.io/oracle-rac-in-azure/)kullanarak Azure'da Oracle Gerçek Uygulama Kümelerini (RAC) etkinleştirin.

## <a name="backup-oracle-workloads"></a>Yedekleme Oracle İş Yükleri

* [Azure Yedekleme'yi](https://docs.microsoft.com/azure/backup/backup-overview) kullanarak Oracle VM'lerinizi yedekle

* Oracle RMAN'ı kullanarak Oracle Veritabanınızı yedekleyin ve isteğe bağlı olarak [Azure Blob Fuse'u](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-mount-container-linux) kullanarak [yüksek oranda yenibir Azure Blob Depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-redundancy) açın ve daha fazla esneklik için RMAN yedeklemelerinizi bu hesaba yazın.

## <a name="integration-of-azure-with-oci"></a>Azure'un OCI ile entegrasyonu

Oracle Cloud Infrastructure (OCI) arka uç veritabanlarına bağlı Olan Azure altyapısında Oracle Applications çalıştırın. Bu çözüm aşağıdaki yetenekleri kullanır: 

* **Çapraz bulut ağı** - Uygulama ve veritabanı katmanı arasında yüksek bant genişliği, özel ve düşük gecikme süresi bağlantıları kurmak için Azure ExpressRoute ve Oracle FastConnect arasında kullanılabilen doğrudan ara bağlantıyı kullanın.
* **Tümleşik kimlik** - Çözümler için tek bir kimlik kaynağı oluşturmak için Azure AD ve Oracle IDCS arasında federe kimlik ayarlayın. OCI ve Azure'daki kaynakları yönetmek için tek oturum açma'yı etkinleştirin.

### <a name="deploy-oracle-applications-on-azure"></a>Oracle Uygulamalarını Azure'da dağıtma

Azure altyapısını kurmak ve Oracle Applications'ı yüklemek için Terraform şablonlarını kullanın. 

> [!IMPORTANT]
> Oracle, Azure / Oracle Cloud ara bağlantı çözümünü Mayıs 2020'ye kadar kullanırken bu uygulamaları Azure'da çalışacak şekilde onaylar.

* E-İş Paketi
* JD Edwards EnterpriseOne
* Peoplesoft
* Oracle Perakende uygulamaları
* Oracle Hyperion Finansal Yönetimi

Ayrıca, Azure'da OCI ve diğer Azure hizmetlerine bağlanan özel uygulamaları da dağıtın.

### <a name="set-up-oracle-databases-in-oci"></a>OCI'de Oracle veritabanlarını ayarlama

Azure'da çalışan Oracle Applications ile birlikte Oracle Veritabanı Bulut Hizmetleri'ni (Özerk Veritabanı, RAC, Exadata, DBaaS, Tek Düğüm) kullanın. [OCI veritabanı seçenekleri](https://docs.cloud.oracle.com/iaas/Content/Database/Concepts/databaseoverview.htm)hakkında daha fazla bilgi edinin. 
 

## <a name="licensing"></a>Lisanslama

Oracle Applications'ın Azure'da dağıtımı bir "kendi lisansınızı getirin" modeline dayanır. Oracle yazılımlarını kullanmak için uygun lisansa sahip olduğunuz ve Oracle ile geçerli bir destek anlaşmanız olduğu varsayılır. Oracle, şirket içi azure'a lisans hareketliliğini garanti etmiştir. Oracle-Azure [SSS'ye](https://www.oracle.com/cloud/technologies/oracle-azure-faq.html)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Oracle VM görüntülerini](oracle-vm-solutions.md) Azure altyapısında dağıtma hakkında daha fazla bilgi edinin.

* Azure ile [OCI arasında](oracle-oci-overview.md)bağlantı kurma hakkında daha fazla bilgi edinin.

* Ignite 2019'daki [Azure genel bakış oturumuna](https://myignite.techcommunity.microsoft.com/sessions/82915) göz atın. 