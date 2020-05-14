---
title: Sorgu Performansı İçgörüleri-MySQL için Azure veritabanı
description: Bu makalede MySQL için Azure veritabanı 'nda Sorgu Performansı İçgörüleri özelliği açıklanmaktadır
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 5/12/2020
ms.openlocfilehash: 51b478e5184c79e11d95ff004a652b2e5298558f
ms.sourcegitcommit: 90d2d95f2ae972046b1cb13d9956d6668756a02e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2020
ms.locfileid: "83402605"
---
# <a name="query-performance-insight-in-azure-database-for-mysql"></a>MySQL için Azure Veritabanı'nda Sorgu Performansı İçgörüleri

**Uygulama hedefi:** MySQL için Azure veritabanı 5,7, 8,0

Sorgu Performansı İçgörüleri, en uzun çalışan sorguların ne olduğunu, zaman içinde nasıl değişdiklerinizi ve neleri etkilemeyi beklediğini hızlı bir şekilde tanımanıza yardımcı olur.

## <a name="common-scenarios"></a>Yaygın senaryolar

### <a name="long-running-queries"></a>Uzun süre çalışan sorgular

- Son X saat içinde en uzun çalışan sorguları tanımlama
- Kaynaklarda bekleyen ilk N sorguyu tanımlama
 
### <a name="wait-statistics"></a>Bekleme istatistikleri

- Sorgu için bekleme yapısını anlama
- Kaynak beklemeleri ve kaynak çekişmelerinin nerede olduğunu anlama

## <a name="permissions"></a>İzinler

Sorgu Performansı İçgörüleri’ndeki metni görünüm için **Sahip** veya **Katkıda bulunan** izinleri gereklidir. **Okuyucu**, grafikleri ve tabloları görüntüleyebilir ancak metni sorgulayamaz.

## <a name="prerequisites"></a>Ön koşullar

Sorgu Performansı İçgörüleri çalışması için, verilerin [sorgu deposunda](concepts-query-store.md)bulunması gerekir.

## <a name="viewing-performance-insights"></a>Performans öngörülerini görüntüleme

Azure portaldaki [Sorgu Performansı İçgörüleri](concepts-query-performance-insight.md) görünümü, Query Store’dan alınan önemli bilgilerdeki görselleştirmeleri kullanıma açar.

MySQL için Azure veritabanı sunucunuzun portal sayfasında, menü çubuğunun **akıllı performans** bölümü altında **sorgu performansı içgörüleri** ' yi seçin.

### <a name="long-running-queries"></a>Uzun süre çalışan sorgular

**Uzun süre çalışan sorgular** sekmesi, 15 dakikalık aralıklarda toplanan, yürütme başına ortalama süreye göre ilk 5 sorguyu gösterir. **Sorgu sayısı** açılan listesinden seçerek daha fazla sorgu görüntüleyebilirsiniz. Bunu yaptığınızda, grafik renkleri belirli bir Sorgu Kimliği için değişebilir.

Belirli bir zaman penceresine daraltmak için grafikte tıklayıp sürükleyebilirsiniz. Alternatif olarak, daha küçük veya daha büyük bir zaman aralığı görüntülemek için Yakınlaştır ve Kapat simgelerini kullanın.

![Uzun süre çalışan sorguları Sorgu Performansı İçgörüleri](./media/concepts-query-performance-insight/query-performance-insight-landing-page.png) 

### <a name="wait-statistics"></a>Bekleme istatistikleri

> [!NOTE]
> Bekleme istatistikleri sorgu performans sorunlarını gidermeye yöneliktir. Yalnızca sorun giderme amacıyla açık olması önerilir. <br>Azure portal "hata iletisini alırsanız,*' Microsoft. DBforMySQL ' için karşılaşılan sorun; istek yerine getirilemiyor. Bu sorun devam ederse veya beklenmiyorsa lütfen bu bilgilerle desteğe başvurun.*" Bekleme istatistiklerini görüntülerken, daha kısa bir zaman aralığı kullanın.

Bekleme istatistikleri, belirli bir sorgunun yürütülmesi sırasında oluşan bekleme olaylarının bir görünümünü sağlar. [MySQL Engine belgelerindeki](https://go.microsoft.com/fwlink/?linkid=2098206)bekleme olayı türleri hakkında daha fazla bilgi edinin.

Sunucudaki beklemelerle ilgili görselleştirmeleri görüntülemek için **Bekleme İstatistikleri** sekmesini seçin.

Bekleme istatistikleri görünümünde görüntülenen sorgular, belirtilen zaman aralığı boyunca en büyük değeri gösteren sorgulara göre gruplandırılır.

![Sorgu Performansı İçgörüleri bekleyen istatistikler](./media/concepts-query-performance-insight/query-performance-insight-wait-statistics.png)

## <a name="next-steps"></a>Sonraki adımlar

- MySQL için Azure veritabanı 'nda [izleme ve ayarlama](concepts-monitoring.md) hakkında daha fazla bilgi edinin.