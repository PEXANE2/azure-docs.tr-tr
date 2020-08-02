---
title: Harmanlama desteği
description: Azure SYNAPSE SQL 'de desteklenen harmanlama türleri
author: filippopovic
ms.service: synapse-analytics
ms.topic: reference
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 1099c4e4dd69a8dc8caee96ec5dda633ce8b9d12
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87496211"
---
# <a name="database-collation-support-for-synapse-sql"></a>SYNAPSE SQL için veritabanı harmanlama desteği

Harmanlamalar karakter tabanlı veri türleri için yerel ayar, kod sayfası, sıralama düzeni ve karakter duyarlılığı kuralları sağlar. Seçildiğinde, harmanlama bilgilerini gerektiren tüm sütunlar ve ifadeler, seçili harmanlamayı veritabanı ayarından devralınır. Varsayılan devralma, karakter tabanlı bir veri türü için farklı bir harmanlama açıkça belirtilerek geçersiz kılınabilir.

Yeni bir SQL havuzu veritabanı oluştururken Azure portal varsayılan veritabanı harmanlamasını değiştirebilirsiniz. Bu özellik, 3800 desteklenen veritabanı harmanlamainden birini kullanarak yeni bir veritabanı oluşturulmasını kolaylaştırır.

CREATE DATABASE ifadesini kullanarak oluşturma sırasında varsayılan SYNAPSE SQL isteğe bağlı veritabanı harmanlamasını belirtebilirsiniz.

## <a name="change-collation"></a>Harmanlamayı değiştir
SQL havuzu veritabanı için Varsayılan harmanlamayı değiştirmek üzere, sağlama deneyiminde harmanlama alanına basit bir güncelleştirme olursunuz. Örneğin, Varsayılan harmanlamayı büyük/küçük harfe duyarlı olarak değiştirmek isterseniz, harmanlamayı SQL_Latin1_General_CP1_CI_AS SQL_Latin1_General_CP1_CS_AS olarak yeniden adlandırmanız yeterlidir. 

İsteğe bağlı SQL veritabanı için Varsayılan harmanlamayı değiştirmek üzere ALTER DATABASE deyimini kullanabilirsiniz.

