---
title: Veri ambarı harmanlama türleri
description: Azure SYNAPSE Analytics SQL havuzunda desteklenen harmanlama türleri.
services: sql-data-warehouse
author: antvgski
manager: igorstan
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.date: 12/04/2019
ms.author: anvang
ms.reviewer: jrasnick
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 67627e4157c85853cf05dd6b24ced968a9654e62
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78198448"
---
# <a name="database-collation-support-for-azure-synapse-analytics-sql-pool"></a>Azure SYNAPSE Analytics SQL havuzu için veritabanı harmanlama desteği

Yeni bir Azure SYNAPSE SQL havuzu veritabanı oluştururken Azure portal varsayılan veritabanı harmanlamasını değiştirebilirsiniz. Bu özellik, 3800 desteklenen veritabanı harmanlamainden birini kullanarak yeni bir veritabanı oluşturulmasını kolaylaştırır. 

Harmanlamalar karakter tabanlı veri türleri için yerel ayar, kod sayfası, sıralama düzeni ve karakter duyarlılığı kuralları sağlar. Seçildiğinde, harmanlama bilgilerini gerektiren tüm sütunlar ve ifadeler, seçili harmanlamayı veritabanı ayarından devralınır. Varsayılan devralma, karakter tabanlı bir veri türü için farklı bir harmanlama açıkça belirtilerek geçersiz kılınabilir.

## <a name="changing-collation"></a>Harmanlamayı değiştirme
Varsayılan harmanlamayı değiştirmek için sağlama deneyiminde harmanlama alanına güncelleştirin.

Örneğin, Varsayılan harmanlamayı büyük/küçük harfe duyarlı olarak değiştirmek isterseniz, harmanlamayı SQL_Latin1_General_CP1_CI_AS SQL_Latin1_General_CP1_CS_AS olarak yeniden adlandırmanız yeterlidir. 

## <a name="list-of-unsupported-collation-types"></a>Desteklenmeyen harmanlama türlerinin listesi
*   Japanese_Bushu_Kakusu_140_BIN
*   Japanese_Bushu_Kakusu_140_BIN2
*   Japanese_Bushu_Kakusu_140_CI_AI_VSS
*   Japanese_Bushu_Kakusu_140_CI_AI_WS_VSS
*   Japanese_Bushu_Kakusu_140_CI_AI_KS_VSS
*   Japanese_Bushu_Kakusu_140_CI_AI_KS_WS_VSS
*   Japanese_Bushu_Kakusu_140_CI_AS_VSS
*   Japanese_Bushu_Kakusu_140_CI_AS_WS_VSS
*   Japanese_Bushu_Kakusu_140_CI_AS_KS_VSS
*   Japanese_Bushu_Kakusu_140_CI_AS_KS_WS_VSS
*   Japanese_Bushu_Kakusu_140_CS_AI_VSS
*   Japanese_Bushu_Kakusu_140_CS_AI_WS_VSS
*   Japanese_Bushu_Kakusu_140_CS_AI_KS_VSS
*   Japanese_Bushu_Kakusu_140_CS_AI_KS_WS_VSS
*   Japanese_Bushu_Kakusu_140_CS_AS_VSS
*   Japanese_Bushu_Kakusu_140_CS_AS_WS_VSS
*   Japanese_Bushu_Kakusu_140_CS_AS_KS_VSS
*   Japanese_Bushu_Kakusu_140_CS_AS_KS_WS_VSS
*   Japanese_Bushu_Kakusu_140_CI_AI
*   Japanese_Bushu_Kakusu_140_CI_AI_WS
*   Japanese_Bushu_Kakusu_140_CI_AI_KS
*   Japanese_Bushu_Kakusu_140_CI_AI_KS_WS
*   Japanese_Bushu_Kakusu_140_CI_AS
*   Japanese_Bushu_Kakusu_140_CI_AS_WS
*   Japanese_Bushu_Kakusu_140_CI_AS_KS
*   Japanese_Bushu_Kakusu_140_CI_AS_KS_WS
*   Japanese_Bushu_Kakusu_140_CS_AI
*   Japanese_Bushu_Kakusu_140_CS_AI_WS
*   Japanese_Bushu_Kakusu_140_CS_AI_KS
*   Japanese_Bushu_Kakusu_140_CS_AI_KS_WS
*   Japanese_Bushu_Kakusu_140_CS_AS
*   Japanese_Bushu_Kakusu_140_CS_AS_WS
*   Japanese_Bushu_Kakusu_140_CS_AS_KS
*   Japanese_Bushu_Kakusu_140_CS_AS_KS_WS
*   Japanese_XJIS_140_BIN
*   Japanese_XJIS_140_BIN2
*   Japanese_XJIS_140_CI_AI_VSS
*   Japanese_XJIS_140_CI_AI_WS_VSS
*   Japanese_XJIS_140_CI_AI_KS_VSS
*   Japanese_XJIS_140_CI_AI_KS_WS_VSS
*   Japanese_XJIS_140_CI_AS_VSS
*   Japanese_XJIS_140_CI_AS_WS_VSS
*   Japanese_XJIS_140_CI_AS_KS_VSS
*   Japanese_XJIS_140_CI_AS_KS_WS_VSS
*   Japanese_XJIS_140_CS_AI_VSS
*   Japanese_XJIS_140_CS_AI_WS_VSS
*   Japanese_XJIS_140_CS_AI_KS_VSS
*   Japanese_XJIS_140_CS_AI_KS_WS_VSS
*   Japanese_XJIS_140_CS_AS_VSS
*   Japanese_XJIS_140_CS_AS_WS_VSS
*   Japanese_XJIS_140_CS_AS_KS_VSS
*   Japanese_XJIS_140_CS_AS_KS_WS_VSS
*   Japanese_XJIS_140_CI_AI
*   Japanese_XJIS_140_CI_AI_WS
*   Japanese_XJIS_140_CI_AI_KS
*   Japanese_XJIS_140_CI_AI_KS_WS
*   Japanese_XJIS_140_CI_AS
*   Japanese_XJIS_140_CI_AS_WS
*   Japanese_XJIS_140_CI_AS_KS
*   Japanese_XJIS_140_CI_AS_KS_WS
*   Japanese_XJIS_140_CS_AI
*   Japanese_XJIS_140_CS_AI_WS
*   Japanese_XJIS_140_CS_AI_KS
*   Japanese_XJIS_140_CS_AI_KS_WS
*   Japanese_XJIS_140_CS_AS
*   Japanese_XJIS_140_CS_AS_WS
*   Japanese_XJIS_140_CS_AS_KS
*   Japanese_XJIS_140_CS_AS_KS_WS
*   SQL_EBCDIC1141_CP1_CS_AS
*   SQL_EBCDIC277_2_CP1_CS_AS

## <a name="checking-the-current-collation"></a>Geçerli harmanlama denetleniyor

Veritabanının geçerli harmanlamasını denetlemek için aşağıdaki T-SQL kod parçacığını çalıştırabilirsiniz:

```sql
SELECT DATABASEPROPERTYEX(DB_NAME(), 'Collation') AS Collation;
```

Özellik parametresi olarak ' harmanlama ' geçirildiğinde, DatabasePropertyEx işlevi belirtilen veritabanı için geçerli harmanlamayı döndürür. Daha fazla bilgi için bkz. [databasepropertyex](/sql/t-sql/functions/databasepropertyex-transact-sql).

