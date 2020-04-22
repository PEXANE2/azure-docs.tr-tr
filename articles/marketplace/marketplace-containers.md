---
title: Kapsayıcılar Azure Marketi Için Yayımlama Kılavuzu Sunuyor
description: Bu makalede, Market'te Kapsayıcıların yayımlanması gereken gereksinimler açıklanmaktadır
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: dsindona
ms.openlocfilehash: 80bcf0d5d37a44cee2aab38161abac619542204a
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81684316"
---
# <a name="containers-offer-publishing-guide"></a>Konteynerler Teklif Yayın Kılavuzu

Kapsayıcı, kapsayıcı resminizi Azure Marketi'nde yayımlamanıza yardımcı olur. Bu teklifin gereksinimlerini anlamak için bu kılavuzu kullanın. 

Bunlar, Market aracılığıyla dağıtılan ve faturalanan işlem teklifleridir. Bir kullanıcının gördüğü eylem çağrısı "Şimdi Al" dır.

Çözümünüz Kubernetes tabanlı Azure kapsayıcı hizmeti olarak sağlanan docker kapsayıcı görüntüsü olduğunda Konteyner teklif türünü kullanın.

>[!NOTE]
>Örneğin, Azure Kubernetes Hizmeti veya Azure Kapsayıcı Örnekleri gibi Kubernetes tabanlı bir Azure kapsayıcı hizmeti, Kubernetes tabanlı bir kapsayıcı çalışma zamanı için Azure müşterilerinin seçimi.  

Microsoft şu anda ücretsiz ve kendi lisansını getir (BYOL) lisans modellerini desteklemektedir.

## <a name="containers-offer"></a>Konteynerler Teklif

| Gereksinim | Ayrıntılar |  
|:--- |:--- |  
| Faturalama ve ölçüm | Ücretsiz veya BYOL faturalandırma modelini destekleyin. |  
| Dockerfile'den oluşturulmuş görüntü | Konteyner görüntüleri Docker görüntü belirtimine dayalı olmalı ve dockerdosyasından oluşturulmalıdır.<ul> <li>Docker görüntüleri oluşturma hakkında daha fazla bilgi için [docs.docker.com/engine/reference/builder/#usage'da](https://docs.docker.com/engine/reference/builder/#usage)bulunan Kullanım bölümünü ziyaret edin.</li> </ul> |  
| ACR'de Barındırma | Kapsayıcı görüntüleri bir Azure Kapsayıcı Kayıt Defteri (ACR) deposunda barındırılmalıdır.<ul> <li>ACR ile çalışma hakkında daha fazla bilgi için Quickstart: [docs.microsoft.com/azure/container-registry/container-registry-get-started-portal'da](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-portal)bulunan Azure portal sayfasını kullanarak bir konteyner kayıt defteri oluşturun.</li> </ul> |  
| Resim etiketleme | Kapsayıcı görüntüler en az 1 etiket içermelidir (maksimum etiket: 16).<ul> <li>Bir görüntüyü etiketleme hakkında daha fazla bilgi [için, docs.docker.com/engine/reference/commandline/tag](https://docs.docker.com/engine/reference/commandline/tag)adresinde bulunan docker etiket sayfasını ziyaret edin.</li> </ul> |  

## <a name="next-steps"></a>Sonraki adımlar

Eğer bunu yapmadıysanız, 

- Pazar yeri hakkında [bilgi edinin.](https://azuremarketplace.microsoft.com/sell)

Ortak Merkezi'ne kaydolmak için yeni bir teklif oluşturmaya veya varolan bir teklif üzerinde çalışmaya başlayın:

- Teklifinizi oluşturmak veya tamamlamak için [Partner Center'da oturum açın.](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership)
- Bkz. Daha fazla bilgi için [bir Azure kapsayıcı teklifi oluşturun.](./partner-center-portal/create-azure-container-offer.md)