## <a name="list-of-unsupported-collation-types"></a>Desteklenmeyen harmanlama türlerinin listesi
*    Japanese_Bushu_Kakusu_140_BIN
*    Japanese_Bushu_Kakusu_140_BIN2
*    Japanese_Bushu_Kakusu_140_CI_AI_VSS
*    Japanese_Bushu_Kakusu_140_CI_AI_WS_VSS
*    Japanese_Bushu_Kakusu_140_CI_AI_KS_VSS
*    Japanese_Bushu_Kakusu_140_CI_AI_KS_WS_VSS
*    Japanese_Bushu_Kakusu_140_CI_AS_VSS
*    Japanese_Bushu_Kakusu_140_CI_AS_WS_VSS
*    Japanese_Bushu_Kakusu_140_CI_AS_KS_VSS
*    Japanese_Bushu_Kakusu_140_CI_AS_KS_WS_VSS
*    Japanese_Bushu_Kakusu_140_CS_AI_VSS
*    Japanese_Bushu_Kakusu_140_CS_AI_WS_VSS
*    Japanese_Bushu_Kakusu_140_CS_AI_KS_VSS
*    Japanese_Bushu_Kakusu_140_CS_AI_KS_WS_VSS
*    Japanese_Bushu_Kakusu_140_CS_AS_VSS
*    Japanese_Bushu_Kakusu_140_CS_AS_WS_VSS
*    Japanese_Bushu_Kakusu_140_CS_AS_KS_VSS
*    Japanese_Bushu_Kakusu_140_CS_AS_KS_WS_VSS
*    Japanese_Bushu_Kakusu_140_CI_AI
*    Japanese_Bushu_Kakusu_140_CI_AI_WS
*    Japanese_Bushu_Kakusu_140_CI_AI_KS
*    Japanese_Bushu_Kakusu_140_CI_AI_KS_WS
*    Japanese_Bushu_Kakusu_140_CI_AS
*    Japanese_Bushu_Kakusu_140_CI_AS_WS
*    Japanese_Bushu_Kakusu_140_CI_AS_KS
*    Japanese_Bushu_Kakusu_140_CI_AS_KS_WS
*    Japanese_Bushu_Kakusu_140_CS_AI
*    Japanese_Bushu_Kakusu_140_CS_AI_WS
*    Japanese_Bushu_Kakusu_140_CS_AI_KS
*    Japanese_Bushu_Kakusu_140_CS_AI_KS_WS
*    Japanese_Bushu_Kakusu_140_CS_AS
*    Japanese_Bushu_Kakusu_140_CS_AS_WS
*    Japanese_Bushu_Kakusu_140_CS_AS_KS
*    Japanese_Bushu_Kakusu_140_CS_AS_KS_WS
*    Japanese_XJIS_140_BIN
*    Japanese_XJIS_140_BIN2
*    Japanese_XJIS_140_CI_AI_VSS
*    Japanese_XJIS_140_CI_AI_WS_VSS
*    Japanese_XJIS_140_CI_AI_KS_VSS
*    Japanese_XJIS_140_CI_AI_KS_WS_VSS
*    Japanese_XJIS_140_CI_AS_VSS
*    Japanese_XJIS_140_CI_AS_WS_VSS
*    Japanese_XJIS_140_CI_AS_KS_VSS
*    Japanese_XJIS_140_CI_AS_KS_WS_VSS
*    Japanese_XJIS_140_CS_AI_VSS
*    Japanese_XJIS_140_CS_AI_WS_VSS
*    Japanese_XJIS_140_CS_AI_KS_VSS
*    Japanese_XJIS_140_CS_AI_KS_WS_VSS
*    Japanese_XJIS_140_CS_AS_VSS
*    Japanese_XJIS_140_CS_AS_WS_VSS
*    Japanese_XJIS_140_CS_AS_KS_VSS
*    Japanese_XJIS_140_CS_AS_KS_WS_VSS
*    Japanese_XJIS_140_CI_AI
*    Japanese_XJIS_140_CI_AI_WS
*    Japanese_XJIS_140_CI_AI_KS
*    Japanese_XJIS_140_CI_AI_KS_WS
*    Japanese_XJIS_140_CI_AS
*    Japanese_XJIS_140_CI_AS_WS
*    Japanese_XJIS_140_CI_AS_KS
*    Japanese_XJIS_140_CI_AS_KS_WS
*    Japanese_XJIS_140_CS_AI
*    Japanese_XJIS_140_CS_AI_WS
*    Japanese_XJIS_140_CS_AI_KS
*    Japanese_XJIS_140_CS_AI_KS_WS
*    Japanese_XJIS_140_CS_AS
*    Japanese_XJIS_140_CS_AS_WS
*    Japanese_XJIS_140_CS_AS_KS
*    Japanese_XJIS_140_CS_AS_KS_WS

Ayrıca, SQL havuzu aşağıdaki harmanlama türlerini desteklemez:

*    SQL_EBCDIC1141_CP1_CS_AS
*    SQL_EBCDIC277_2_CP1_CS_AS
*    UTF-8

## <a name="check-the-current-collation"></a>Geçerli harmanlamayı denetle
Veritabanının geçerli harmanlamasını denetlemek için aşağıdaki T-SQL kod parçacığını çalıştırabilirsiniz:
```sql
SELECT DATABASEPROPERTYEX(DB_NAME(), 'Collation') AS Collation;
```
Özellik parametresi olarak ' harmanlama ' geçirildiğinde, DatabasePropertyEx işlevi belirtilen veritabanı için geçerli harmanlamayı döndürür. MSDN 'de DatabasePropertyEx işlevi hakkında daha fazla bilgi edinebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

SQL havuzu için en iyi uygulamalar ve isteğe bağlı SQL hakkında ek bilgiler aşağıdaki makalelerde bulunabilir:

- [SQL havuzu için en iyi uygulamalar](best-practices-sql-pool.md)
- [İsteğe bağlı SQL için en iyi uygulamalar](best-practices-sql-on-demand.md)


