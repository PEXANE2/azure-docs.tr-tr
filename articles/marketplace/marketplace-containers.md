---
title: Kapsayıcılar Azure Marketi Için Yayımlama Kılavuzu Sunuyor
description: Bu makalede, Market'te Kapsayıcıların yayımlanması gereken gereksinimler açıklanmaktadır
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: dsindona
ms.openlocfilehash: 882d428d7a8a9b56408aaa68964fa1e36c7cc7c2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80277315"
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

- Pazara [kaydolun.](https://azuremarketplace.microsoft.com/sell)

Kayıtlıysanız ve yeni bir teklif oluşturuyorsanız veya varolan bir teklif üzerinde çalışıyorsanız,

- Teklifinizi oluşturmak veya tamamlamak için [Cloud İş Ortağı Portalı'nda oturum](https://cloudpartner.azure.com) açın.
- Daha fazla bilgi için [Kapsayıcılar'a](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/containers/cpp-containers-offer) bakın.
