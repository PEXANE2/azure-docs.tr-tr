---
title: Azure yay bulutu için hizmet planları ve Kotalar
description: Azure Spring Cloud için hizmet kotaları ve hizmet planları hakkında bilgi edinin
author: jpconnock
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: jeconnoc
ms.openlocfilehash: 0518b13ea228b4834a095a9bf126b131e70a5f45
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74851562"
---
# <a name="quotas-and-service-plans-for-azure-spring-cloud"></a>Azure yay bulutu için kotalar ve hizmet planları

Tüm Azure Hizmetleri, kaynaklar ve özellikler için varsayılan limitleri ve kotaları ayarlar.  Önizleme dönemi boyunca Azure yay bulutu yalnızca bir hizmet planı sunar.

Bu makalede, geçerli önizleme döneminde sunulan hizmet kotaları ayrıntılı olarak açıklanır.

## <a name="azure-spring-cloud-service-tiers-and-quotas"></a>Azure yay bulut hizmeti katmanları ve kotaları

Önizleme dönemi boyunca Azure yay bulutu yalnızca bir hizmet katmanı sunar.

Kaynak | Miktar
------- | -------
vCPU | 4 hizmet örneği başına
Hafıza | hizmet örneği başına 8 GB
Abonelik başına bölge başına Azure yay bulut hizmeti örnekleri | 10
Azure yay bulut hizmeti örneği başına toplam uygulama örnekleri | 500
Yay uygulaması başına toplam uygulama örneği sayısı | 20
Kalıcı birimler | 10 x 50 GBayt

Bir kotaya ulaştığınızda, şöyle bir 400 hatası alırsınız: "Kota, *Azure Spring Cloud Service 'in oluşturulduğu*bölge bölgesindeki *aboneliğiniz için* sınırı aşıyor.

## <a name="next-steps"></a>Sonraki adımlar

Bazı varsayılan limitler ve kotalar artırılabilir. Kaynağınız bir artış gerektiriyorsa, [bir destek isteği oluşturun](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request).
