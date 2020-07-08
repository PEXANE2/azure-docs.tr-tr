---
title: Azure yay bulutu için hizmet planları ve Kotalar
description: Azure Spring Cloud için hizmet kotaları ve hizmet planları hakkında bilgi edinin
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: brendm
ms.openlocfilehash: dc4426fc08efff44bc46ba37eb6fbc6fe31f356b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84735447"
---
# <a name="quotas-and-service-plans-for-azure-spring-cloud"></a>Azure yay bulutu için kotalar ve hizmet planları

Tüm Azure Hizmetleri, kaynaklar ve özellikler için varsayılan limitleri ve kotaları ayarlar.   Azure yay bulutu iki fiyatlandırma katmanı sunar: temel ve standart. Bu makaledeki her iki katmanda da limitleri ayrıntılandıracağız.

## <a name="azure-spring-cloud-service-tiers-and-limits"></a>Azure yay bulut hizmeti katmanları ve limitleri

| Kaynak | Temel | Standart
------- | ------- | -------
Sanal işlemci | hizmet örneği başına 1 | 4 hizmet örneği başına
Bellek | hizmet örneği başına 2 GB | hizmet örneği başına 8 GB
Abonelik başına bölge başına Azure yay bulut hizmeti örnekleri | 10 | 10
Azure yay bulut hizmeti örneği başına toplam uygulama örnekleri | 25 | 500
Kalıcı birimler | 1 GB/uygulama x 10 uygulamaları | 50 GB/uygulama x 10 uygulamaları


Önizleme dönemi boyunca Azure yay bulutu yalnızca bir hizmet katmanı sunar. Bir sınıra ulaştığınızda, şöyle bir 400 hatası alırsınız: "Kota, *Azure Spring Cloud Service 'in oluşturulduğu*bölge bölgesindeki *aboneliğiniz için* sınırı aşıyor.

## <a name="next-steps"></a>Sonraki adımlar

Bazı varsayılan sınırlar artırılabilir. Kurulum için bir artış gerekiyorsa, [bir destek isteği oluşturun](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request).
