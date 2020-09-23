---
title: PostgreSQL için Azure veritabanı 'na genel bakış-hiper ölçek (Citus)
description: Hyperscale (Citus) dağıtım seçeneğine genel bakış sağlar
author: jonels-msft
ms.author: jonels
ms.custom: mvc
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: overview
ms.date: 09/01/2020
ms.openlocfilehash: e62d1bf0e9db5e80193cb0615d0a9d31e3041d63
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948067"
---
# <a name="what-is-azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL için Azure veritabanı-hiper ölçek (Citus) nedir?

PostgreSQL için Azure veritabanı, geliştiriciler için derlenmiş Microsoft bulutundaki bir ilişkisel veritabanı hizmetidir. Bu, açık kaynak [PostgreSQL](https://www.postgresql.org/) veritabanı altyapısının topluluk sürümünü temel alır.

Hiper ölçek (Citus), parçaları kullanarak birden çok makine genelinde sorguları yatay olarak ölçeklendirerek kullanılan bir dağıtım seçeneğidir. Sorgu altyapısı, büyük veri kümelerinde daha hızlı yanıtlar için bu sunucular genelinde gelen SQL sorgularını paralelleştiriyor. Diğer dağıtım seçeneklerinden daha fazla ölçek ve performans gerektiren uygulamalar sunar: genellikle yaklaştığı veya zaten aşmış--100 GB veri olan iş yükleri.

Hiper ölçek (Citus) şunları sağlar:

- Parçalara ayırma kullanılarak birden çok makine arasında yatay ölçeklendirme
- Büyük veri kümelerinde daha hızlı yanıtlar için bu sunucular genelinde paralelleştirme sorgula
- Çok kiracılı uygulamalar, gerçek zamanlı operasyonel çözümlemeler ve yüksek verimlilik işlem iş yükleri için mükemmel destek

PostgreSQL için derlenmiş uygulamalar, standart [bağlantı kitaplıkları](./concepts-connection-libraries.md) ve en az değişiklikle hiper ölçekte (Citus) dağıtılmış sorgular çalıştırabilir.

## <a name="next-steps"></a>Sonraki adımlar

- İlk PostgreSQL için Azure veritabanı-hiper ölçek (Citus) sunucu grubu [oluşturarak](./quickstart-create-hyperscale-portal.md) başlayın.
- Maliyet karşılaştırmaları ve hesaplayıcıları için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/postgresql/) bakın. Hiper ölçek (Citus) ön ödemeli ayrılmış örnek indirimleri de sunar, Ayrıntılar için bkz. [HYPERSCALE RI fiyatlandırma](concepts-hyperscale-reserved-pricing.md) sayfaları.
- Sunucu grubunuz için en iyi [ilk boyutu](howto-hyperscale-scaling.md#picking-initial-size) belirleme
