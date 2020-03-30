---
title: Sorun giderme Bulut Hizmeti ayırma hatası | Microsoft Dokümanlar
description: Azure’da Cloud Services dağıtımı sırasında oluşan ayırma hatalarını giderme
services: azure-service-management, cloud-services
documentationcenter: ''
author: simonxjx
manager: dcscontentpm
editor: ''
tags: top-support-issue
ms.assetid: 529157eb-e4a1-4388-aa2b-09e8b923af74
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: v-six
ms.openlocfilehash: 470778e5c441bb05ffc7c5e1c5ef97a6c30d3359
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247546"
---
# <a name="troubleshooting-allocation-failure-when-you-deploy-cloud-services-in-azure"></a>Azure’da Cloud Services dağıtımı sırasında oluşan ayırma hatalarını giderme
## <a name="summary"></a>Özet
Örnekleri bir Bulut Hizmetine dağıttığınızda veya yeni web veya çalışan rol örnekleri eklediğinizde, Microsoft Azure hesaplama kaynakları ayırır. Azure abonelik sınırlarına ulaşmadan önce bile bu işlemleri gerçekleştirirken zaman zaman hata lar alabilirsiniz. Bu makalede, bazı ortak ayırma hatalarının nedenleri açıklanmaktadır ve olası düzeltme önerir. Bu bilgiler, hizmetlerinizin dağıtımını planlarken de yararlı olabilir.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

### <a name="background--how-allocation-works"></a>Arka Plan – Ayırma nasıl çalışır?
Azure veri merkezlerindeki sunucular kümelere bölünmüştür. Birden çok kümede yeni bir bulut hizmeti ayırma isteği denenir. İlk örnek bir bulut hizmetine (evreleme veya üretimde) dağıtıldığında, bu bulut hizmeti bir kümeye sabitlenir. Bulut hizmeti için başka dağıtımlar da aynı kümede gerçekleşir. Bu makalede, buna "kümeye sabitlenmiş" olarak atıfta bulunacağız. Aşağıdaki diyagram 1, birden çok kümede denenen normal bir ayırma durumunu göstermektedir; Diyagram 2, varolan Bulut Hizmeti CS_1 barındırıldığı yer olduğundan Cluster 2'ye sabitlenmiş bir ayırma durumunu gösterir.

![Tahsisat Diyagramı](./media/cloud-services-allocation-failure/Allocation1.png)

### <a name="why-allocation-failure-happens"></a>Ayırma hatası neden olur?
Ayırma isteği bir kümeye sabitlendiğinde, kullanılabilir kaynak havuzu kümeyle sınırlı olduğundan, boş kaynak bulamama olasılığı daha yüksektir. Ayrıca, ayırma isteğiniz bir kümeye sabitlenmişse, ancak istediğiniz kaynak türü bu küme tarafından desteklenmiyorsa, kümede boş kaynak olsa bile isteğiniz başarısız olur. Aşağıdaki diyagram 3, tek aday kümenin boş kaynakları olmadığından sabitlenmiş bir ayırmanın başarısız olduğu durumu göstermektedir. Diyagram 4, kümenin boş kaynakları olmasına rağmen tek aday küme istenen VM boyutunu desteklemediği için sabitlenmiş bir ayırmanın başarısız olduğu durumu gösterir.

![Sabitlenmiş Ayırma Hatası](./media/cloud-services-allocation-failure/Allocation2.png)

## <a name="troubleshooting-allocation-failure-for-cloud-services"></a>Bulut hizmetleri için sorun giderme giderme hatası
### <a name="error-message"></a>Hata İletisi
Aşağıdaki hata iletisini görebilirsiniz:

    "Azure operation '{operation id}' failed with code Compute.ConstrainedAllocationFailed. Details: Allocation failed; unable to satisfy constraints in request. The requested new service deployment is bound to an Affinity Group, or it targets a Virtual Network, or there is an existing deployment under this hosted service. Any of these conditions constrains the new deployment to specific Azure resources. Please retry later or try reducing the VM size or number of role instances. Alternatively, if possible, remove the aforementioned constraints or try deploying to a different region."

