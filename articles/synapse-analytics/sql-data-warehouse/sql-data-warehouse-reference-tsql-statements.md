---
title: T-SQL bildirimleri
description: SQL Analytics'te desteklenen T-SQL deyimleri için belgelere bağlantılar.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 05/01/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 53609049f69f0a3d50430ef512a5bf4a8ffd6d82
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80351348"
---
# <a name="t-sql-statements-supported-in-sql-analytics"></a>SQL Analytics'te desteklenen T-SQL deyimleri
SQL Analytics'te desteklenen T-SQL deyimleri için belgelere bağlantılar.

## <a name="data-definition-language-ddl-statements"></a>Veri Tanımı Dili (DDL) deyimleri
* [VERITABANıNı DEĞIŞTIR](https://msdn.microsoft.com/library/mt204042.aspx)
* [ALTER INDEX](https://msdn.microsoft.com/library/ms188388.aspx)
* [ALTER MATERIALIZED VIEW](/sql/t-sql/statements/alter-materialized-view-transact-sql?view=azure-sqldw-latest) (Önizleme) 
* [ALTER PROSEDÜRÜ](https://msdn.microsoft.com/library/ms189762.aspx)
* [ALTER ŞEMA](https://msdn.microsoft.com/library/ms173423.aspx)
* [ALTER TABLE](https://msdn.microsoft.com/library/ms190273.aspx)
* [SÜTUN MAĞAZA DIZINİ OLUŞTUR](https://msdn.microsoft.com/library/gg492153.aspx)
* [VERITABANı OLUŞTUR](https://msdn.microsoft.com/library/mt204021.aspx)
* [VERITABANı KAPSAMLI Kimlik BILGILERI OLUŞTURMA](https://msdn.microsoft.com/library/mt270260.aspx)
* [DıŞ VERI KAYNAĞı OLUŞTURMA](https://msdn.microsoft.com/library/dn935022.aspx)
* [CREATE EXTERNAL FILE FORMAT](https://msdn.microsoft.com/library/dn935026.aspx)
* [DıŞ TABLO OLUŞTURMA](https://msdn.microsoft.com/library/dn935021.aspx)
* [İşLEV OLUŞTUR](https://msdn.microsoft.com/library/mt203952.aspx)
* [CREATE INDEX](https://msdn.microsoft.com/library/ms188783.aspx)
* [SELECT OLARAK MATERYALIZE GÖRÜNÜM OLUŞTURMA](/sql/t-sql/statements/create-materialized-view-as-select-transact-sql?view=azure-sqldw-latest) (Önizleme) 
* [YORDAM OLUŞTURMA](https://msdn.microsoft.com/library/ms187926.aspx)
* [ŞEMA YARAT](https://msdn.microsoft.com/library/ms189462.aspx)
* [İstATİstİk LER YARATIN](https://msdn.microsoft.com/library/ms188038.aspx)
* [TABLO OLUŞTUR](https://msdn.microsoft.com/library/mt203953.aspx)
* [CREATE TABLE AS SELECT](https://msdn.microsoft.com/library/mt204041.aspx)
* [GÖRÜNÜM OLUŞTUR](https://msdn.microsoft.com/library/ms187956.aspx)
* [İŞ YÜKÜ SıNıFLANDıRıCı SıYRıK OLUŞTURMA](/sql/t-sql/statements/create-workload-classifier-transact-sql)
* [Dış VERI KAYNAĞıNı Bırak](https://msdn.microsoft.com/library/mt146367.aspx)
* [Dış DOSYA BIÇIMINI BıRAK](https://msdn.microsoft.com/library/mt146379.aspx)
* [DıŞ TABLOYU Bırak](https://msdn.microsoft.com/library/mt130698.aspx)
* [DROP INDEX](https://msdn.microsoft.com/library/ms176118.aspx)
* [DAMLA PROSEDÜRÜ](https://msdn.microsoft.com/library/ms174969.aspx)
* [DAMLA İstATİstİkLerİ](https://msdn.microsoft.com/library/ms175075.aspx)
* [DAMLA TABLOSU](https://msdn.microsoft.com/library/ms173790.aspx)
* [DAMLA ŞEMA](https://msdn.microsoft.com/library/ms186751.aspx)
* [DAMLA GÖRÜNÜMÜ](https://msdn.microsoft.com/library/ms173492.aspx)
* [BıRAK İŞ YÜKÜ SınıfLANDıRıCı](/sql/t-sql/statements/drop-workload-classifier-transact-sql)
* [Yeni -den adlandırmak](https://msdn.microsoft.com/library/mt631611.aspx)
* [SET RESULT_SET_CACHING](/sql/t-sql/statements/set-result-set-caching-transact-sql) 
* [BUKATET TABLOSU](https://msdn.microsoft.com/library/ms177570.aspx)
* [UPDATE STATISTICS](https://msdn.microsoft.com/library/ms187348.aspx)

## <a name="data-manipulation-language-dml-statements"></a>Veri Düzenleme Dili (DML) deyimleri
* [DELETE](https://msdn.microsoft.com/library/ms189835.aspx)
* [Ekle](https://msdn.microsoft.com/library/ms174335.aspx)
* [Güncelleştirme](https://msdn.microsoft.com/library/ms177523.aspx)

## <a name="database-console-commands"></a>Veritabanı Konsol Komutları
* [DBCC DROPCLEANBUFFERS](https://msdn.microsoft.com/library/ms187762.aspx)
* [DBCC DROPRESULTSETCACHE](/sql/t-sql/database-console-commands/dbcc-dropresultsetcache-transact-sql?view=azure-sqldw-latest) (Önizleme)
* [DBCC FREEPROCCACHE](https://msdn.microsoft.com/library/mt204018.aspx)
* [DBCC SHRINKLOG](https://msdn.microsoft.com/library/mt204020.aspx)
* [DBCC PDW_SHOWEXECUTIONPLAN](https://msdn.microsoft.com/library/mt204017.aspx)
* [DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD](/sql/t-sql/database-console-commands/dbcc-pdw-showmaterializedviewoverhead-transact-sql?view=azure-sqldw-latest)
* [DBCC SHOWRESULTCACHESPACEUSED](/sql/t-sql/database-console-commands/dbcc-showresultcachespaceused-transact-sql) (Önizleme)
* [DBCC PDW_SHOWPARTITIONSTATS](https://msdn.microsoft.com/library/mt204013.aspx)
* [DBCC PDW_SHOWSPACEUSED](https://msdn.microsoft.com/library/mt204028.aspx)
* [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/mt204043.aspx)

## <a name="query-statements"></a>Sorgu ifadeleri
* [Seçin](https://msdn.microsoft.com/library/ms189499.aspx)
* [Common_table_expression](https://msdn.microsoft.com/library/ms175972.aspx)
* [HARIÇ ve KESİşMe](https://msdn.microsoft.com/library/ms188055.aspx)
* [Açıklaya](https://msdn.microsoft.com/library/mt631615.aspx)
* [Kaynak](https://msdn.microsoft.com/library/ms177634.aspx)
* [PIVOT ve UNPIVOT'u kullanma](https://msdn.microsoft.com/library/ms177410.aspx)
* [GROUP BY](https://msdn.microsoft.com/library/ms177673.aspx)
* [HAVING](https://msdn.microsoft.com/library/ms180199.aspx)
* [SİPARİŞ VEREN](https://msdn.microsoft.com/library/ms188385.aspx)
* [OPTION](https://msdn.microsoft.com/library/ms190322.aspx)
* [Birliği](https://msdn.microsoft.com/library/ms180026.aspx)
* [Nerede](https://msdn.microsoft.com/library/ms188047.aspx)
* [Sayfanın Üstü](https://msdn.microsoft.com/library/ms189463.aspx)
* [Diğer ad kullanımı](https://msdn.microsoft.com/library/mt631614.aspx)
* [Arama koşulu](https://msdn.microsoft.com/library/ms173545.aspx)
* [Alt sorgular](https://msdn.microsoft.com/library/mt631613.aspx)

## <a name="security-statements"></a>Güvenlik deyimleri
* İzinler: [HIBE](https://msdn.microsoft.com/library/ms187965.aspx), [İNKAR](https://msdn.microsoft.com/library/ms188338.aspx), [İptal](https://msdn.microsoft.com/library/ms187728.aspx)
* [YETKILENDIRMEYI DEĞIŞTIR](https://msdn.microsoft.com/library/ms187359.aspx)
* [SERTIFIKAYI DEĞIŞTIR](https://msdn.microsoft.com/library/ms189511.aspx)
* [VERITABANı ŞIFRELEME ANAHTARıNı DEĞIŞTIR](https://msdn.microsoft.com/library/bb630389.aspx)
* [ALTER Gİrİş](https://msdn.microsoft.com/library/ms189828.aspx)
* [ALTER MASTER ANAHTARI](https://msdn.microsoft.com/library/ms186937.aspx)
* [ROLÜ DEĞIŞTIR](https://msdn.microsoft.com/library/ms189775.aspx)
* [ALTER KULLANıCı](https://msdn.microsoft.com/library/ms176060.aspx)
* [YEDEKLEME SERTIFIKASı](https://msdn.microsoft.com/library/ms178578.aspx)
* [MASTER TUŞUNU KAPAT](https://msdn.microsoft.com/library/ms188387.aspx)
* [SERTIFIKA OLUŞTUR](https://msdn.microsoft.com/library/ms187798.aspx)
* [VERITABANı ŞIFRELEME ANAHTARı OLUŞTURMA](https://msdn.microsoft.com/library/bb677241.aspx)
* [Gİrİş YAP](https://msdn.microsoft.com/library/ms189751.aspx)
* [ANA ANAHTAR OLUŞTUR](https://msdn.microsoft.com/library/ms174382.aspx)
* [ROL OLUŞTUR](https://msdn.microsoft.com/library/ms187936.aspx)
* [KULLANıCı OLUŞTUR](https://msdn.microsoft.com/library/ms173463.aspx)
* [DROP SERTIFIKASı](https://msdn.microsoft.com/library/ms179906.aspx)
* [Veritabanı ŞIFRELEME ANAHTARıNı BıRAK](https://msdn.microsoft.com/library/bb630256.aspx)
* [DROP Gİrİş](https://msdn.microsoft.com/library/ms188012.aspx)
* [DAMLA ANA ANAHTAR](https://msdn.microsoft.com/library/ms180071.aspx)
* [DROP ROLÜ](https://msdn.microsoft.com/library/ms174988.aspx)
* [DROP KULLANıCı](https://msdn.microsoft.com/library/ms189438.aspx)
* [ANA ANAHTARı AÇ](https://msdn.microsoft.com/library/ms174433.aspx)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla referans bilgisi için SQL [Analytics'teki T-SQL dil öğelerine](sql-data-warehouse-reference-tsql-language-elements.md)ve [SQL Analytics'teki Sistem görünümlerine](sql-data-warehouse-reference-tsql-system-views.md)bakın.
