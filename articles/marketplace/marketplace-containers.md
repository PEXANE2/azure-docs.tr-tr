---
title: Azure Market 'te kapsayıcı teklifleri için Yayımlama Kılavuzu
description: Bu makalede, Azure Marketi 'nde kapsayıcı tekliflerini yayımlama gereksinimleri açıklanmaktadır.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 09/04/2020
ms.openlocfilehash: c52fabcfc2ff22df2de6dd93f2543d625310baef
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89484350"
---
# <a name="publishing-guide-for-container-offers"></a>Kapsayıcı teklifleri için Yayımlama Kılavuzu

Kapsayıcı, kapsayıcı görüntünüzü Azure Market 'Te yayımlamanıza yardımcı olur. Bu teklifin gereksinimlerini anlamak için bu kılavuzu kullanın. 

Kapsayıcı teklifleri, Azure Marketi aracılığıyla dağıtılan ve faturalandırılan işlem teklifleridir. Kullanıcının gördüğü listeleme seçeneği "Şimdi al" dır.

Çözümünüz bir Kubernetes tabanlı Azure Container Service örneği olarak ayarlanan bir Docker kapsayıcı görüntüsü olduğunda *kapsayıcı* teklif türünü kullanın. 

> [!NOTE]
> Kubernetes tabanlı Azure Container Service örneklerinin örnekleri olan Azure Kubernetes hizmeti veya Azure Container Instances, Kubernetes tabanlı bir kapsayıcı çalışma zamanı için Azure müşterilerinin seçimi.  

Microsoft şu anda ücretsiz ve kendi lisansını getir (KLG) lisanslama modellerini desteklemektedir.

## <a name="container-offer-requirements"></a>Kapsayıcı teklif gereksinimleri

| Gereksinim | Ayrıntılar |  
|:--- |:--- |  
| Faturalandırma ve ölçüm | Ücretsiz veya KLG faturalandırma modelini destekler.<br><br> |  
| Dockerfile dosyasından oluşturulan resim | Kapsayıcı görüntülerinin Docker görüntü belirtimine dayalı olması ve Dockerfile 'dan oluşturulması gerekir.<br> <br>Docker görüntülerini oluşturma hakkında daha fazla bilgi için [Dockerfile başvurusunun](https://docs.docker.com/engine/reference/builder/#usage)"kullanım" bölümüne bakın.<br><br> |  
| Azure Container Registry deposunda barındırma | Kapsayıcı görüntülerinin bir Azure Container Registry deposunda barındırılması gerekir.<br> <br>Azure Container Registry ile çalışma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Azure Portal kullanarak özel kapsayıcı kayıt defteri oluşturma](../container-registry/container-registry-get-started-portal.md).<br><br> |  
| Görüntü etiketleme | Kapsayıcı görüntülerinin en az bir etiket içermesi gerekir (en fazla etiket sayısı: 16).<br><br>Bir görüntüyü etiketleme hakkında daha fazla bilgi için `docker tag` [Docker belgeleri](https://docs.docker.com/engine/reference/commandline/tag) sitesindeki sayfasına bakın.<br><br> |  

## <a name="next-steps"></a>Sonraki adımlar

Henüz yapmadıysanız, [Azure Marketi ile bulut işletmenizi nasıl büyütireceğinizi](https://azuremarketplace.microsoft.com/sell)öğrenin.

Iş Ortağı Merkezi 'nde çalışmaya kaydolmak ve başlamak için:

- Teklifinizi oluşturmak veya tamamlayabilmeniz için [Iş Ortağı Merkezi ' nde oturum açın](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) .
- Daha fazla bilgi için bkz. [Azure Container teklifi oluşturma](./partner-center-portal/create-azure-container-offer.md) .
