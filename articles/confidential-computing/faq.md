---
title: Azure gizli bilgi Işlem hakkında SSS
description: Azure gizli bilgi işlem hakkında sık sorulan soruların yanıtları.
author: JBCook
ms.topic: troubleshooting
ms.workload: infrastructure
ms.service: virtual-machines
ms.subservice: workloads
ms.date: 4/17/2020
ms.author: jencook
ms.openlocfilehash: 2bd9430fc6f48d72faa2c1850af0bb8432a7c5f5
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82149508"
---
# <a name="frequently-asked-questions-for-azure-confidential-computing"></a>Azure gizli bilgi Işlem için sık sorulan sorular

Bu makalede, [Azure 'da gizli bilgi işlem iş yüklerini](overview.md)çalıştırmaya ilişkin en yaygın soruların yanıtları sunulmaktadır.

Azure sorununuz bu makalede giderilmemişse, [MSDN ve Stack Overflow](https://azure.microsoft.com/support/forums/)Azure forumlarını ziyaret edin. Sorununuzu bu forumlara gönderebilir veya [ @AzureSupport Twitter 'da](https://twitter.com/AzureSupport)ilan edebilirsiniz. Ayrıca, bir Azure destek isteği de gönderebilirsiniz. Destek isteği göndermek için [Azure Desteği sayfasında](https://azure.microsoft.com/support/options/)destek al ' ı seçin.

## <a name="confidential-computing-virtual-machines"></a>Gizli Bilgi Işlem sanal makineleri<a id="vm-faq"></a>

1. **DCsv2 serisi VM 'Leri dağıtmaya nasıl başlayabilmeniz gerekir?**

   Bir DCsv2 VM dağıtımı için bazı yollar şunlardır:
   - [Azure Resource Manager şablonu](../virtual-machines/windows/template-description.md) kullanma
   - [Azure Portal](https://portal.azure.com/#create/hub)
   - [Azure gizli bilgi işlem (sanal makine)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-compute.acc-virtual-machine-v2?tab=overview) marketi çözüm şablonunda. Market çözüm şablonu, bir müşteriyi desteklenen senaryolara (bölgeler, görüntüler, kullanılabilirlik, disk şifrelemesi) karşı sınırlandırılmasına yardımcı olur. 

1. **Tüm işletim sistemi görüntüleri Azure gizli bilgi işlem ile çalışacak mı?**

   Hayır. Sanal makineler yalnızca 2. nesil sanal makinelere dağıtılabilir. Ubuntu Server 18,04, Ubuntu Server 16,04 ve Windows Server 2016 Datacenter için 2. nesil destek sunuyoruz. [Linux](../virtual-machines/linux/generation-2.md) ve [Windows](../virtual-machines/windows/generation-2.md) üzerinde gen 2 VM 'ler hakkında daha fazla bilgi edinin

1. **DCsv2 sanal makineler portalda gri renkte bulunur ve bir tane seçemiyorum**

   VM 'nin yanındaki bilgi kabarcığa göre gerçekleştirilecek farklı eylemler vardır:
    -   **Unsupportedgeneration**: sanal makine görüntüsünün neslini "Gen2" olarak değiştirin.
    -   **NotAvailableForSubscription** : bölge, aboneliğiniz için henüz kullanılamıyor. Kullanılabilir bir bölge seçin.
    -   **InsufficientQuota**: [kotayı artırmak için bir destek isteği oluşturun](../azure-portal/supportability/per-vm-quota-requests.md). Ücretsiz deneme aboneliklerinin, gizli bilgi işlem VM 'Leri için kotası yoktur. 

1. **DCsv2 sanal makineler, Portal boyut Seçicisi 'nde arama yapmayı denediğimde gösterilmez**

   Kullanılabilir bir bölge seçtiğinizden emin olun. Ayrıca, boyut seçicisindeki "tüm filtreleri temizle" yi seçtiğinizden emin olun. 

1. **DCsv2 serisi ve DC Serisi VM 'Ler arasındaki fark nedir?**

   DC Serisi VM 'Ler, SGX ile daha eski 6 çekirdekli Intel Işlemcilerde çalışır. Bunlar, daha az toplam belleğe sahiptir, daha az EPC (şifreleme sayfası Kamage) belleğine sahiptir ve daha az bölgede kullanılabilir. Bu VM 'Ler yalnızca ABD Doğu kullanılabilir ve Batı Batı, iki boyutta sunulmaktadır: Standard_DC2s ve Standard_DC4s. Bunlar GA 'ye gitmeyecektir ve yalnızca [gizli Işlem DC-SERIES VM [Önizleme]](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-compute.confidentialcompute?tab=Overview) Market örneğinde dağıtılabilir

1. **DCsv2 sanal makineleri küresel olarak kullanılabilir mi?**

   Hayır, bu sanal makineler yalnızca seçim bölgelerinde kullanılabilir. Kullanılabilir en son bölgeler için [ürünleri bölgelere göre sayfasına](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines) göz atın. 

1. **Açık şifreleme SDK 'sını Nasıl yaparım? mi?**
   
   Azure 'da veya şirket içinde bulunan bir makineye OE SDK 'nın nasıl yükleneceğine ilişkin yönergeler için [Open Enclave SDK GitHub](https://github.com/openenclave/openenclave)' daki yönergeleri izleyin.
     
   Ayrıca, işletim sistemine özgü yükleme yönergeleri için açık şifreleme SDK GitHub ' a da gidebilirsiniz:
     - [Windows 'a OE SDK 'Yı yükler](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/install_oe_sdk-Windows.md)
     - [Ubuntu 18,04 ' ye OE SDK 'Yı kurma](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/install_oe_sdk-Ubuntu_18.04.md)
     - [Ubuntu 16,04 ' ye OE SDK 'Yı kurma](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/install_oe_sdk-Ubuntu_16.04.md)
