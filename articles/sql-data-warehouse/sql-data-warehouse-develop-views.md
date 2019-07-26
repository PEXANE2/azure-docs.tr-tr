---
title: Azure SQL veri ambarı 'nda T-SQL görünümlerini kullanma | Microsoft Docs
description: Azure SQL veri ambarı 'nda çözüm geliştirmeye yönelik T-SQL görünümlerini kullanma ipuçları.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 8a770e66120e69271744942899186ece39b2a3c3
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68479514"
---
# <a name="views-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı 'nda görünümler
Azure SQL veri ambarı 'nda çözüm geliştirmeye yönelik T-SQL görünümlerini kullanma ipuçları. 

## <a name="why-use-views"></a>Görünümler neden kullanılmalıdır?
Görünümler, çözümünüzün kalitesini artırmak için çeşitli şekillerde kullanılabilir.  Bu makalede, çözümünüzü görünümlerle nasıl zenginleştirmenin yanı sıra göz önünde bulundurulması gereken sınırlamalar gösterilmektedir.


> [!IMPORTANT]
> [GERÇEKLEŞTIRILMIŞ Görünüm Oluştur ' da Select olarak](/sql/t-sql/statements/create-materialized-view-as-select-transact-sql?view=azure-sqldw-latest)yeni gerçekleştirilmiş görünüm sözdizimine bakın.  Daha fazla bilgi için [sürüm notlarına](/azure/sql-data-warehouse/release-notes-10-0-10106-0)bakın.
>


> [!NOTE]
> OLUŞTURMA görünümü sözdizimi Bu makalede ele alınmıyor. Daha fazla bilgi için bkz. [create VIEW](/sql/t-sql/statements/create-view-transact-sql) documentation.
> 

## <a name="architectural-abstraction"></a>Mimari soyutlama

Ortak bir uygulama deseninin ardından CREATE TABLE SELECT (CTAS) kullanarak tabloları yeniden oluşturmak ve ardından verileri yüklerken bir nesne yeniden adlandırma deseninin oluşturulması.

Aşağıdaki örnek bir tarih boyutuna yeni tarih kayıtları ekler. Yeni bir tablo olan DimDate_New, ilk olarak nasıl oluşturulduğunu ve tablonun orijinal sürümünün yerini alacak şekilde yeniden adlandırdığını aklınızda edin.

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

Bununla birlikte, bu yaklaşım, tabloların bir kullanıcının görünümünden görünmesine ve görünmesine neden olabilir ve "tablo yok" hata iletileri de görünür. Görünümler, kullanıcılara, temel alınan nesnelerin yeniden adlandırıldığını tutarlı bir sunum katmanı sağlamak için kullanılabilir. Görünümler aracılığıyla verilere erişim sağlayarak, kullanıcılar temeldeki tablolara yönelik görünürlüğe gerek kalmaz. Bu katman, veri ambarı tasarımcılarının veri modelini gelişebilmesini sağlarken tutarlı bir kullanıcı deneyimi sağlar. Temel tabloları geliştirmekte olan tasarımcılar, tasarımcı 'nın veri yükleme işlemi sırasında performansı en üst düzeye çıkarmak için CTAS 'yi kullanabileceği anlamına gelir.   

## <a name="performance-optimization"></a>Performans iyileştirmesi
Görünümler, tablolar arasında performans için iyileştirilmiş birleştirmeleri zorlamak için de kullanılabilir. Örneğin, bir görünüm, veri hareketini en aza indirmek için katılım ölçütlerinin bir parçası olarak yedekli bir dağıtım anahtarı içerebilir. Bir görünümün başka bir avantajı, belirli bir sorguyu veya katılma ipucunu zorlamak olabilir. Bu şekilde görünümlerin kullanılması, birleşimlerin her zaman en iyi şekilde gerçekleştirilmesini sağlar ve kullanıcıların birleştirmeler için doğru yapıyı hatırlamaları gereksinimini ortadan önler.

## <a name="limitations"></a>Sınırlamalar
SQL veri ambarı 'ndaki görünümler yalnızca meta veri olarak depolanır. Sonuç olarak, aşağıdaki seçenekler kullanılamaz:

* Şema bağlama seçeneği yok
* Temel tablolar görünüm aracılığıyla güncelleştirilemez
* Görünümler geçici tablolar üzerinde oluşturulamaz
* GENIŞLETME/NOEXPAND ipuçları desteği yok
* SQL Data Warehouse 'da dizinli görünüm yok

## <a name="next-steps"></a>Sonraki adımlar
Geliştirme ile ilgili daha fazla ipucu için bkz. [SQL Data Warehouse geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).


