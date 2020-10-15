---
title: Azure yay bulutu için hizmet planları ve Kotalar
description: Azure Spring Cloud için hizmet kotaları ve hizmet planları hakkında bilgi edinin
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: 496f2e812a102e85fea92a535552daaaadf5f31e
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92093438"
---
# <a name="quotas-and-service-plans-for-azure-spring-cloud"></a>Azure yay bulutu için kotalar ve hizmet planları

**Bu makale şu şekilde geçerlidir:** ✔️ Java ✔️ C #

Tüm Azure Hizmetleri, kaynaklar ve özellikler için varsayılan limitleri ve kotaları ayarlar.   Azure yay bulutu iki fiyatlandırma katmanı sunar: temel ve standart. Bu makaledeki her iki katmanda da limitleri ayrıntılandıracağız.

## <a name="azure-spring-cloud-service-tiers-and-limits"></a>Azure yay bulut hizmeti katmanları ve limitleri

| Kaynak | Temel | Standart
------- | ------- | -------
Sanal işlemci | hizmet örneği başına 1 | 4 hizmet örneği başına
Bellek | hizmet örneği başına 2 GB | hizmet örneği başına 8 GB
Abonelik başına bölge başına Azure yay bulut hizmeti örnekleri | 10 | 10
Azure yay bulut hizmeti örneği başına toplam uygulama örnekleri | 25 | 500
Kalıcı birimler | 1 GB/uygulama x 10 uygulamaları | 50 GB/uygulama x 10 uygulamaları

## <a name="next-steps"></a>Sonraki adımlar

Bazı varsayılan sınırlar artırılabilir. Kurulum için bir artış gerekiyorsa, [bir destek isteği oluşturun](../azure-portal/supportability/how-to-create-azure-support-request.md).