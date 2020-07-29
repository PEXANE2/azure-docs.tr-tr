---
title: T-SQL görünümlerini kullanma
description: T-SQL görünümlerini kullanma ve SYNAPSE SQL havuzunda çözüm geliştirme ipuçları.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 26eb3a495fd1c896416265687d92da66dfc3599b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85212301"
---
# <a name="views-in-synapse-sql-pool"></a>SYNAPSE SQL havuzundaki görünümler

Görünümler, çözümünüzün kalitesini artırmak için çeşitli şekillerde kullanılabilir.

SQL havuzu hem standart hem de gerçekleştirilmiş görünümleri destekler. Her ikisi de SELECT ifadelerle oluşturulmuş ve sorguları Mantıksal tablolar olarak sunulan sanal tablolardır.

Görünümler ortak veri hesaplamasının karmaşıklığını yalıtır ve değişiklikleri hesaplama için bir soyutlama katmanı ekler. böylece sorguları yeniden yazmaya gerek kalmaz.

## <a name="standard-view"></a>Standart Görünüm

Standart Görünüm, görünümün her seferinde verilerini hesaplar. Diskte depolanan veri yok. İnsanlar genellikle standart görünümleri, bir veritabanındaki mantıksal nesneleri ve sorguları düzenlemeye yardımcı olan bir araç olarak kullanır.

Standart bir görünüm kullanmak için bir sorgunun kendisine doğrudan başvuru yapması gerekir. Daha fazla bilgi için bkz. [create VIEW](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) documentation.

SQL havuzundaki görünümler yalnızca meta veri olarak depolanır. Sonuç olarak, aşağıdaki seçenekler kullanılabilir değildir:

* Şema bağlama seçeneği yok
* Temel tablolar görünüm aracılığıyla güncelleştirilemiyor
* Görünümler geçici tablolar üzerinde oluşturulamaz
* GENIŞLETME/NOEXPAND ipuçları desteği yok
* SQL havuzunda dizinli görünüm yok

Standart Görünümler, tablolar arasında performans için iyileştirilmiş birleştirmeleri zorlamak için kullanılabilir. Örneğin, bir görünüm, veri hareketini en aza indirmek için katılım ölçütlerinin bir parçası olarak yedekli bir dağıtım anahtarı içerebilir.

Bir görünümün başka bir avantajı, belirli bir sorguyu veya katılma ipucunu zorlamak olabilir. Bu şekilde görünümlerin kullanılması, birleşimlerin her zaman en iyi şekilde gerçekleştirilmesini sağlar ve kullanıcıların birleştirmeler için doğru yapıyı hatırlamaları gereksinimini ortadan önler.

## <a name="materialized-view"></a>Gerçekleştirilmiş görünüm

Gerçekleştirilmiş bir görünüm, verileri SQL havuzunda tıpkı bir tablo gibi önceden hesaplar, depolar ve korur. Gerçekleştirilmiş bir görünüm kullanıldığında her seferinde yeniden hesaplama gerekmez.

Veriler temel tablolara yüklendiği için, SQL havuzu gerçekleştirilmiş görünümleri eşzamanlı olarak yeniler.  Sorgu iyileştiricisi, görünümler sorguda başvurulmuyorsa bile sorgu performansını artırmak için dağıtılmış gerçekleştirilmiş görünümleri otomatik olarak kullanır.  

Gerçekleştirilmiş görünümlerin en büyük yararlanırken sorguları, küçük sonuç kümesi üreten büyük tablolarda karmaşık sorgulardır (genellikle birleşimler ve toplamaları olan sorgular).  

Gerçekleştirilmiş görünüm sözdizimi ve diğer gereksinimlere ilişkin ayrıntılar için bkz. [Select olarak GERÇEKLEŞTIRILMIŞ görünüm oluşturma](/sql/t-sql/statements/create-materialized-view-as-select-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).  

Sorgu ayarlama Kılavuzu için, [gerçekleştirilmiş görünümlerle performans ayarlamayı](performance-tuning-materialized-views.md)kontrol edin.

## <a name="example"></a>Örnek

Ortak bir uygulama deseninin CREATE TABLE SELECT (CTAS) ve ardından veri yüklenirken bir nesne yeniden adlandırma deseninin kullanıldığı tabloları yeniden oluşturmak için kullanılır.  

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
FROM   dbo.DimDate_stg AS stg;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Ancak, bu yaklaşım, "tablo yok" hata iletileri veren bir kullanıcının görünümü ile birlikte, tabloların görünmesine ve görünmesine neden olabilir.

Görünümler, temel alınan nesneler yeniden adlandırıldığında kullanıcılara tutarlı bir sunum katmanı sağlamak için kullanılabilir. Görünümler aracılığıyla verilere erişim sağlayarak, kullanıcılar temel alınan tablolara yönelik görünürlüğe gerek kalmaz.

Bu katman, veri ambarı tasarımcılarının veri modelini gelişebilmesini sağlarken tutarlı bir kullanıcı deneyimi sağlar. Temel tabloları geliştirmekte olan tasarımcılar, tasarımcı 'nın veri yükleme işlemi sırasında performansı en üst düzeye çıkarmak için CTAS 'yi kullanabileceği anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla geliştirme ipucu için bkz. [SQL havuzu geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).
