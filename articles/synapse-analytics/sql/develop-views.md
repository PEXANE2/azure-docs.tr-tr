---
title: Synapse SQL kullanarak T-SQL görünümleri
description: Synapse SQL ile T-SQL görünümlerini kullanmak ve çözümler geliştirmek için ipuçları.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: 3a073c9539f2fb996ae59ef513525c217170f2e7
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81428660"
---
# <a name="t-sql-views-using-synapse-sql"></a>Synapse SQL kullanarak T-SQL görünümleri
Bu makalede, T-SQL görünümlerini kullanmak ve Synapse SQL ile çözümler geliştirmek için ipuçları bulacaksınız. 

## <a name="why-use-views"></a>Neden görünümler kullanılır?

Görünümler, çözümünüzün kalitesini artırmak için çeşitli şekillerde kullanılabilir.  Bu makalede, çözümünüzü görünümlerle nasıl zenginleştireceğinize ilişkin birkaç örnek vurgulanır ve dikkate alınması gereken sınırlamaları içerir.

### <a name="sql-pool---create-view"></a>SQL havuzu - görünüm oluşturma

> [!NOTE]
> **SQL havuzu**: CREATE VIEW için sözdizimi bu makalede ele alınmadı. Daha fazla bilgi için [CREATE VIEW](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) belgelerine bakın.

### <a name="sql-on-demand-preview---create-view"></a>İsteğe bağlı SQL (önizleme) - görünüm oluşturma

> [!NOTE]
> **SQL on-demand**: CREATE VIEW için sözdizimi bu makalede ele alınmadı. Daha fazla bilgi için [CREATE VIEW](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) belgelerine bakın.

## <a name="architectural-abstraction"></a>Mimari soyutlama

Ortak bir uygulama deseni, create [TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) (CTAS) kullanarak tabloları yeniden oluşturmaktır ve bunu veri yüklerken nesne yeniden adlandırma deseni izler.

Aşağıdaki örnek, bir tarih boyutuna yeni tarih kayıtları ekler. DimDate_New yeni bir tablonun önce nasıl oluşturulduğuna ve daha sonra tablonun özgün sürümünü niçin değiştirilmeye başlanmıştır.

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

Bu yaklaşımın tabloların kullanıcının görünümünden görünmesine ve kaybolmasına neden olabileceğini ve "tablo yok" hata iletilerini sistemleri olduğunu unutmayın. Görünümler, temel nesneler yeniden adlandırılırken kullanıcılara tutarlı bir sunu katmanı sağlamak için kullanılabilir.

Görünümler aracılığıyla verilere erişim sağlayarak, kullanıcıların temel tablolariçin görünürlüğe ihtiyacı yoktur. Tutarlı bir kullanıcı deneyimine ek olarak, bu katman analiz tasarımcılarının veri modelini geliştirebilmesini sağlar. Temel tabloları geliştirme yeteneği, tasarımcıların veri yükleme işlemi sırasında performansı en üst düzeye çıkarmak için CTAS'yi kullanabileceği anlamına gelir.

## <a name="performance-optimization"></a>Performansı en iyi duruma getirme

Görünümler, tablolar arasında performans en iyi duruma getirilmiş birleştirmeleri zorlamak için de kullanılabilir. Örneğin, bir görünüm, veri hareketini en aza indirmek için birleştirme ölçütlerinin bir parçası olarak gereksiz bir dağıtım anahtarı ekleyebilir.

Belirli bir sorguzorlama veya ipucu birleştirme T-SQL görünümleri kullanarak başka bir yararı. Bu nedenle, görünüm yeteneği birleştirmelerin her zaman en uygun şekilde gerçekleştirilmesini sağlar. Kullanıcıların birleştirmeleri için doğru yapıyı hatırlamaları gerektiğinden kurtulursunuz.

## <a name="limitations"></a>Sınırlamalar

Synapse SQL'deki görünümler yalnızca meta veri olarak depolanır. Sonuç olarak, aşağıdaki seçenekler kullanılamıyor:

* Şema bağlama seçeneği yoktur.
* Temel tablolar görünüm üzerinden güncelleştirilemiyor
* Geçici tablolar üzerinde görünümler oluşturulamıyor
* EXPAND / NOEXPAND ipuçları için destek yok
* Synapse SQL'de dizine eklenmiş görünüm yok

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla geliştirme ipucu için [Synapse SQL geliştirme genel bakış'a](develop-overview.md)bakın.



