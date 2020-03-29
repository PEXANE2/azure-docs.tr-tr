---
title: Sorgu Performans Öngörüsü - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, PostgreSQL - Single Server için Azure Veritabanı'ndaki Sorgu Performans Öngörüsü özelliği açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 08/21/2019
ms.openlocfilehash: dd5b4ec53d82421ddd9d680ca41e48eeecc43c2c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74768393"
---
# <a name="query-performance-insight"></a>Sorgu Performansı İçgörüleri 

**Aşağıdakiler için geçerlidir:** PostgreSQL için Azure Veritabanı - Tek Sunucu sürümleri 9.6, 10, 11

Sorgu Performans Öngörüsü, en uzun süre çalışan sorgularınızın ne olduğunu, zaman içinde nasıl değiştiğini ve hangi beklemelerin onları etkilediğini hızlı bir şekilde belirlemenize yardımcı olur.

## <a name="permissions"></a>İzinler
Sorgu Performansı İçgörüleri’ndeki metni görünüm için **Sahip** veya **Katkıda bulunan** izinleri gereklidir. **Okuyucu**, grafikleri ve tabloları görüntüleyebilir ancak metni sorgulayamaz.

## <a name="prerequisites"></a>Ön koşullar
Sorgu Performans Öngörüsünc'ün çalışması için Sorgu [Deposu'nda](concepts-query-store.md)veri bulunması gerekir.

## <a name="viewing-performance-insights"></a>Performans öngörülerini görüntüleme
Azure portaldaki [Sorgu Performansı İçgörüleri](concepts-query-performance-insight.md) görünümü, Query Store’dan alınan önemli bilgilerdeki görselleştirmeleri kullanıma açar. 

PostgreSQL sunucusu için Azure Veritabanınızın portal sayfasında, menü çubuğunun **Akıllı Performans** bölümü altında Sorgu **performans Öngörüsünü** seçin.

![Sorgu Performans Öngörüsü uzun süren sorgular](./media/concepts-query-performance-insight/query-performance-insight-landing-page.png)

**Uzun süren sorgular** sekmesi, 15 dakikalık aralıklarla toplanan yürütme başına ortalama süreye göre en iyi beş sorguyu gösterir. **Sorgu Sayısı** açılır menüsünden seçerek, daha fazla sorgu görüntüleyebilirsiniz. Bunu yaptığınızda, grafik renkleri belirli bir Sorgu Kimliği için değişebilir.

Belirli bir zaman penceresine daraltmak için grafikte tıklayıp sürükleyebilirsiniz. Alternatif olarak, sırasıyla daha küçük veya daha büyük bir süreyi görüntülemek için simgeleri yakınlaştırın ve çıkartın.

Grafiğin altındaki tablo, o zaman penceresinde uzun süren sorgular hakkında daha fazla ayrıntı verir.

Sunucudaki beklemelerle ilgili görselleştirmeleri görüntülemek için **Bekleme İstatistikleri** sekmesini seçin.

![Sorgu Performans Öngörüsü istatistikleri bekler](./media/concepts-query-performance-insight/query-performance-insight-wait-statistics.png)

## <a name="considerations"></a>Dikkat edilmesi gerekenler
* Sorgu Performans Öngörüsü [okundu yinelemeleri](concepts-read-replicas.md)için kullanılamaz.

## <a name="next-steps"></a>Sonraki adımlar
- PostgreSQL için Azure Veritabanı’nda [izleme ve ayarlama](concepts-monitoring.md) hakkında daha fazla bilgi edinin.


