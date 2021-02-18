---
title: Azure yay bulutu için hizmet planları ve Kotalar
description: Azure Spring Cloud için hizmet kotaları ve hizmet planları hakkında bilgi edinin
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: 20ebeb23fe09ba4fd70a724828afadfaa3901abd
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "101095673"
---
# <a name="quotas-and-service-plans-for-azure-spring-cloud"></a>Azure yay bulutu için kotalar ve hizmet planları

**Bu makale şu şekilde geçerlidir:** ✔️ Java ✔️ C #

Tüm Azure Hizmetleri, kaynaklar ve özellikler için varsayılan limitleri ve kotaları ayarlar.   Azure yay bulutu iki fiyatlandırma katmanı sunar: temel ve standart. Bu makaledeki her iki katmanda da limitleri ayrıntılandıracağız.

## <a name="azure-spring-cloud-service-tiers-and-limits"></a>Azure yay bulut hizmeti katmanları ve limitleri

| Kaynak | Kapsam | Temel | Standart
------- | ------- | -------
Sanal işlemci | uygulama örneği başına | 1 | 4
Bellek | uygulama örneği başına | 2 GB | 8 GB
Azure yay bulut hizmeti örnekleri | Abonelik başına bölge başına | 10 | 10
Toplam uygulama örnekleri | Azure yay bulut hizmeti örneği başına | 25 | 500
Özel Etki Alanları | Azure yay bulut hizmeti örneği başına | 0 | 25 
Kalıcı birimler | Azure yay bulut hizmeti örneği başına | 1 GB/uygulama x 10 uygulamaları | 50 GB/uygulama x 10 uygulamaları

> [!TIP]
> Hizmet örneği başına toplam uygulama örnekleri için listelenen ücretler, uygulamalar/dağıtımlar için durdurulmuş durumda uygulanır. Lütfen kullanımda olan uygulamaları/dağıtımları silin.

## <a name="next-steps"></a>Sonraki adımlar

Bazı varsayılan sınırlar artırılabilir. Kurulum için bir artış gerekiyorsa, [bir destek isteği oluşturun](../azure-portal/supportability/how-to-create-azure-support-request.md).
