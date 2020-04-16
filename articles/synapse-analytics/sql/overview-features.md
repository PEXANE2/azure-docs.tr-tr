---
title: Azure Synapse SQL'de T-SQL özellik farklılıkları
description: Synapse SQL'de bulunan Transact-SQL özelliklerinin listesi.
services: synapse analytics
author: jovanpop-msft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: jovanpop
ms.reviewer: jrasnick
ms.openlocfilehash: b3e4d705bea7243966959038cf15373097c73ebc
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424898"
---
# <a name="transact-sql-features-supported-in-azure-synapse-sql"></a>Azure Synapse SQL'de desteklenen Transact-SQL özellikleri

Azure Synapse SQL, T-SQL dilini kullanarak verilerinizi sorgulayabilmenizi ve analiz etmenizi sağlayan büyük bir veri analitik hizmetidir. Veri analizi için SQL Server ve Azure SQL Veritabanı'nda kullanılan SQL dilinin standart ANSI uyumlu lehçesini kullanabilirsiniz. 

Transact-SQL dili Synapse SQL sunucusuz olarak kullanılır ve sağlanan model farklı nesnelere başvuru yapabilir ve desteklenen özellikler kümesinde bazı farklılıklar vardır. Bu sayfada, Synapse SQL tüketim modelleri arasındaki üst düzey Transact-SQL dil farklarını bulabilirsiniz.

## <a name="database-objects"></a>Veritabanı nesneleri

Synapse SQL'deki tüketim modelleri farklı veritabanı nesnelerini kullanmanıza olanak tanır. Desteklenen nesne türlerinin karşılaştırılması aşağıdaki tabloda gösterilmiştir:

