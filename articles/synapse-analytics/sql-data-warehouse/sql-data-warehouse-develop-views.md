---
title: T-SQL görünümlerini kullanma
description: Synapse SQL havuzunda T-SQL görünümlerini kullanmak ve çözümler geliştirmek için ipuçları.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 76442368fe4b3e498f622a8a3cd5b5b973f16bd6
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633386"
---
# <a name="views-in-synapse-sql-pool"></a>Synapse SQL havuzunda görünümler

Görünümler, çözümünüzün kalitesini artırmak için çeşitli şekillerde kullanılabilir.

SQL havuzu hem standart hem de maddelendirilmiş görünümleri destekler. Her ikisi de SELECT ifadeleri ile oluşturulan ve sorgulara mantıksal tablolar olarak sunulan sanal tablolardır.

Görünümler, ortak veri hesaplamasının karmaşıklığını kapsüller ve sorguları yeniden yazmaya gerek kalmadan hesaplama değişikliklerine bir soyutlama katmanı ekler.

## <a name="standard-view"></a>Standart görünüm

Standart görünüm, görünüm her kullanıldığında verilerini bilgilenir. Diskte depolanan veri yok. İnsanlar genellikle bir veritabanında mantıksal nesneleri ve sorguları düzenlemeye yardımcı olan bir araç olarak standart görünümleri kullanır.

Standart bir görünüm kullanmak için, bir sorgunun doğrudan başvuruda bulunması gerekir. Daha fazla bilgi için [CREATE VIEW](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) belgelerine bakın.

SQL havuzundaki görünümler yalnızca meta veri olarak depolanır. Sonuç olarak, aşağıdaki seçenekler kullanılamıyor:

* Şema bağlama seçeneği yoktur
* Temel tablolar görünüm üzerinden güncelleştirilemiyor
* Geçici tablolar üzerinde görünümler oluşturulamıyor
* EXPAND / NOEXPAND ipuçları için destek yoktur
* SQL havuzunda dizinlenmiş görünüm yok

Tablolar arasında performans en iyi duruma getirilmiş birleştirmeleri zorlamak için standart görünümler kullanılabilir. Örneğin, bir görünüm, veri hareketini en aza indirmek için birleştirme ölçütlerinin bir parçası olarak gereksiz bir dağıtım anahtarı ekleyebilir.

Görünümün başka bir yararı da belirli bir sorguya zorlamak veya ipucunu birleştirmek olabilir. Görünümlerin bu şekilde kullanılması, birleştirmelerin her zaman en uygun şekilde gerçekleştirildiğini ve kullanıcıların birleştirmeleri için doğru yapıyı hatırlamalarını önler.

## <a name="materialized-view"></a>Gerçekleştirilmiş görünüm

Somutlaştırılmış görünüm, verilerini bir tablo gibi SQL havuzunda önceden hesaplar, depolar ve korur. Somutlaştırılmış görünüm kullanıldığında her seferinde yeniden hesaplama gerekmez.

Veriler temel tablolara yüklendikçe, SQL havuzu maddeleşmiş görünümleri eşzamanlı olarak yeniler.  Sorgu optimize edici, sorguda başvurulsa bile sorgu performansını artırmak için otomatik olarak dağıtılmış maddeleştirilmiş görünümleri kullanır.  

Önemlileştirilmiş görünümlerden en çok yararlanan sorgular, küçük sonuç kümesi oluşturan büyük tablolardaki karmaşık sorgulardır (genellikle birleştirme ve toplamaiçeren sorgular).  

Maddeleştirilmiş görünüm sözdizimi ve diğer gereksinimler hakkında ayrıntılı bilgi için SELECT [olarak MATERYALIZE GÖRÜNÜM OLUŞTUR'a](/sql/t-sql/statements/create-materialized-view-as-select-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)bakın.  

Sorgu atonlama kılavuzu için, [gerçekleştirilmiş görünümlerle Performans atolukutusunu](performance-tuning-materialized-views.md)denetleyin.

## <a name="example"></a>Örnek

Ortak bir uygulama deseni, create TABLE AS SELECT (CTAS) kullanarak tabloları yeniden oluşturmak ve ardından verileri yüklerken nesne yeniden adlandırma deseni oluşturmaktır.  

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
FROM   dbo.DimDate_stg AS stg;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Ancak, bu yaklaşım tabloların görünmesine ve kullanıcının görünümünden kaybolmasına ve "tablo yok" hata iletilerinin verilmesine neden olabilir.

Görünümler, temel nesneler yeniden adlandırılırken kullanıcılara tutarlı bir sunu katmanı sağlamak için kullanılabilir. Görünümler aracılığıyla verilere erişim sağlayarak, kullanıcıların temel tablolariçin görünürlüğe ihtiyacı yoktur.

Bu katman, veri ambarı tasarımcılarının veri modelini geliştirebilmesini sağlarken tutarlı bir kullanıcı deneyimi sağlar. Temel tabloları geliştirebilmek, tasarımcıların veri yükleme işlemi sırasında performansı en üst düzeye çıkarmak için CTAS'yi kullanabileceği anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla geliştirme ipucu için [SQL havuzu geliştirme genel bakış](sql-data-warehouse-overview-develop.md)bakın.
