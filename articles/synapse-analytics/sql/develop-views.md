---
title: SYNAPSE SQL kullanarak T-SQL görünümleri
description: T-SQL görünümlerini kullanma ve SYNAPSE SQL ile çözüm geliştirme ipuçları.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: 3a073c9539f2fb996ae59ef513525c217170f2e7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81428660"
---
# <a name="t-sql-views-using-synapse-sql"></a>SYNAPSE SQL kullanarak T-SQL görünümleri
Bu makalede T-SQL görünümlerini kullanma ve SYNAPSE SQL ile çözüm geliştirme hakkında ipuçları bulacaksınız. 

## <a name="why-use-views"></a>Görünümler neden kullanılmalıdır?

Görünümler, çözümünüzün kalitesini artırmak için çeşitli şekillerde kullanılabilir.  Bu makalede, çözümünüzü görünümlerle nasıl zenginleştirmenin birkaç örneği vurgulanmıştır ve göz önünde bulundurulması gereken sınırlamalar yer almaktadır.

### <a name="sql-pool---create-view"></a>SQL havuzu-görünüm oluştur

> [!NOTE]
> **SQL havuzu**: oluşturma görünümü sözdizimi Bu makalede ele alınmıyor. Daha fazla bilgi için bkz. [create VIEW](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) documentation.

### <a name="sql-on-demand-preview---create-view"></a>İsteğe bağlı SQL (Önizleme)-görünüm oluştur

> [!NOTE]
> **İsteğe bağlı SQL**: oluşturma görünümü sözdizimi Bu makalede ele alınmıyor. Daha fazla bilgi için bkz. [create VIEW](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) documentation.

## <a name="architectural-abstraction"></a>Mimari soyutlama

Ortak bir uygulama, [Create Table Select](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) (CTAS) kullanarak tabloları yeniden oluşturmak, bu, verileri yüklerken bir nesne yeniden adlandırma deseninin ardından.

Aşağıdaki örnek bir tarih boyutuna yeni tarih kayıtları ekler. Yeni bir tablo olan DimDate_New önce nasıl oluşturulduğuna ve sonra tablonun orijinal sürümünün yerini alacak şekilde yeniden adlandırdığına göz önünde edin.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Bu yaklaşımda, tabloların bir kullanıcının görünümünden görüntülenen ve görünmeyen tabloların oluşmasına neden olabileceğini ve "tablo yok" hata iletileri istemlerini aklınızda bulundurun. Görünümler, temel alınan nesneler yeniden adlandırıldığında kullanıcılara tutarlı bir sunum katmanı sağlamak için kullanılabilir.

Görünümler aracılığıyla verilere erişim sağlayarak, kullanıcılar temel alınan tablolara yönelik görünürlüğe gerek kalmaz. Tutarlı bir kullanıcı deneyiminin yanı sıra, bu katman analiz tasarımcılarının veri modelini geliştirebilir olmasını sağlar. Temel tabloları gelişme özelliği, tasarımcılarının veri yükleme işlemi sırasında performansı en üst düzeye çıkarmak için CTAS 'yi kullanabileceği anlamına gelir.

## <a name="performance-optimization"></a>Performansı en iyi duruma getirme

Görünümler, tablolar arasında performans için iyileştirilmiş birleştirmeleri zorlamak için de kullanılabilir. Örneğin, bir görünüm, veri hareketini en aza indirmek için katılım ölçütlerinin bir parçası olarak yedekli bir dağıtım anahtarı içerebilir.

Belirli bir sorguyu veya katılma ipucunu zorlamak, T-SQL görünümlerini kullanmanın başka bir avantajıdır. Bu nedenle, görünümler özelliği birleştirmeleri her zaman en iyi şekilde gerçekleştirilmesini sağlar. Kullanıcıların birleştirmeler için doğru yapıyı hatırlamaları gereksinimini ortadan yacaksınız.

## <a name="limitations"></a>Sınırlamalar

SYNAPSE SQL 'deki görünümler yalnızca meta veri olarak depolanır. Sonuç olarak, aşağıdaki seçenekler kullanılabilir değildir:

* Şema bağlama seçeneği yok
* Temel tablolar görünüm aracılığıyla güncelleştirilemiyor
* Görünümler geçici tablolar üzerinde oluşturulamaz
* GENIŞLETME/NOEXPAND ipuçları için destek yoktur
* SYNAPSE SQL 'de dizinli görünüm yok

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla geliştirme ipucu için bkz. [SYNAPSE SQL geliştirmesine genel bakış](develop-overview.md).



