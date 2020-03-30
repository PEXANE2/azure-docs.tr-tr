---
title: Sorgu Performans Öngörüsü - MariaDB için Azure Veritabanı
description: Bu makalede, MariaDB için Azure Veritabanı'ndaki Sorgu Performans Öngörüsü özelliği açıklanmaktadır
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 88777ee44551ed6abdb7a6c7c909d6bf55db48c0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79527853"
---
# <a name="query-performance-insight-in-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanı'nda Sorgu Performansı İçgörüleri

**Aşağıdakiler için geçerlidir:** MariaDB 10.2 için Azure Veritabanı

Sorgu Performans Öngörüsü, en uzun süre çalışan sorgularınızın ne olduğunu, zaman içinde nasıl değiştiğini ve hangi beklemelerin onları etkilediğini hızlı bir şekilde belirlemenize yardımcı olur.

## <a name="common-scenarios"></a>Genel senaryolar

### <a name="long-running-queries"></a>Uzun süre çalışan sorgular

- Son X saatiçinde en uzun süre çalışan sorguları tanımlama
- Kaynaklarda bekleyen en iyi N sorgularını tanımlama
 
### <a name="wait-statistics"></a>Bekleme istatistikleri

- Sorgu için bekleme yapısını anlama
- Kaynak bekleme eğilimlerinin ve kaynak çekişmelerinin nerede olduğunu anlama

## <a name="permissions"></a>İzinler

Sorgu Performansı İçgörüleri’ndeki metni görünüm için **Sahip** veya **Katkıda bulunan** izinleri gereklidir. **Okuyucu**, grafikleri ve tabloları görüntüleyebilir ancak metni sorgulayamaz.

## <a name="prerequisites"></a>Ön koşullar

Sorgu Performans Öngörüsünc'ün çalışması için Sorgu [Deposu'nda](concepts-query-store.md)veri bulunması gerekir.

## <a name="viewing-performance-insights"></a>Performans öngörülerini görüntüleme

Azure portaldaki [Sorgu Performansı İçgörüleri](concepts-query-performance-insight.md) görünümü, Query Store’dan alınan önemli bilgilerdeki görselleştirmeleri kullanıma açar.

MariaDB sunucusu için Azure Veritabanınızın portal sayfasında, menü çubuğunun **Akıllı Performans** bölümü altında Sorgu **Performans Öngörüsü'nü** seçin.

### <a name="long-running-queries"></a>Uzun süre çalışan sorgular

**Uzun süren sorgular** sekmesi, 15 dakikalık aralıklarla toplanan yürütme başına ortalama süreye göre en üstteki 5 sorguyu gösterir. Açılan **Sorgu Sayısı'ndan** seçerek daha fazla sorgu görüntüleyebilirsiniz. Bunu yaptığınızda, grafik renkleri belirli bir Sorgu Kimliği için değişebilir.

Belirli bir zaman penceresine daraltmak için grafikte tıklayıp sürükleyebilirsiniz. Alternatif olarak, sırasıyla daha küçük veya daha büyük bir zaman dilimini görüntülemek için simgeleri yakınlaştırın ve çıkar'ı kullanın.

![Sorgu Performans Öngörüsü uzun süren sorgular](./media/concepts-query-performance-insight/query-performance-insight-landing-page.png)

### <a name="wait-statistics"></a>Bekleme istatistikleri 

> [!NOTE]
> Bekleme istatistikleri, sorun giderme sorgusu performans sorunları içindir. Yalnızca sorun giderme amacıyla açık olması önerilir. <br>Azure portalında hata iletisi alırsanız "*'Microsoft.DBforMariaDB' için karşılaşılan sorun; isteğini yerine getiremez. Bu sorun devam ederse veya beklenmeyen bir durumsa, lütfen bu bilgilerle destek le iletişime geçin.* bekleme istatistiklerini görüntülerken daha küçük bir zaman dilimi kullanın.

Bekleme istatistikleri, belirli bir sorgunun yürütülmesi sırasında oluşan bekleme olaylarının görünümünü sağlar. [MySQL motor belgelerinde](https://go.microsoft.com/fwlink/?linkid=2098206)bekleme olay türleri hakkında daha fazla bilgi edinin.

Sunucudaki beklemelerle ilgili görselleştirmeleri görüntülemek için **Bekleme İstatistikleri** sekmesini seçin.

Bekleme istatistikleri görünümünde görüntülenen sorgular, belirtilen zaman aralığında en büyük bekleme süresini gösteren sorgulara göre gruplandırılır.

![Sorgu Performans Öngörüsü istatistikleri bekler](./media/concepts-query-performance-insight/query-performance-insight-wait-statistics.png)

## <a name="next-steps"></a>Sonraki adımlar

- MariaDB için Azure Veritabanı'nda [izleme ve aetme](concepts-monitoring.md) hakkında daha fazla bilgi edinin.