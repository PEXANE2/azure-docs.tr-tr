---
title: Azure Market 'te kapsayıcı teklifleri için Yayımlama Kılavuzu
description: Bu makalede, Azure Marketi 'nde kapsayıcı tekliflerini yayımlama gereksinimleri açıklanmaktadır.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: dsindona
ms.openlocfilehash: 99aecee930e5d77302ad54babd927588519e33fd
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "82160469"
---
# <a name="publishing-guide-for-container-offers"></a>Kapsayıcı teklifleri için Yayımlama Kılavuzu

Kapsayıcı, kapsayıcı görüntünüzü Azure Market 'Te yayımlamanıza yardımcı olur. Bu teklifin gereksinimlerini anlamak için bu kılavuzu kullanın. 

Kapsayıcı teklifleri, Azure Marketi aracılığıyla dağıtılan ve faturalandırılan işlem teklifleridir. Kullanıcının gördüğü eyleme yapılan çağrı "Şimdi al" dır.

Çözümünüz bir Kubernetes tabanlı Azure Container Service örneği olarak ayarlanan bir Docker kapsayıcı görüntüsü olduğunda *kapsayıcı* teklif türünü kullanın. 

> [!NOTE]
> Kubernetes tabanlı Azure Container Service örneklerinin örnekleri olan Azure Kubernetes hizmeti veya Azure Container Instances, Kubernetes tabanlı bir kapsayıcı çalışma zamanı için Azure müşterilerinin seçimi.  

Microsoft şu anda ücretsiz ve kendi lisansını getir (KLG) lisanslama modellerini desteklemektedir.

## <a name="container-offer-requirements"></a>Kapsayıcı teklif gereksinimleri

| Gereksinim | Ayrıntılar |  
|:--- |:--- |  
| Faturalandırma ve ölçüm | Ücretsiz veya KLG faturalandırma modelini destekler.<br><br> |  
| Dockerfile dosyasından oluşturulan resim | Kapsayıcı görüntülerinin Docker görüntü belirtimine dayalı olması ve Dockerfile 'dan oluşturulması gerekir.<br> <br>Docker görüntülerini oluşturma hakkında daha fazla bilgi için [Dockerfile başvurusunun](https://docs.docker.com/engine/reference/builder/#usage)"kullanım" bölümüne bakın.<br><br> |  
| Azure Container Registry deposunda barındırma | Kapsayıcı görüntülerinin bir Azure Container Registry deposunda barındırılması gerekir.<br> <br>Azure Container Registry ile çalışma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Azure Portal kullanarak özel kapsayıcı kayıt defteri oluşturma](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-portal).<br><br> |  
| Resim etiketleme | Kapsayıcı görüntülerinin en az bir etiket içermesi gerekir (en fazla etiket sayısı: 16).<br><br>Bir görüntüyü etiketleme hakkında daha fazla bilgi için `docker tag` [Docker belgeleri](https://docs.docker.com/engine/reference/commandline/tag) sitesindeki sayfasına bakın.<br><br> |  

## <a name="next-steps"></a>Sonraki adımlar

Henüz yapmadıysanız, [Azure Marketi ile bulut işletmenizi nasıl büyütireceğinizi](https://azuremarketplace.microsoft.com/sell)öğrenin.

Iş Ortağı Merkezi 'nde çalışmaya kaydolmak ve başlamak için:

- Teklifinizi oluşturmak veya tamamlayabilmeniz için [Iş Ortağı Merkezi ' nde oturum açın](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) .
- Daha fazla bilgi için bkz. [Azure Container teklifi oluşturma](./partner-center-portal/create-azure-container-offer.md) .
