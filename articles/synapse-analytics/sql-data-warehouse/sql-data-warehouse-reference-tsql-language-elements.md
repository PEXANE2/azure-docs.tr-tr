---
title: T-SQL dil öğeleri
description: Synapse SQL havuzunda desteklenen T-SQL deyimleri için belgelere bağlantılar.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 06/13/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: f27a0a351e4a446dc950ac13f850bd14b2b3c65a
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80586409"
---
# <a name="t-sql-language-elements-supported-in-synapse-sql-pool"></a>Synapse SQL havuzunda desteklenen T-SQL dil öğeleri

Synapse SQL havuzunda desteklenen T-SQL dil öğeleri için belgelere bağlantılar.

## <a name="core-elements"></a>Temel elemanlar

* [sözdizimi kuralları](/sql/t-sql/language-elements/transact-sql-syntax-conventions-transact-sql)
* [nesne adlandırma kuralları](https://msdn.microsoft.com/library/ms175874.aspx)
* [ayrılmış anahtar kelimeler](https://msdn.microsoft.com/library/ms189822.aspx)
* [Alfabe](https://msdn.microsoft.com/library/ff848763.aspx)
* [Yorum](https://msdn.microsoft.com/library/ms181627.aspx)
* [Sabit](https://msdn.microsoft.com/library/ms179899.aspx)
* [veri türleri](https://msdn.microsoft.com/library/ms187752.aspx)
* [Yürütmek](https://msdn.microsoft.com/library/ms188332.aspx)
* [Ifa -de](https://msdn.microsoft.com/library/ms190286.aspx)
* [Öldür](https://msdn.microsoft.com/library/ms173730.aspx)
* [KİmLİk özelliği geçici çözüm](https://msdn.microsoft.com/library/ms186775.aspx)
* [Yazdırma](https://msdn.microsoft.com/library/ms176047.aspx)
* [Kullanın](https://msdn.microsoft.com/library/ms188366.aspx)

## <a name="batches-control-of-flow-and-variables"></a>Toplu iş, akış kontrolü ve değişkenler

* [Başlamak... Son -unda](https://msdn.microsoft.com/library/ms190487.aspx)
* [Mola](https://msdn.microsoft.com/library/ms181271.aspx)
* [Bildirmek@local_variable](https://msdn.microsoft.com/library/ms188927.aspx)
* [EĞER ... Başka](https://msdn.microsoft.com/library/ms182717.aspx)
* [Raıserror](https://msdn.microsoft.com/library/ms178592.aspx)
* [SET@local_variable](https://msdn.microsoft.com/library/ms189484.aspx)
* [Atmak](https://msdn.microsoft.com/library/ee677615.aspx)
* [Deneyin... Yakalamak](https://msdn.microsoft.com/library/ms175976.aspx)
* [Süre](https://msdn.microsoft.com/library/ms178642.aspx)

## <a name="operators"></a>İşleçler

* [+ (Ekle)](https://msdn.microsoft.com/library/ms178565.aspx)
* [+ (Dize Birleştirme)](https://msdn.microsoft.com/library/ms177561.aspx)
* [- (Negatif)](https://msdn.microsoft.com/library/ms189480.aspx)
* [- (Çıkar)](https://msdn.microsoft.com/library/ms189518.aspx)
* [* (Çarp)](https://msdn.microsoft.com/library/ms176019.aspx)
* [(Böl)](https://msdn.microsoft.com/library/ms175009.aspx)
* [Mod](https://msdn.microsoft.com/library/ms190279.aspx)

## <a name="wildcard-characters-to-match"></a>Joker karakter(ler) eşleşmek için

* [= (Eşittir)](https://msdn.microsoft.com/library/ms175118.aspx)
* [> (Büyük)](https://msdn.microsoft.com/library/ms178590.aspx)
* [< (Daha Az)](https://msdn.microsoft.com/library/ms179873.aspx)
* [>= (Büyük veya eşit)](https://msdn.microsoft.com/library/ms181567.aspx)
* [<= (Az veya eşit)](https://msdn.microsoft.com/library/ms174978.aspx)
* [<>  (Eşit değil)](https://msdn.microsoft.com/library/ms176020.aspx)
* [!= (Eşit değil)](https://msdn.microsoft.com/library/ms190296.aspx)
* [Ve](https://msdn.microsoft.com/library/ms188372.aspx)
* [Arasında](https://msdn.microsoft.com/library/ms187922.aspx)
* [Var](https://msdn.microsoft.com/library/ms188336.aspx)
* [Inç](https://msdn.microsoft.com/library/ms177682.aspx)
* [[DEĞIL] NULL](https://msdn.microsoft.com/library/ms188795.aspx)
* [LIKE](https://msdn.microsoft.com/library/ms179859.aspx)
* [Değil](https://msdn.microsoft.com/library/ms189455.aspx)
* [Veya](https://msdn.microsoft.com/library/ms188361.aspx)

### <a name="bitwise-operators"></a>Bit düzeyinde işleçler

* [& (Bitwise AND)](https://msdn.microsoft.com/library/ms174965.aspx)
* [| (Bitwise VEYA)](https://msdn.microsoft.com/library/ms186714.aspx)
* [^ (Bitwise özel OR)](https://msdn.microsoft.com/library/ms190277.aspx)
* [~ (Bitwise Değİl)](https://msdn.microsoft.com/library/ms173468.aspx)
* [^= (Bitwise Exclusive VEYA EQUALS)](https://msdn.microsoft.com/library/cc627413.aspx)
* [|= (Bitwise VEYA EQUALS)](https://msdn.microsoft.com/library/cc627409.aspx)
* [&= (Bitwise VE EQUALS)](https://msdn.microsoft.com/library/cc627427.aspx)

## <a name="functions"></a>İşlevler

* [@@DATEFIRST](https://msdn.microsoft.com/library/ms187766.aspx)
* [@@ERROR](https://msdn.microsoft.com/library/ms188790.aspx)
* [@@LANGUAGE](https://msdn.microsoft.com/library/ms177557.aspx)
* [@@SPID](https://msdn.microsoft.com/library/ms189535.aspx)
* [@@TRANCOUNT](https://msdn.microsoft.com/library/ms187967.aspx)
* [@@VERSION](https://msdn.microsoft.com/library/ms177512.aspx)
* [Abs](https://msdn.microsoft.com/library/ms189800.aspx)
* [ACOS](https://msdn.microsoft.com/library/ms178627.aspx)
* [Ascıı](https://msdn.microsoft.com/library/ms177545.aspx)
* [ASIN](https://msdn.microsoft.com/library/ms181581.aspx)
* [ATAN](https://msdn.microsoft.com/library/ms181746.aspx)
* [ATN2](https://msdn.microsoft.com/library/ms173854.aspx)
* [Bınary_checksum](https://msdn.microsoft.com/library/ms173784.aspx)
* [Durumda](https://msdn.microsoft.com/library/ms181765.aspx)
* [DÖKÜM ve CONVERT](https://msdn.microsoft.com/library/ms187928.aspx)
* [CEILING](https://msdn.microsoft.com/library/ms189818.aspx)
* [Char](https://msdn.microsoft.com/library/ms187323.aspx)
* [CHARINDEX](https://msdn.microsoft.com/library/ms186323.aspx)
* [Sağlama toplamı](https://msdn.microsoft.com/library/ms189788.aspx)
* [COALESCE](https://msdn.microsoft.com/library/ms190349.aspx)
* [COL_NAME](https://msdn.microsoft.com/library/ms174974.aspx)
* [HARMANLAMA ÖZELLIĞI](https://msdn.microsoft.com/library/ms190305.aspx)
* [Concat](https://msdn.microsoft.com/library/hh231515.aspx)
* [Çünkü](https://msdn.microsoft.com/library/ms188919.aspx)
* [COT](https://msdn.microsoft.com/library/ms188921.aspx)
* [Sayısı](https://msdn.microsoft.com/library/ms175997.aspx)
* [Count_bıg](https://msdn.microsoft.com/library/ms190317.aspx)
* [CUME_DIST](https://msdn.microsoft.com/library/hh231078.aspx)
* [CURRENT_TIMESTAMP](https://msdn.microsoft.com/library/ms188751.aspx)
* [Örnein](https://msdn.microsoft.com/library/ms176050.aspx)
* [Databasepropertyex](https://msdn.microsoft.com/library/ms186823.aspx)
* [Veriuzunluğu](https://msdn.microsoft.com/library/ms173486.aspx)
* [DATEADD](https://msdn.microsoft.com/library/ms186819.aspx)
* [DATEDIFF](https://msdn.microsoft.com/library/ms189794.aspx)
* [DATEFROMPARTS](https://msdn.microsoft.com/library/hh213228.aspx)
* [DATENAME](https://msdn.microsoft.com/library/ms174395.aspx)
* [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx)
* [DATETIME2FROMPARÇALAR](https://msdn.microsoft.com/library/hh213312.aspx)
* [DATETIMEFROMPARTS](https://msdn.microsoft.com/library/hh213233.aspx)
* [DATETIMEOFFSETFROMPARTS](https://msdn.microsoft.com/library/hh231077.aspx)
* [Gün](https://msdn.microsoft.com/library/ms176052.aspx)
* [Db_ıd](https://msdn.microsoft.com/library/ms186274.aspx)
* [Db_name](https://msdn.microsoft.com/library/ms189753.aspx)
* [DEGREES](https://msdn.microsoft.com/library/ms178566.aspx)
* [Dense_rank](https://msdn.microsoft.com/library/ms173825.aspx)
* [Fark](https://msdn.microsoft.com/library/ms188753.aspx)
* [EOMONTH](https://msdn.microsoft.com/library/hh213020.aspx)
* [Error_message](https://msdn.microsoft.com/library/ms190358.aspx)
* [Error_number](https://msdn.microsoft.com/library/ms175069.aspx)
* [Error_procedure](https://msdn.microsoft.com/library/ms188398.aspx)
* [ERROR_SEVERITY](https://msdn.microsoft.com/library/ms178567.aspx)
* [Error_state](https://msdn.microsoft.com/library/ms180031.aspx)
* [Exp](https://msdn.microsoft.com/library/ms179857.aspx)
* [FIRST_VALUE](https://msdn.microsoft.com/library/hh213018.aspx)
* [Kat](https://msdn.microsoft.com/library/ms178531.aspx)
* [Getdate](https://msdn.microsoft.com/library/ms188383.aspx)
* [Getutcdate](https://msdn.microsoft.com/library/ms178635.aspx)
* [HAS_DBACCESS](https://msdn.microsoft.com/library/ms187718.aspx)
* [HASHBYTES](https://msdn.microsoft.com/library/ms174415.aspx)
* [ındexproperty](https://msdn.microsoft.com/library/ms187729.aspx)
* [ısdate](https://msdn.microsoft.com/library/ms187347.aspx)
* [ısnull](https://msdn.microsoft.com/library/ms184325.aspx)
* [ısnumerıc](https://msdn.microsoft.com/library/ms186272.aspx)
* [LAG](https://msdn.microsoft.com/library/hh231256.aspx)
* [LAST_VALUE](https://msdn.microsoft.com/library/hh231517.aspx)
* [Kurşun](https://msdn.microsoft.com/library/hh213125.aspx)
* [LEFT](https://msdn.microsoft.com/library/ms177601.aspx)
* [LEN](https://msdn.microsoft.com/library/ms190329.aspx)
* [Günlük](https://msdn.microsoft.com/library/ms190319.aspx)
* [LOG10](https://msdn.microsoft.com/library/ms175121.aspx)
* [LOWER](https://msdn.microsoft.com/library/ms174400.aspx)
* [LTRIM](https://msdn.microsoft.com/library/ms177827.aspx)
* [Max](https://msdn.microsoft.com/library/ms187751.aspx)
* [Dk](https://msdn.microsoft.com/library/ms179916.aspx)
* [Ay](https://msdn.microsoft.com/library/ms187813.aspx)
* [Nchar](https://msdn.microsoft.com/library/ms182673.aspx)
* [NTILE](https://msdn.microsoft.com/library/ms175126.aspx)
* [Nullıf](https://msdn.microsoft.com/library/ms177562.aspx)
* [Object_ıd](https://msdn.microsoft.com/library/ms190328.aspx)
* [OBJECT_NAME](https://msdn.microsoft.com/library/ms186301.aspx)
* [Objectproperty](https://msdn.microsoft.com/library/ms176105.aspx)
* [OIBJECTPROPERTYEX](https://msdn.microsoft.com/library/ms188390.aspx)
* [ODBCS skaler fonksiyonları](https://msdn.microsoft.com/library/bb630290.aspx)
* [OVER yan tümcesi](https://msdn.microsoft.com/library/ms189461.aspx)
* [PARSENAME](https://msdn.microsoft.com/library/ms188006.aspx)
* [PATINDEX](https://msdn.microsoft.com/library/ms188395.aspx)
* [PERCENTILE_CONT](https://msdn.microsoft.com/library/hh231473.aspx)
* [PERCENTILE_DISC](https://msdn.microsoft.com/library/hh231327.aspx)
* [PERCENT_RANK](https://msdn.microsoft.com/library/hh213573.aspx)
* [PI](https://msdn.microsoft.com/library/ms189512.aspx)
* [POWER](https://msdn.microsoft.com/library/ms174276.aspx)
* [QUOTENAME](https://msdn.microsoft.com/library/ms176114.aspx)
* [RADIANS](https://msdn.microsoft.com/library/ms189742.aspx)
* [RAND](https://msdn.microsoft.com/library/ms177610.aspx)
* [Rütbe](https://msdn.microsoft.com/library/ms176102.aspx)
* [REPLACE](https://msdn.microsoft.com/library/ms186862.aspx)
* [ÇOĞALTILAN](https://msdn.microsoft.com/library/ms174383.aspx)
* [Ters](https://msdn.microsoft.com/library/ms180040.aspx)
* [RIGHT](https://msdn.microsoft.com/library/ms177532.aspx)
* [ROUND](https://msdn.microsoft.com/library/ms175003.aspx)
* [Row_number](https://msdn.microsoft.com/library/ms186734.aspx)
* [RTRIM](https://msdn.microsoft.com/library/ms178660.aspx)
* [SCHEMA_ID](https://msdn.microsoft.com/library/ms188797.aspx)
* [Schema_name](https://msdn.microsoft.com/library/ms175068.aspx)
* [Serverproperty](https://msdn.microsoft.com/library/ms174396.aspx)
* [SESSION_USER](https://msdn.microsoft.com/library/ms177587.aspx)
* [SIGN](https://msdn.microsoft.com/library/ms188420.aspx)
* [SIN](https://msdn.microsoft.com/library/ms188377.aspx)
* [SMALLDATETIMEFROMPARTS](https://msdn.microsoft.com/library/hh213396.aspx)
* [Soundex](https://msdn.microsoft.com/library/ms187384.aspx)
* [Alanı](https://msdn.microsoft.com/library/ms187950.aspx)
* [SQL_VARIANT_PROPERTY](https://msdn.microsoft.com/library/ms178550.aspx)
* [Karekök](https://msdn.microsoft.com/library/ms176108.aspx)
* [Meydanı](https://msdn.microsoft.com/library/ms173569.aspx)
* [STATS_DATE](https://msdn.microsoft.com/library/ms190330.aspx)
* [STDEV](https://msdn.microsoft.com/library/ms190474.aspx)
* [STDEVP](https://msdn.microsoft.com/library/ms176080.aspx)
* [Str](https://msdn.microsoft.com/library/ms189527.aspx)
* [Şey](https://msdn.microsoft.com/library/ms188043.aspx)
* [SUBSTRING](https://msdn.microsoft.com/library/ms187748.aspx)
* [Toplam](https://msdn.microsoft.com/library/ms187810.aspx)
* [Suser_sname](https://msdn.microsoft.com/library/ms174427.aspx)
* [SWITCHOFFSET](https://msdn.microsoft.com/library/bb677244.aspx)
* [Sysdatetıme](https://msdn.microsoft.com/library/bb630353.aspx)
* [Sysdatetımeoffset](https://msdn.microsoft.com/library/bb677334.aspx)
* [System_user](https://msdn.microsoft.com/library/ms179930.aspx)
* [Sysutcdatetıme](https://msdn.microsoft.com/library/bb630387.aspx)
* [TAN](https://msdn.microsoft.com/library/ms190338.aspx)
* [TERTIARY_WEIGHTS](https://msdn.microsoft.com/library/ms186881.aspx)
* [ZAMANTAN PARÇALAR](https://msdn.microsoft.com/library/hh213398.aspx)
* [TODATETIMEOFFSET](https://msdn.microsoft.com/library/bb630335.aspx)
* [TYPE_ID](https://msdn.microsoft.com/library/ms181628.aspx)
* [Type_name](https://msdn.microsoft.com/library/ms189750.aspx)
* [Typeproperty](https://msdn.microsoft.com/library/ms188419.aspx)
* [Unicode](https://msdn.microsoft.com/library/ms180059.aspx)
* [UPPER](https://msdn.microsoft.com/library/ms180055.aspx)
* [KULLANICI](https://msdn.microsoft.com/library/ms186738.aspx)
* [Kullanıcı_adı](https://msdn.microsoft.com/library/ms188014.aspx)
* [VAR](https://msdn.microsoft.com/library/ms186290.aspx)
* [VARP](https://msdn.microsoft.com/library/ms188735.aspx)
* [Yıl](https://msdn.microsoft.com/library/ms186313.aspx)
* [Xact_state](https://msdn.microsoft.com/library/ms189797.aspx)

## <a name="transactions"></a>İşlemler

* [Hareket](https://msdn.microsoft.com/library/mt204031.aspx)

## <a name="diagnostic-sessions"></a>Tanı lama seansları

* [TANıLAMA OTURUMU OLUŞTUR](https://msdn.microsoft.com/library/mt204029.aspx)

## <a name="procedures"></a>Yordamlar

* [Sp_addrolemember](https://msdn.microsoft.com/library/ms187750.aspx)
* [sp_columns](https://msdn.microsoft.com/library/ms176077.aspx)
* [Sp_configure](https://msdn.microsoft.com/library/ms188787.aspx)
* [sp_datatype_info_90](https://msdn.microsoft.com/library/mt204014.aspx)
* [Sp_droprolemember](https://msdn.microsoft.com/library/ms188369.aspx)
* [sp_execute](https://msdn.microsoft.com/library/ff848746.aspx)
* [Sp_executesql](https://msdn.microsoft.com/library/ms188001.aspx)
* [sp_fkeys](https://msdn.microsoft.com/library/ms175090.aspx)
* [sp_pdw_add_network_credentials](https://msdn.microsoft.com/library/mt204011.aspx)
* [sp_pdw_database_encryption](https://msdn.microsoft.com/library/mt219360.aspx)
* [sp_pdw_database_encryption_regenerate_system_keys](https://msdn.microsoft.com/library/mt204033.aspx)
* [sp_pdw_log_user_data_masking](https://msdn.microsoft.com/library/mt204023.aspx)
* [sp_pdw_remove_network_credentials](https://msdn.microsoft.com/library/mt204038.aspx)
* [sp_pkeys](https://msdn.microsoft.com/library/ms189813.aspx)
* [sp_prepare](https://msdn.microsoft.com/library/ff848808.aspx)
* [Sp_spaceused](https://msdn.microsoft.com/library/ms188776.aspx)
* [sp_special_columns_100](https://msdn.microsoft.com/library/mt204025.aspx)
* [sp_sproc_columns](https://msdn.microsoft.com/library/ms182705.aspx)
* [sp_statistics](https://msdn.microsoft.com/library/ms173842.aspx)
* [sp_tables](https://msdn.microsoft.com/library/ms186250.aspx)
* [sp_unprepare](https://msdn.microsoft.com/library/ff848735.aspx)

## <a name="set-statements"></a>SET deyimleri

* [SET ANSI_DEFAULTS](https://msdn.microsoft.com/library/ms188340.aspx)
* [SET ANSI_NULL_DFLT_OFF](https://msdn.microsoft.com/library/ms187356.aspx)
* [SET ANSI_NULL_DFLT_ON](https://msdn.microsoft.com/library/ms187375.aspx)
* [SET ANSI_NULLS](https://msdn.microsoft.com/library/ms188048.aspx)
* [SET ANSI_PADDING](https://msdn.microsoft.com/library/ms187403.aspx)
* [SET ANSI_WARNINGS](https://msdn.microsoft.com/library/ms190368.aspx)
* [SET ARITHABORT](https://msdn.microsoft.com/library/ms190306.aspx)
* [SET ARITMA](https://msdn.microsoft.com/library/ms184341.aspx)
* [SET CONCAT_NULL_YIELDS_NULL](https://msdn.microsoft.com/library/ms176056.aspx)
* [ÖNCE TARIHI AYARLA](https://msdn.microsoft.com/library/ms181598.aspx)
* [DATEFORMAT'ı AYARLA](https://msdn.microsoft.com/library/ms189491.aspx)
* [SET FMTONLY](https://msdn.microsoft.com/library/ms173839.aspx)
* [SET IMPLICIT_TRANSACITONS](https://msdn.microsoft.com/library/ms187807.aspx)
* [SET LOCK_TIMEOUT](https://msdn.microsoft.com/library/ms189470.aspx)
* [SET NUMBERIC_ROUNDABORT](https://msdn.microsoft.com/library/ms188791.aspx)
* [SET QUOTED_IDENTIFIER](https://msdn.microsoft.com/library/ms174393.aspx)
* [SATıR SAYıSıNı AYARLA](https://msdn.microsoft.com/library/ms188774.aspx)
* [TEXTSIZE'ı AYARLA](https://msdn.microsoft.com/library/ms186238.aspx)
* [HAREKET YALıTıM DÜZEYINI AYARLA](https://msdn.microsoft.com/library/ms173763.aspx)
* [SET XACT_ABORT](https://msdn.microsoft.com/library/ms188792.aspx)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla başvuru bilgisi için [Synapse SQL havuzundaki T-SQL deyimlerine](sql-data-warehouse-reference-tsql-statements.md)ve [Synapse SQL havuzundaki Sistem görünümlerine](sql-data-warehouse-reference-tsql-system-views.md)bakın.

