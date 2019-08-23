---
title: PostgreSQL için Azure veritabanı 'nda Sorgu Performansı İçgörüleri-tek sunucu
description: Bu makalede PostgreSQL için Azure veritabanı-tek sunucu Sorgu Performansı İçgörüleri özelliği açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 08/21/2019
ms.openlocfilehash: cd07656ab4e6e0f684eecf35d241eac539b7a552
ms.sourcegitcommit: beb34addde46583b6d30c2872478872552af30a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69907463"
---
# <a name="query-performance-insight"></a>Sorgu Performansı İçgörüleri 

**Şunlara uygulanır:** PostgreSQL için Azure veritabanı-tek sunucu 9,6 ve 10

Sorgu Performansı İçgörüleri, en uzun çalışan sorguların ne olduğunu, zaman içinde nasıl değişdiklerinizi ve neleri etkilemeyi beklediğini hızlı bir şekilde tanımanıza yardımcı olur.

## <a name="permissions"></a>İzinler
Sorgu Performansı İçgörüleri’ndeki metni görünüm için **Sahip** veya **Katkıda bulunan** izinleri gereklidir. **Okuyucu**, grafikleri ve tabloları görüntüleyebilir ancak metni sorgulayamaz.

## <a name="prerequisites"></a>Önkoşullar
Sorgu Performansı İçgörüleri çalışması için, verilerin [sorgu deposunda](concepts-query-store.md)bulunması gerekir.

## <a name="viewing-performance-insights"></a>Performans öngörülerini görüntüleme
Azure portaldaki [Sorgu Performansı İçgörüleri](concepts-query-performance-insight.md) görünümü, Query Store’dan alınan önemli bilgilerdeki görselleştirmeleri kullanıma açar. 

PostgreSQL için Azure veritabanı sunucunuzun portal sayfasında, menü çubuğunun **akıllı performans** bölümünde **sorgu performansı Insight** ' ı seçin.

![Uzun süre çalışan sorguları Sorgu Performansı İçgörüleri](./media/concepts-query-performance-insight/query-performance-insight-landing-page.png)

**Uzun süre çalışan sorgular** sekmesi, 15 dakikalık aralıklarda toplanan, yürütme başına ortalama süreye göre ilk beş sorguyu gösterir. **Sorgu Sayısı** açılır menüsünden seçerek, daha fazla sorgu görüntüleyebilirsiniz. Bunu yaptığınızda, grafik renkleri belirli bir Sorgu Kimliği için değişebilir.

Belirli bir zaman penceresine daraltmak için grafikte tıklayıp sürükleyebilirsiniz. Alternatif olarak, daha küçük veya daha büyük bir zaman aralığını görüntülemek için Yakınlaştır ve Kapat simgelerini kullanın.

Grafiğin altındaki tablo, bu zaman penceresinde uzun süreli sorgular hakkında daha fazla ayrıntı sağlar.

Sunucudaki beklemelerle ilgili görselleştirmeleri görüntülemek için **Bekleme İstatistikleri** sekmesini seçin.

![Sorgu Performansı İçgörüleri bekleyen istatistikler](./media/concepts-query-performance-insight/query-performance-insight-wait-statistics.png)

## <a name="considerations"></a>Dikkat edilmesi gerekenler
* Sorgu Performansı İçgörüleri, [okuma çoğaltmaları](concepts-read-replicas.md)için kullanılamaz.

## <a name="next-steps"></a>Sonraki adımlar
- PostgreSQL için Azure Veritabanı’nda [izleme ve ayarlama](concepts-monitoring.md) hakkında daha fazla bilgi edinin.


