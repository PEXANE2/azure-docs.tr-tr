---
title: T-SQL deyimleri-Azure SQL veri ambarı | Microsoft Docs
description: Azure SQL veri ambarı 'nda desteklenen T-SQL deyimlerine yönelik belgelere bağlantılar.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: query
ms.date: 05/01/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 5704ee4bf84b396dcef5a4f9edd887128ba95fb1
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68479381"
---
# <a name="t-sql-statements-supported-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı 'nda desteklenen T-SQL deyimleri
Azure SQL veri ambarı 'nda desteklenen T-SQL deyimlerine yönelik belgelere bağlantılar.

## <a name="data-definition-language-ddl-statements"></a>Veri tanımlama dili (DDL) deyimleri
* [ALTER DATABASE](https://msdn.microsoft.com/library/mt204042.aspx)
* [ALTER INDEX](https://msdn.microsoft.com/library/ms188388.aspx)
* [GERÇEKLEŞTIRILMIŞ Görünümü Değiştir](/sql/t-sql/statements/alter-materialized-view-transact-sql?view=azure-sqldw-latest) Önizle 
* [DEĞIŞTIRME YORDAMI](https://msdn.microsoft.com/library/ms189762.aspx)
* [ŞEMAYI DEĞIŞTIR](https://msdn.microsoft.com/library/ms173423.aspx)
* [ALTER TABLE](https://msdn.microsoft.com/library/ms190273.aspx)
* [COLUMNSTORE DIZINI OLUŞTUR](https://msdn.microsoft.com/library/gg492153.aspx)
* [CREATE DATABASE](https://msdn.microsoft.com/library/mt204021.aspx)
* [VERITABANI KAPSAMLI KIMLIK BILGISI OLUŞTUR](https://msdn.microsoft.com/library/mt270260.aspx)
* [DIŞ VERI KAYNAĞI OLUŞTUR](https://msdn.microsoft.com/library/dn935022.aspx)
* [CREATE EXTERNAL FILE FORMAT](https://msdn.microsoft.com/library/dn935026.aspx)
* [DIŞ TABLO OLUŞTUR](https://msdn.microsoft.com/library/dn935021.aspx)
* [IŞLEV OLUŞTUR](https://msdn.microsoft.com/library/mt203952.aspx)
* [DIZIN OLUŞTUR](https://msdn.microsoft.com/library/ms188783.aspx)
* [GERÇEKLEŞTIRILMIŞ görünümü Seç](/sql/t-sql/statements/create-materialized-view-as-select-transact-sql?view=azure-sqldw-latest) Önizle 
* [YORDAM OLUŞTUR](https://msdn.microsoft.com/library/ms187926.aspx)
* [ŞEMA OLUŞTUR](https://msdn.microsoft.com/library/ms189462.aspx)
* [CREATE STATISTICS](https://msdn.microsoft.com/library/ms188038.aspx)
* [CREATE TABLE](https://msdn.microsoft.com/library/mt203953.aspx)
* [CREATE TABLE AS SELECT](https://msdn.microsoft.com/library/mt204041.aspx)
* [GÖRÜNÜM OLUŞTUR](https://msdn.microsoft.com/library/ms187956.aspx)
* [IŞ YÜKÜ SINIFLANDIRICISI OLUŞTURMA](/sql/t-sql/statements/create-workload-classifier-transact-sql)
* [DIŞ VERI KAYNAĞINI BIRAK](https://msdn.microsoft.com/library/mt146367.aspx)
* [DIŞ DOSYA BIÇIMINI BIRAK](https://msdn.microsoft.com/library/mt146379.aspx)
* [DIŞ TABLO BIRAK](https://msdn.microsoft.com/library/mt130698.aspx)
* [DROP INDEX](https://msdn.microsoft.com/library/ms176118.aspx)
* [PROSEDÜRÜ BIRAK](https://msdn.microsoft.com/library/ms174969.aspx)
* [BIRAKMA ISTATISTIKLERI](https://msdn.microsoft.com/library/ms175075.aspx)
* [TABLOYU BIRAK](https://msdn.microsoft.com/library/ms173790.aspx)
* [ŞEMAYI BIRAK](https://msdn.microsoft.com/library/ms186751.aspx)
* [GÖRÜNÜMÜ BIRAK](https://msdn.microsoft.com/library/ms173492.aspx)
* [BIRAKMA IŞ YÜKÜ SINIFLANDIRICISI](/sql/t-sql/statements/drop-workload-classifier-transact-sql)
* [YENIDEN ADLANDIR](https://msdn.microsoft.com/library/mt631611.aspx)
* [RESULT_SET_CACHING AYARLA](/sql/t-sql/statements/set-result-set-caching-transact-sql) 
* [TRUNCATE TABLE](https://msdn.microsoft.com/library/ms177570.aspx)
* [UPDATE STATISTICS](https://msdn.microsoft.com/library/ms187348.aspx)

## <a name="data-manipulation-language-dml-statements"></a>Veri Işleme dili (DML) deyimleri
* [DELETE](https://msdn.microsoft.com/library/ms189835.aspx)
* [INSERT](https://msdn.microsoft.com/library/ms174335.aspx)
* [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx)

## <a name="database-console-commands"></a>Veritabanı Konsolu komutları
* [DBCC DROPCLEANARABELLEKLERI](https://msdn.microsoft.com/library/ms187762.aspx)
* [DBCC DROPRESULTSETCACHE](/sql/t-sql/database-console-commands/dbcc-dropresultsetcache-transact-sql?view=azure-sqldw-latest) Önizle
* [DBCC FREEPROCCACHE](https://msdn.microsoft.com/library/mt204018.aspx)
* [DBCC SHRINKLOG](https://msdn.microsoft.com/library/mt204020.aspx)
* [DBCC PDW_SHOWEXECUTIONPLAN](https://msdn.microsoft.com/library/mt204017.aspx)
* [DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD](/sql/t-sql/database-console-commands/dbcc-pdw-showmaterializedviewoverhead-transact-sql?view=azure-sqldw-latest)
* [DBCC SHOWRESULTCACHESPACEKULLANDıNıZ](/sql/t-sql/database-console-commands/dbcc-showresultcachespaceused-transact-sql) Önizle
* [DBCC PDW_SHOWPARTITIONSTATS](https://msdn.microsoft.com/library/mt204013.aspx)
* [DBCC PDW_SHOWSPACEUSED](https://msdn.microsoft.com/library/mt204028.aspx)
* [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/mt204043.aspx)

## <a name="query-statements"></a>Sorgu deyimleri
* [SEÇIN](https://msdn.microsoft.com/library/ms189499.aspx)
* [Common_table_expression Ile](https://msdn.microsoft.com/library/ms175972.aspx)
* [EXCEPT ve ıNTERSECT](https://msdn.microsoft.com/library/ms188055.aspx)
* [AÇIKLAMAK](https://msdn.microsoft.com/library/mt631615.aspx)
* [KAYNAK](https://msdn.microsoft.com/library/ms177634.aspx)
* [PIVOT ve UNPıVOT kullanma](https://msdn.microsoft.com/library/ms177410.aspx)
* [GRUPLANDIRMA ÖLÇÜTÜ](https://msdn.microsoft.com/library/ms177673.aspx)
* [AÇMADAN](https://msdn.microsoft.com/library/ms180199.aspx)
* [SIRALAMA ÖLÇÜTÜ](https://msdn.microsoft.com/library/ms188385.aspx)
* [OPTION](https://msdn.microsoft.com/library/ms190322.aspx)
* [BIRLEŞIM](https://msdn.microsoft.com/library/ms180026.aspx)
* [OLMADIĞI](https://msdn.microsoft.com/library/ms188047.aspx)
* [SAYFANIN ÜSTÜ](https://msdn.microsoft.com/library/ms189463.aspx)
* [Masını](https://msdn.microsoft.com/library/mt631614.aspx)
* [Arama koşulu](https://msdn.microsoft.com/library/ms173545.aspx)
* [Alt](https://msdn.microsoft.com/library/mt631613.aspx)

## <a name="security-statements"></a>Güvenlik deyimleri
* İzinler: [VERME](https://msdn.microsoft.com/library/ms187965.aspx), [REDDETME](https://msdn.microsoft.com/library/ms188338.aspx), [İPTAL ETME](https://msdn.microsoft.com/library/ms187728.aspx)
* [DEĞIŞTIRME YETKILENDIRMESI](https://msdn.microsoft.com/library/ms187359.aspx)
* [SERTIFIKAYI DEĞIŞTIR](https://msdn.microsoft.com/library/ms189511.aspx)
* [ALTER DATABASE ŞIFRELEME ANAHTARI](https://msdn.microsoft.com/library/bb630389.aspx)
* [DEĞIŞTIRME OTURUMU AÇMA](https://msdn.microsoft.com/library/ms189828.aspx)
* [DEĞIŞIKLIK ANA ANAHTARI](https://msdn.microsoft.com/library/ms186937.aspx)
* [ROLÜ DEĞIŞTIR](https://msdn.microsoft.com/library/ms189775.aspx)
* [KULLANICI DEĞIŞTIR](https://msdn.microsoft.com/library/ms176060.aspx)
* [YEDEKLEME SERTIFIKASI](https://msdn.microsoft.com/library/ms178578.aspx)
* [ANA ANAHTARI KAPAT](https://msdn.microsoft.com/library/ms188387.aspx)
* [SERTIFIKA OLUŞTUR](https://msdn.microsoft.com/library/ms187798.aspx)
* [VERITABANI ŞIFRELEME ANAHTARI OLUŞTUR](https://msdn.microsoft.com/library/bb677241.aspx)
* [OTURUM AÇMA OLUŞTUR](https://msdn.microsoft.com/library/ms189751.aspx)
* [ANA ANAHTAR OLUŞTUR](https://msdn.microsoft.com/library/ms174382.aspx)
* [ROL OLUŞTUR](https://msdn.microsoft.com/library/ms187936.aspx)
* [KULLANICI OLUŞTUR](https://msdn.microsoft.com/library/ms173463.aspx)
* [SERTIFIKAYI BIRAK](https://msdn.microsoft.com/library/ms179906.aspx)
* [BIRAKMA VERITABANI ŞIFRELEME ANAHTARI](https://msdn.microsoft.com/library/bb630256.aspx)
* [OTURUMU BIRAK](https://msdn.microsoft.com/library/ms188012.aspx)
* [ANA ANAHTAR BIRAK](https://msdn.microsoft.com/library/ms180071.aspx)
* [ROLÜ BIRAK](https://msdn.microsoft.com/library/ms174988.aspx)
* [KULLANICIYI BIRAK](https://msdn.microsoft.com/library/ms189438.aspx)
* [ANA ANAHTARI AÇ](https://msdn.microsoft.com/library/ms174433.aspx)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla başvuru bilgisi için bkz. [Azure SQL veri ambarı 'Nda T-SQL dil öğeleri](sql-data-warehouse-reference-tsql-language-elements.md)ve [Azure SQL veri ambarı 'nda sistem görünümleri](sql-data-warehouse-reference-tsql-system-views.md).