|   | Sağlan | Sunucusuz |
| --- | --- | --- |
| Tablolar | [Evet](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Hayır, sunucusuz model yalnızca Azure [Depolama'ya](#storage-options) yerleştirilen harici verileri sorgulayabilir |
| Görünümler | [Evet, evet.](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) Görünümler, sağlanan modelde kullanılabilen [sorgu dili öğelerini](#query-language) kullanabilir. | [Evet, evet.](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) Görünümler, sunucusuz modelde kullanılabilen [sorgu dili öğelerini](#query-language) kullanabilir. |
| Şemalar | [Evet](/sql/t-sql/statements/create-schema-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | [Evet](/sql/t-sql/statements/create-schema-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| Geçici tablolar | [Evet](../sql-data-warehouse/sql-data-warehouse-tables-temporary.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) | Hayır |
| Yordamlar | [Evet](/sql/t-sql/statements/create-procedure-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Hayır |
| İşlevler | [Evet](/sql/t-sql/statements/create-function-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Hayır |
| Tetikleyiciler | Hayır | Hayır |
| Dış tablolar | [Evet, evet.](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) Desteklenen [veri biçimlerine](#data-formats)bakın. | [Evet, evet.](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) Desteklenen [veri biçimlerine](#data-formats)bakın. |
| Sorguları önbelleğe alma | Evet, birden çok form (SSD tabanlı önbelleğe alma, bellek, sonuç kümesi önbelleğe alma). Buna ek olarak, Materyalize Görünüm desteklenir | Hayır |
| Tablo değişkenleri | [Hayır,](/sql/t-sql/data-types/table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)geçici tablolar kullanın | Hayır |

## <a name="query-language"></a>Sorgu dili

Synapse SQL'de kullanılan sorgu dilleri, tüketim modeline bağlı olarak farklı desteklenen özelliklere sahip olabilir. Aşağıdaki tabloda Transact-SQL lehçelerinde en önemli sorgu dili farklılıkları özetleme:

|   | Sağlan | Sunucusuz |
| --- | --- | --- |
| SELECT deyimi | Evet. [XML/FOR JSON,](/sql/t-sql/queries/select-for-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) [MATCH](/sql/t-sql/queries/match-sql-graph?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) ve [PREDICT](/sql/t-sql/queries/predict-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) için Transact-SQL sorgu yan tümceleri desteklenmez. | Evet. [XML,](/sql/t-sql/queries/select-for-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) [MATCH](/sql/t-sql/queries/match-sql-graph?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [PREDICT](/sql/t-sql/queries/predict-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve sorgu ipuçları için Transact-SQL sorgu yan tümceleri desteklenmez. [OFSET/FETCH](/sql/t-sql/queries/select-order-by-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#using-offset-and-fetch-to-limit-the-rows-returned) ve [PIVOT/UNPIVOT](/sql/t-sql/queries/from-using-pivot-and-unpivot?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) yalnızca geçici tablolardaki (harici verilerle değil) verileri sorgulamak için kullanılabilir. |
| INSERT deyimi | Evet | Hayır |
| UPDATE deyimi | Evet | Hayır |
| DELETE deyimi | Evet | Hayır |
| Birleştirme deyimi | Evet | Hayır |
| Veri yükü | Evet. Tercih edilen yardımcı program [COPY](/sql/t-sql/statements/copy-into-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) deyimidir, ancak sistem veri yüklemesi için hem TOPLU yükü (BCP) hem de [CETAS'ı](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) destekler. | Hayır |
| Veri dışarı aktarma | Evet. [CETAS'ı](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)kullanma . | Evet. [CETAS'ı](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)kullanma . |
| Türler | Evet, [imleç,](/sql/t-sql/data-types/cursor-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) [hiyerarşik,](/sql/t-sql/data-types/hierarchyid-data-type-method-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) [ntext, metin ve görüntü](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [rowversion](/sql/t-sql/data-types/rowversion-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [Uzamsal Türleri](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), sql [\_varyantı](/sql/t-sql/data-types/sql-variant-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve [xml](/sql/t-sql/xml/xml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) hariç tüm Transact-SQL türleri | Evet, [imleç,](/sql/t-sql/data-types/cursor-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) [hiyerarşik,](/sql/t-sql/data-types/hierarchyid-data-type-method-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) [ntext, metin ve görüntü](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [rowversion](/sql/t-sql/data-types/rowversion-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [Uzamsal Türleri](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), sql [\_varyantı](/sql/t-sql/data-types/sql-variant-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [xml](/sql/t-sql/xml/xml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve Tablo türü hariç tüm Transact-SQL türleri |
| Veritabanları arası sorgular | Hayır | Evet, [USE](/sql/t-sql/language-elements/use-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) deyimi de dahil. |
| Dahili işlevler (analiz) | Evet, tüm Transact-SQL [Analytic](/sql/t-sql/functions/analytic-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), Dönüşüm, [Tarih ve Saat](/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), Mantıksal, [Matematiksel](/sql/t-sql/functions/mathematical-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) fonksiyonlar, [SELECT](/sql/t-sql/functions/logical-functions-choose-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)hariç , [IIF](/sql/t-sql/functions/logical-functions-iif-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), ve [PARSE](/sql/t-sql/functions/parse-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Evet, tüm Transact-SQL [Analytic,](/sql/t-sql/functions/analytic-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)Dönüşüm, [Tarih ve Saat](/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), Mantıksal, [Matematiksel](/sql/t-sql/functions/mathematical-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) fonksiyonlar. |
| Yerleşik işlevler (metin) | Evet. tüm Transact-SQL [String](/sql/t-sql/functions/string-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [JSON](/sql/t-sql/functions/json-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve Collation fonksiyonları, [STRING_ESCAPE](/sql/t-sql/functions/string-escape-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) ve [TRANSLATE](/sql/t-sql/functions/translate-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) hariç | Evet. Tüm İşleme-SQL [String](/sql/t-sql/functions/string-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [JSON](/sql/t-sql/functions/json-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve Collation işlevleri. |
| Yerleşik tablo değeri işlevleri | Evet, [Transact-SQL Rowset fonksiyonları](/sql/t-sql/functions/functions?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#rowset-functions), [OPENXML](/sql/t-sql/functions/openxml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)hariç , [OPENDATASOURCE](/sql/t-sql/functions/opendatasource-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [OPENQUERY](/sql/t-sql/functions/openquery-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), ve [OPENROWSET](/sql/t-sql/functions/openrowset-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Evet, [Transact-SQL Rowset fonksiyonları](/sql/t-sql/functions/functions?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#rowset-functions), [OPENXML](/sql/t-sql/functions/openxml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)hariç , [OPENDATASOURCE](/sql/t-sql/functions/opendatasource-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), ve [OPENQUERY](/sql/t-sql/functions/openquery-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)  |
| Toplamalar |  CHECKSUM_AGG [ve](/sql/t-sql/functions/checksum-agg-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) [GROUPING_ID](/sql/t-sql/functions/grouping-id-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) hariç transact-SQL dahili agregalar | [STRING AGG\_](/sql/t-sql/functions/string-agg-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) hariç transact-SQL dahili agregalar |
| İşleçler | Evet, [!>](/sql/t-sql/language-elements/not-greater-than-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) ve [!<](/sql/t-sql/language-elements/not-less-than-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) dışındaki tüm [Transact-SQL işleçleri](/sql/t-sql/language-elements/operators-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Evet, tüm [Transact-SQL işleçleri](/sql/t-sql/language-elements/operators-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)  |
| Akış kontrolü | Evet. [CONTINUE](/sql/t-sql/language-elements/continue-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [GOTO](/sql/t-sql/language-elements/goto-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [RETURN](/sql/t-sql/language-elements/return-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [USE](/sql/t-sql/language-elements/use-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve [WAITFOR](/sql/t-sql/language-elements/waitfor-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) hariç tüm [Transact-SQL Control-of-flow deyimi](/sql/t-sql/language-elements/control-of-flow?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Evet. Return ve SELECT sorgusu hariç tüm [RETURN](/sql/t-sql/language-elements/return-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) [Transact-SQL Akış Denetimi deyimi](/sql/t-sql/language-elements/control-of-flow?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) `WHILE (...)` |
| DDL deyimleri (CREATE, ALTER, DROP) | Evet. Desteklenen nesne türleri için geçerli olan tüm Transact-SQL DDL deyimi | Evet. Desteklenen nesne türleri için geçerli olan tüm Transact-SQL DDL deyimi |

## <a name="security"></a>Güvenlik

Synapse SQL, verilerinizi güvence altına almak ve erişimi denetlemek için yerleşik güvenlik özelliklerini kullanmanıza olanak tanır. Aşağıdaki tablo, Synapse SQL tüketim modelleri arasındaki üst düzey farkları karşılaştırmaktadır.

|   | Sağlan | Sunucusuz |
| --- | --- | --- |
| Oturum açma bilgileri | N/A (yalnızca içerdiği kullanıcılar veritabanlarında desteklenir) | Evet |
| Kullanıcılar |  N/A (yalnızca içerdiği kullanıcılar veritabanlarında desteklenir) | Evet |
| [Bağımsız kullanıcılar](/sql/relational-databases/security/contained-database-users-making-your-database-portable?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Evet. **Not:** yalnızca bir AAD kullanıcısı sınırsız yönetici olabilir | Evet |
| Azure Etkin Dizin (AAD) kimlik doğrulaması | Evet, AAD kullanıcıları | Evet, AAD girişleri ve kullanıcıları |
| Depolama AAD geçiş kimlik doğrulaması | Evet | Evet |
| Depolama SAS belirteç kimlik doğrulaması | Hayır | Evet, [Dış VERİ KAYNAĞI](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) veya örnek düzeyinde [Kimlik](/sql/t-sql/statements/create-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)Bilgileri [VERITABANı KAPSAMLI Kimlik](/sql/t-sql/statements/create-database-scoped-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) kullanarak. |
| Depolama Erişim Anahtarı kimlik doğrulaması | Evet, [Dış VERİ KAYNAĞINDA](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) [DATABASE SCOPED CredenTIAL'ı](/sql/t-sql/statements/create-database-scoped-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) kullanma | Hayır |
| Depolama Yönetilen Kimlik kimlik doğrulaması | Evet, [Yönetilen Hizmet Kimlik Bilgilerini](../../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) kullanma | Evet, `Managed Identity` kimlik bilgisi kullanarak. |
| Depolama Uygulaması kimlik doğrulaması | [Evet](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Hayır |
| İzinler - Nesne düzeyi | Evet, kullanıcılara izin verme, reddetme ve iptal etme olanağı da dahil olmak üzere | Evet, desteklenen sistem nesnelerinde kullanıcılara/oturum açmaizinlerine HIBE, İNKAR etme ve Iptal etme olanağı da dahil olmak üzere |
| İzinler - Şema düzeyinde | Evet, şemada kullanıcılara/oturum açmalara izin verme, reddetme ve iptal etme olanağı da dahil olmak üzere | Evet, şemada kullanıcılara/oturum açmalara izin verme, reddetme ve iptal etme olanağı da dahil olmak üzere |
| İzinler - [Veritabanı düzeyinde](/sql/relational-databases/security/authentication-access/database-level-roles?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Evet | Evet |
| İzinler - [Sunucu düzeyinde](/sql/relational-databases/security/authentication-access/server-level-roles) | Hayır | Evet, sysadmin ve diğer sunucu rolleri desteklenir |
| Roller/gruplar | Evet (veritabanı kapsamı) | Evet (hem sunucu hem de veritabanı kapsamı) |
| Güvenlik &amp; kimlik işlevleri | Bazı Transact-SQL güvenlik fonksiyonları `CURRENT_USER` `HAS_DBACCESS`ve `IS_MEMBER` `IS_ROLEMEMBER`işleçleri: , `USER_NAME` `EXECUTE AS`, `SESSION_USER`, `SUSER_NAME` `SUSER_SNAME`, `SYSTEM_USER`, `USER`, , , , ,`OPEN/CLOSE MASTER KEY` | Bazı Transact-SQL güvenlik fonksiyonları `CURRENT_USER` `HAS_DBACCESS`ve `HAS_PERMS_BY_NAME` `IS_MEMBER', 'IS_ROLEMEMBER`işleçleri: `SUSER_SNAME` `SYSTEM_USER`, `USER` `USER_NAME`, `EXECUTE AS`, `REVERT` `IS_SRVROLEMEMBER` `SESSION_USER`, `SUSER_NAME`, , , , , ve . Güvenlik işlevleri dış verileri sorgulamak için kullanılamaz (sonucu sorguda kullanılabilecek değişkende saklayın).  |
| DATABASE SCOPED Kimlik BILGILERI | Evet | Evet |

SQL havuzu ve SQL isteğe bağlı verileri sorgulamak için standart Transact-SQL dilini kullanın. Ayrıntılı farklar için [Transact-SQL dil referansına](/sql/t-sql/language-reference)bakın.

## <a name="tools"></a>Araçlar

Verileri sorgulamak için Synapse SQL'e bağlanmak için çeşitli araçlar kullanabilirsiniz.

|   | Sağlan | Sunucusuz |
| --- | --- | --- |
| Synapse Stüdyosu | Evet, SQL komut dosyaları | Evet, SQL komut dosyaları |
| Power BI | Evet | [Evet](tutorial-connect-power-bi-desktop.md) |
| Azure Analiz Hizmeti | Evet | Evet |
| Azure Data Studio | Evet | Evet, sürüm 1.14 veya daha yüksek. SQL komut dosyaları ve SQL Notebook'lar desteklenir. |
| SQL Server Management Studio | Evet | Evet, sürüm 18.4 veya üstü |

Uygulamaların çoğu synapse SQL'in hem sağlanan hem de sunucusuz tüketim modellerini sorgulayabilir standart Transact-SQL dilini kullanır.

## <a name="storage-options"></a>Depolama seçenekleri

Analiz edilen veriler çeşitli depolama türlerinde depolanabilir. Aşağıdaki tablo, kullanılabilir tüm depolama seçeneklerini listeler:

|   | Sağlan | Sunucusuz |
| --- | --- | --- |
| Dahili depolama | Evet | Hayır |
| Azure Veri Gölü v2 | Evet | Evet |
| Azure Blob Depolama | Evet | Evet |

## <a name="data-formats"></a>Veri biçimleri

Analiz edilen veriler çeşitli depolama biçimlerinde depolanabilir. Aşağıdaki tablo, çözümlenebilen tüm kullanılabilir veri biçimlerini listeler:

|   | Sağlan | Sunucusuz |
| --- | --- | --- |
| Sınırlandırılmış | [Evet](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | [Evet](query-single-csv-file.md) |
| CSV | Evet (çok karakterli sınırlayıcılar desteklenmiyor) | [Evet](query-single-csv-file.md) |
| Parke | [Evet](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | [Evet](query-parquet-files.md), iç [içe geçme türü](query-parquet-nested-types.md) olan dosyalar da dahil olmak üzere |
| Kovan ORC | [Evet](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Hayır |
| Kovan RC | [Evet](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Hayır |
| JSON | Evet | [Evet](query-json-files.md) |
| [Delta gölü](https://delta.io/) | Hayır | Hayır |
| [Cdm](https://docs.microsoft.com/common-data-model/) | Hayır | Hayır |

## <a name="next-steps"></a>Sonraki adımlar
SQL havuzu ve isteğe bağlı SQL için en iyi uygulamalar hakkında ek bilgiler aşağıdaki makalelerde bulunabilir:

- [SQL havuzu için En İyi Uygulamalar](best-practices-sql-pool.md)
- [İsteğe bağlı SQL için en iyi uygulamalar](best-practices-sql-on-demand.md)