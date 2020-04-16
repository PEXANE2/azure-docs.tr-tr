---
title: Harmanlama
description: Azure Synapse SQL'de desteklenen harmanlama türleri
author: filippopovic
ms.service: synapse-analytics
ms.topic: reference
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 4270677f8f5f77e1ada0b1d9385163dad762bbda
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81431546"
---
# <a name="database-collation-support-for-synapse-sql"></a>Synapse SQL için veritabanı harmanlama desteği

Harmanlamalar, karakter tabanlı veri türleri için yerel düzen, kod sayfası, sıralama sırası ve karakter duyarlılığı kurallarını sağlar. Seçildikten sonra, harmanlama bilgileri gerektiren tüm sütunlar ve ifadeler seçilen harmanlama veritabanı ayarından devralır. Varsayılan devralma, karakter tabanlı bir veri türü için farklı bir harmanlama açıkça belirterek geçersiz kılınabilir.

Yeni bir SQL havuzu veritabanı oluşturduğunuzda varsayılan veritabanı harmanlamaını Azure portalından değiştirebilirsiniz. Bu özellik, desteklenen 3800 veritabanı harmanlamalarından birini kullanarak yeni bir veritabanı oluşturmayı daha da kolaylaştırır.

CREATE DATABASE deyimini kullanarak oluşturma sırasında varsayılan Synapse SQL isteğe bağlı veritabanı harmanlamaını belirtebilirsiniz.

## <a name="changing-collation"></a>Harmanlama
SQL havuz veritabanı için varsayılan harmanlama değiştirmek için, sağlama deneyiminde Collation alanına basit güncelleştirme. Örneğin, varsayılan harmanlama durumunu büyük/küçük harf duyarlıolarak değiştirmek isterseniz, Collation'ı SQL_Latin1_General_CP1_CI_AS'dan SQL_Latin1_General_CP1_CS_AS'a yeniden adlandırmanız yeterlidir. 

SQL on-demand veritabanı için varsayılan harmanlama değiştirmek için ALTER DATABASE deyimini kullanabilirsiniz.

## <a name="list-of-unsupported-collation-types"></a>Desteklenmeyen harmanlama türleri listesi
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

## <a name="checking-the-current-collation"></a>Geçerli harmanlama denetimi
Veritabanı için geçerli harmanlama denetlemek için aşağıdaki T-SQL snippet çalıştırabilirsiniz:
```sql
SELECT DATABASEPROPERTYEX(DB_NAME(), 'Collation') AS Collation;
```
Özellik parametresi olarak 'Harmasyon' geçirildiğinde, DatabasePropertyEx işlevi belirtilen veritabanı için geçerli harmanlama döndürür. MSDN'deki DatabasePropertyEx işlevi hakkında daha fazla bilgi edinebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

SQL havuzu ve isteğe bağlı SQL için en iyi uygulamalar hakkında ek bilgiler aşağıdaki makalelerde bulunabilir:

- [SQL havuzu için En İyi Uygulamalar](best-practices-sql-pool.md)
- [İsteğe bağlı SQL için en iyi uygulamalar](best-practices-sql-on-demand.md)


