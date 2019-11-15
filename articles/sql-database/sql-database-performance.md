---
title: Performansı izleme ve iyileştirme
description: Azure SQL veritabanı, geçerli sorgu performansını iyileştirebilecek olan bölgeleri belirlemenize yardımcı olacak performans araçları sağlar.
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
ms.date: 11/14/2019
ms.openlocfilehash: c710e2b7c4376fc1725032c86cd02d9a6c8cafac
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2019
ms.locfileid: "74090342"
---
# <a name="monitor-and-improve-performance"></a>Performansı izleme ve iyileştirme

Azure SQL veritabanı, veritabanınızdaki olası sorunları tanımlar ve akıllı ayarlama eylemleri ve önerileri sağlayarak iş yükünüzün performansını iyileştirebilecek eylemler önerir.

## <a name="performance-tuning-options"></a>Performans ayarlama seçenekleri

Performans ayarlama seçenekleri kullanılabilir Azure SQL veritabanı, "akıllı performans" altındaki veritabanı gezinti menüsünde bulunabilir:

| Performans ayarlama seçeneği | Tek veritabanı ve havuza alınmış veritabanı desteği | Örnek veritabanı desteği |
| :----------------------------- | ----- | ----- |
| **[Performansa genel bakış](#performance-overview)** -veritabanınız için tüm performans etkinliklerini izleyin. | Yes | Hayır | 
| **[Performans önerileri](#performance-recommendations)** -iş yükünüzün performansını iyileştirebilecek performans önerilerini gösterir. | Yes | Hayır | 
| **[Sorgu performansı içgörüleri](#query-performance-insight)** -veritabanında en çok kullanılan sorguların performansını gösterir. | Yes | Hayır | 
| **[Otomatik ayarlama](#automatic-tuning)** -veritabanı performansınızı otomatik olarak Iyileştirmek IÇIN Azure SQL veritabanı 'nı kullanın. | Yes | Hayır | 

## <a name="performance-overview"></a>Performansa genel bakış

Bu görünüm, veritabanı Performanslarınızın bir özetini sağlar ve performans ayarlama ve sorun giderme konusunda size yardımcı olur. 

![Performans](./media/sql-database-performance/performance.png)

* **Öneriler** kutucuğu, veritabanınız için ayarlama önerilerinin dökümünü sağlar (daha fazla varsa ilk üç öneri gösterilir). Bu Kutucuğa tıkladığınızda **[performans önerilerine](#performance-recommendations)** gidersiniz. 
* **Ayarlama etkinliği** kutucuğu, veritabanınız için devam eden ve tamamlanmış ayarlama eylemlerinin bir özetini sağlar ve bu sayede ayarlama etkinliğinin geçmişine hızlı bir görünüm sağlar. Bu Kutucuğa tıkladığınızda veritabanınız için tam ayarlama geçmişi görünümüne gidersiniz.
* **Otomatik ayarlama** kutucuğu veritabanınız için [otomatik ayarlama yapılandırmasını](sql-database-automatic-tuning-enable.md) gösterir (veritabanınıza otomatik olarak uygulanan ayarlama seçenekleri). Bu Kutucuğa tıkladığınızda Otomasyon yapılandırma iletişim kutusu açılır.
* **Veritabanı sorguları** kutucuğu, veritabanınızın sorgu performansının özetini gösterir (genel DTU kullanımı ve en üstteki kaynak kullanan sorgular). Bu Kutucuğa tıkladığınızda **[sorgu performansı içgörüleri](#query-performance-insight)** için gidersiniz.

## <a name="performance-recommendations"></a>Performans önerileri

Bu sayfa, veritabanınızın performansını iyileştirebilecek akıllı [ayarlama önerileri](sql-database-advisor.md) sağlar. Bu sayfada aşağıdaki tür öneriler gösterilmektedir:

* Oluşturulacak veya bırakılacak dizinlere ilişkin öneriler.
* Veritabanında şema sorunlarının belirlenme önerileri.
* Sorguların parametreli sorgulardan faydalanabileceği öneriler.

![Performans](./media/sql-database-performance/recommendations.png)

Geçmişte uygulanan ayarlama eylemlerinin tamamlanma geçmişini de bulabilirsiniz.

[Performans önerilerini bul ve Uygula](sql-database-advisor-portal.md) makalesindeki performans önerilerini nasıl bulacağınızı öğrenin.

## <a name="query-performance-insight"></a>Sorgu Performansı İçgörüleri

[Sorgu performansı içgörüleri](sql-database-query-performance.md) , aşağıdakileri sağlayarak veritabanı performansını sorun gidermeye daha az zaman harcamanızı sağlar:

* Veritabanları kaynağı (DTU) tüketiminiz hakkında daha derin Öngörüler. 
* Daha iyi performans için ayarlanabilir olabilecek, en üstteki CPU kullanan sorgular. 
* Bir sorgunun ayrıntılarına gitme olanağı. 

  ![Performans panosu](./media/sql-database-query-performance/performance.png)

Bu sayfa hakkında daha fazla bilgi edinmek **[için sorgu performansı içgörüleri kullanma makalesine gidin](sql-database-query-performance.md)** .

## <a name="automatic-tuning"></a>Otomatik ayarlama

Azure SQL veritabanları, [performans önerileri](sql-database-advisor.md)uygulayarak veritabanı performansını otomatik olarak ayarlayabilir. Daha fazla bilgi edinmek için [otomatik ayarlama makalesini](sql-database-automatic-tuning.md)okuyun. Etkinleştirmek için [otomatik ayarlamayı etkinleştirme](sql-database-automatic-tuning-enable.md)konusunu okuyun.

## <a name="additional-resources"></a>Ek kaynaklar

* [Tek veritabanları için Azure SQL veritabanı performans Kılavuzu](sql-database-performance-guidance.md)
* [Elastik havuz ne zaman kullanılmalıdır?](sql-database-elastic-pool-guidance.md)
