---
title: Azure yay bulutu için hizmet planları ve Kotalar
description: Azure Spring Cloud için hizmet kotaları ve hizmet planları hakkında bilgi edinin
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: brendm
ms.openlocfilehash: 8a7ba3c3b9c19b2084b6892b55ac417da38ab047
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76278885"
---
# <a name="quotas-and-service-plans-for-azure-spring-cloud"></a>Azure yay bulutu için kotalar ve hizmet planları

Tüm Azure Hizmetleri, kaynaklar ve özellikler için varsayılan limitleri ve kotaları ayarlar.  Önizleme dönemi boyunca Azure yay bulutu yalnızca bir hizmet planı sunar.

Bu makalede, geçerli önizleme döneminde sunulan hizmet kotaları ayrıntılı olarak açıklanır.

## <a name="azure-spring-cloud-service-tiers-and-quotas"></a>Azure yay bulut hizmeti katmanları ve kotaları

Önizleme dönemi boyunca Azure yay bulutu yalnızca bir hizmet katmanı sunar.

Kaynak | Miktar
------- | -------
Sanal işlemci | 4 hizmet örneği başına
Bellek | hizmet örneği başına 8 GB
Abonelik başına bölge başına Azure yay bulut hizmeti örnekleri | 10
Azure yay bulut hizmeti örneği başına toplam uygulama örnekleri | 500
Yay uygulaması başına toplam uygulama örneği sayısı | 20
Kalıcı birimler | 10 x 50 GBayt

Bir kotaya ulaştığınızda, şöyle bir 400 hatası alırsınız: "Kota, *Azure Spring Cloud Service 'in oluşturulduğu*bölge bölgesindeki *aboneliğiniz için* sınırı aşıyor.

## <a name="next-steps"></a>Sonraki adımlar

Bazı varsayılan limitler ve kotalar artırılabilir. Kaynağınız bir artış gerektiriyorsa, [bir destek isteği oluşturun](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request).
