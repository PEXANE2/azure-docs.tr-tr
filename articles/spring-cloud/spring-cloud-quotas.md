---
title: Azure yay bulutu için hizmet planları ve Kotalar
description: Azure Spring Cloud için hizmet kotaları ve hizmet planları hakkında bilgi edinin
author: jpconnock
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 9/27/2019
ms.author: jeconnoc
ms.openlocfilehash: 89934ee0ab0c901a904a1a5ac2fb620185571a23
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038788"
---
# <a name="quotas-and-service-plans-for-azure-spring-cloud"></a>Azure yay bulutu için kotalar ve hizmet planları

Tüm Azure Hizmetleri, kaynaklar ve özellikler için varsayılan limitleri ve kotaları ayarlar.  Önizleme dönemi boyunca Azure yay bulutu yalnızca bir hizmet planı sunar.

Bu makalede, geçerli önizleme döneminde sunulan hizmet kotaları ayrıntılı olarak açıklanır.

## <a name="azure-spring-cloud-service-tiers-and-quotas"></a>Azure yay bulut hizmeti katmanları ve kotaları

Önizleme dönemi boyunca Azure yay bulutu yalnızca bir hizmet katmanı sunar.

Kaynak | Miktar
------- | -------
vCPU | 4
Hafıza | 8 GBayt
Azure yay bulut aboneliği | 1
Abonelik başına bölge başına Azure yay bulut hizmeti örnekleri | 2
Azure yay bulut hizmeti örneği başına toplam uygulama örnekleri | 50
Yay uygulaması başına toplam uygulama örneği sayısı | 20
Kalıcı birimler | 10 x 50 GBayt

Bir kotaya ulaştığınızda, şöyle bir 400 hatası alırsınız: "Kota, *Azure Spring Cloud Service 'in oluşturulduğu*bölge bölgesindeki *aboneliğiniz için* sınırı aşıyor.

## <a name="next-steps"></a>Sonraki adımlar

Bazı varsayılan limitler ve kotalar artırılabilir. Kaynağınız bir artış gerektiriyorsa isteğiniz bize gönderin: azure-spring-cloud@service.microsoft.com.