### <a name="common-issues"></a>Genel Sorunlar
Ayırma isteğinin tek bir kümeye sabitlenebedilmesine neden olan yaygın ayırma senaryoları aşağıda verilmiştir.

* Evreleme Yuvasına Dağıtım - Bir bulut hizmetinin her iki yuvada da dağıtımı varsa, tüm bulut hizmeti belirli bir kümeye sabitlenir.  Diğer bir deyişle, üretim yuvasında bir dağıtım zaten varsa, yeni bir hazırlama dağıtımı yalnızca üretim yuvası ile aynı kümede ayrılabilir. Küme kapasiteye yaklaşıyorsa, istek başarısız olabilir.
* Ölçeklendirme - Mevcut bir bulut hizmetine yeni örnekler eklendiğinde aynı kümede ayrılmalıdır.  Küçük ölçeklendirme istekleri her zaman olmasa da genellikle ayrılabilir. Küme kapasiteye yaklaşıyorsa, istek başarısız olabilir.
* Affinity Group - Bulut hizmeti bir yakınlık grubuna sabitlenmedikçe, boş bir bulut hizmetine yeni bir dağıtım, o bölgedeki herhangi bir kümedeki kumaş tarafından tahsis edilebilir. Aynı yakınlık grubuna dağıtımlar aynı kümede denenir. Küme kapasiteye yaklaşıyorsa, istek başarısız olabilir.
* Affinity Group vNet - Eski Sanal Ağlar bölgeler yerine yakınlık gruplarına bağlıydı ve bu Sanal Ağlardaki bulut hizmetleri yakınlık grubu kümesine sabitlenecekti. Bu tür sanal ağa dağıtımlar sabitlenmiş kümede denenir. Küme kapasiteye yaklaşıyorsa, istek başarısız olabilir.

## <a name="solutions"></a>Çözümler
1. Yeni bir bulut hizmetine yeniden dağıtın - Platformun o bölgedeki tüm kümeler arasından seçim yapmasına olanak sağladığından, bu çözüm büyük olasılıkla en başarılı olacaktır.

   * İş yükünü yeni bir bulut hizmetine dağıtma  
   * Trafiği yeni bulut hizmetine yönlendirecek Şekilde CNAME veya A kaydını güncelleştirme
   * Eski siteye sıfır trafik ulaştıktan sonra, eski bulut hizmetini silebilirsiniz. Bu çözüm sıfır kapalı kalma süresine neden olmalıdır.
2. Hem üretim hem de evreleme yuvalarını silin - Bu çözüm varolan DNS adınızı korur, ancak uygulamanızda kapalı kalma süresine neden olur.

   * Bulut hizmetinin boş olması için varolan bir bulut hizmetinin üretim ve evreleme yuvalarını silme
   * Varolan bulut hizmetinde yeni bir dağıtım oluşturun. Bu, bölgedeki tüm kümeleri ayırmaya yeniden çalışır. Bulut hizmetinin bir yakınlık grubuna bağlı olmadığından emin olun.
3. Ayrılmış IP - Bu çözüm varolan IP adresinizi korur, ancak uygulamanızda kapalı kalma süresine neden olur.  

   * Powershell'i kullanarak mevcut dağıtımınız için bir ReservedIP oluşturma

     ```
     New-AzureReservedIP -ReservedIPName {new reserved IP name} -Location {location} -ServiceName {existing service name}
     ```
   * Hizmetin CSCFG'sinde yeni ReservedIP'yi belirttiğinizden emin olmak için yukarıdan #2 izleyin.
4. Yeni dağıtımlar için yakınlık grubunu kaldırın - Yakınlık Grupları artık önerilmez. Yeni bir bulut hizmeti dağıtmak için yukarıdaki 1 numaralı öğeye yönelik adımları izleyin. Bulut hizmetinin bir yakınlık grubunda olmadığından emin olun.
5. Bölgesel Sanal Ağa Dönüştürün - [Affinity Groups'tan Bölgesel Sanal Ağa (VNet) nasıl geçirilir.](../virtual-network/virtual-networks-migrate-to-regional-vnet.md)
