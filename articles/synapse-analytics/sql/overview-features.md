---
title: SYNAPSE SQL 'de T-SQL özelliği farklılıkları
description: SYNAPSE SQL 'de kullanılabilen Transact-SQL özelliklerinin listesi.
services: synapse analytics
author: jovanpop-msft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: jovanpop
ms.reviewer: jrasnick
ms.openlocfilehash: e1223a9cd5bd3a092bc35c9ba03f7352c6ac7eed
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87059414"
---
# <a name="transact-sql-features-supported-in-azure-synapse-sql"></a>Azure SYNAPSE SQL 'de desteklenen Transact-SQL özellikleri

Azure SYNAPSE SQL, T-SQL dilini kullanarak verilerinizi sorgulayabilir ve analiz etmenizi sağlayan büyük bir veri analizi hizmetidir. Veri analizi için SQL Server ve Azure SQL veritabanı 'nda kullanılan standart ANSI uyumlu SQL dili lehçini kullanabilirsiniz. 

Transact-SQL Language, SYNAPSE SQL sunucusuz ve sağlanan modelde farklı nesnelere başvurabilir ve desteklenen özellikler kümesinde bazı farklılıklar olabilir. Bu sayfada, SYNAPSE SQL 'in tüketim modelleri arasında üst düzey Transact-SQL dil farklarını bulabilirsiniz.

## <a name="database-objects"></a>Veritabanı nesneleri

SYNAPSE SQL 'deki tüketim modelleri farklı veritabanı nesneleri kullanmanıza olanak sağlar. Desteklenen nesne türlerinin karşılaştırılması aşağıdaki tabloda gösterilmiştir:

|   | Oluşturulamadı | Sunucusuz |
| --- | --- | --- |
| **Tablolar** | [Evet](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Hayır, sunucusuz model yalnızca [Azure Storage](#storage-options) 'a yerleştirilmiş dış verileri sorgulayabilir |
| **Görünümler** | [Evet](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). Görünümler, sağlanan modelde kullanılabilir olan [sorgu dili öğelerini](#query-language) kullanabilir. | [Evet](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). Görünümler, sunucusuz modelde kullanılabilen [sorgu dili öğelerini](#query-language) kullanabilir. |
| **Şemalar** | [Evet](/sql/t-sql/statements/create-schema-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | [Evet](/sql/t-sql/statements/create-schema-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| **Geçici tablolar** | [Evet](../sql-data-warehouse/sql-data-warehouse-tables-temporary.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) | Hayır |
| **Yordamlar** | [Evet](/sql/t-sql/statements/create-procedure-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Hayır |
| **İşlevler** | [Evet](/sql/t-sql/statements/create-function-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Evet, yalnızca satır içi tablo değerli işlevler. |
| **Tetikleyiciler** | Hayır | Hayır |
| **Dış tablolar** | [Evet](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). Bkz. desteklenen [veri biçimleri](#data-formats). | [Evet](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). Bkz. desteklenen [veri biçimleri](#data-formats). |
| **Sorguları önbelleğe alma** | Evet, birden çok form (SSD tabanlı önbelleğe alma, bellek içi, sonuç kümesi önbelleği). Ayrıca, gerçekleştirilmiş görünüm desteklenir | Hayır |
| **Tablo değişkenleri** | [Hayır](/sql/t-sql/data-types/table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), geçici tabloları kullan | Hayır |
| **[Tablo dağıtımı](../sql-data-warehouse/sql-data-warehouse-tables-distribute.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)**               | Evet | Hayır |
| **[Tablo dizinleri](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)**                           | Evet | Hayır |
| **[Tablo bölümleri](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)**                     | Evet | Hayır |
| **[İstatistikler](develop-tables-statistics.md)**            | Yes | Yes |
| **[İş yükü yönetimi, kaynak sınıfları ve eşzamanlılık denetimi](../sql-data-warehouse/resource-classes-for-workload-management.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)** | Evet    | Hayır |

## <a name="query-language"></a>Sorgu dili

SYNAPSE SQL 'de kullanılan sorgu dilleri, tüketim modeline bağlı olarak desteklenen farklı özelliklere sahip olabilir. Aşağıdaki tabloda Transact-SQL dialarcts 'deki en önemli sorgu dili farklılıkları özetlenmektedir:

|   | Oluşturulamadı | Sunucusuz |
| --- | --- | --- |
| **SELECT deyimleri** | Evet. [XML/JSON için](/sql/t-sql/queries/select-for-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)Transact-SQL sorgu yan tümceleri ve [eşleştirme](/sql/t-sql/queries/match-sql-graph?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) desteklenmez. | Evet. XML, [Match](/sql/t-sql/queries/match-sql-graph?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [tahmin](/sql/t-sql/queries/predict-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve sorgu Ipuçları [için](/sql/t-sql/queries/select-for-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)Transact-SQL sorgu yan tümceleri desteklenmez. [Offset/Fetch](/sql/t-sql/queries/select-order-by-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#using-offset-and-fetch-to-limit-the-rows-returned) ve [PIVOT/UNPIVOT](/sql/t-sql/queries/from-using-pivot-and-unpivot?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) , sistem nesnelerini sorgulamak için kullanılabilir (dış veriler değil). |
| **INSERT deyimleri** | Evet | Hayır |
| **UPDATE ekstresi** | Evet | Hayır |
| **DELETE deyimleri** | Evet | Hayır |
| **MERGE ekstresi** | Evet | Hayır |
| **[İşlemler](develop-transactions.md)** | Evet | Hayır |
| **[Etiketler](develop-label.md)** | Evet | Hayır |
| **Veri yükleme** | Evet. Tercih edilen yardımcı program [Copy](/sql/t-sql/statements/copy-into-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) deyimidir, ancak sistem, veri yüklemesi IÇIN hem toplu yükleme (bcp) hem de [Cetas](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 'yi destekler. | Hayır |
| **Veri dışarı aktarma** | Evet. [Cetas](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)kullanma. | Evet. [Cetas](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)kullanma. |
| **Türler** | Evet, [imleç](/sql/t-sql/data-types/cursor-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [HierarchyId](/sql/t-sql/data-types/hierarchyid-data-type-method-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [ntext, Text ve Image](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [rowversion](/sql/t-sql/data-types/rowversion-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [uzamsal türler](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [SQL \_ Variant](/sql/t-sql/data-types/sql-variant-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve [XML](/sql/t-sql/xml/xml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) dışındaki tüm Transact-SQL türleri | Evet, [imleç](/sql/t-sql/data-types/cursor-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [HierarchyId](/sql/t-sql/data-types/hierarchyid-data-type-method-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [ntext, Text ve Image](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [rowversion](/sql/t-sql/data-types/rowversion-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [uzamsal türler](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [SQL \_ Variant](/sql/t-sql/data-types/sql-variant-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [XML](/sql/t-sql/xml/xml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve tablo türü dışındaki tüm Transact-SQL türleri |
| **Veritabanları arası sorgular** | Hayır | Yes [, using ifadesiyle](/sql/t-sql/language-elements/use-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) birlikte. |
| **Yerleşik işlevler (Analiz)** | Evet, tüm Transact-SQL [analitik](/sql/t-sql/functions/analytic-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), dönüştürme, [Tarih ve saat](/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), mantıksal, [matematik](/sql/t-sql/functions/mathematical-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) Işlevleri, [seçme](/sql/t-sql/functions/logical-functions-choose-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [IIf](/sql/t-sql/functions/logical-functions-iif-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve [ayrıştırma](/sql/t-sql/functions/parse-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) dışında | Evet, tüm Transact-SQL [analitik](/sql/t-sql/functions/analytic-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), dönüştürme, [Tarih ve saat](/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), mantıksal, [matematik](/sql/t-sql/functions/mathematical-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) işlevleri. |
| **Yerleşik işlevler (metin)** | Evet. Tüm Transact-SQL [String](/sql/t-sql/functions/string-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [JSON](/sql/t-sql/functions/json-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve harmanlama Işlevleri, [STRING_ESCAPE](/sql/t-sql/functions/string-escape-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) ve [çeviri](/sql/t-sql/functions/translate-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) haricinde | Evet. Tüm Transact-SQL [String](/sql/t-sql/functions/string-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [JSON](/sql/t-sql/functions/json-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve harmanlama işlevleri. |
| **Yerleşik tablo-değer işlevleri** | Evet, [Transact-SQL satır kümesi işlevleri](/sql/t-sql/functions/functions?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#rowset-functions), [openxml](/sql/t-sql/functions/openxml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [opendatasource](/sql/t-sql/functions/opendatasource-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [OPENQUERY](/sql/t-sql/functions/openquery-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve [OPENROWSET](/sql/t-sql/functions/openrowset-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) dışında | Evet, [Transact-SQL Rowset işlevleri](/sql/t-sql/functions/functions?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#rowset-functions), [openxml](/sql/t-sql/functions/openxml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [opendatasource](/sql/t-sql/functions/opendatasource-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve [OPENQUERY](/sql/t-sql/functions/openquery-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) dışında  |
| **Toplar** |  [Checksum_agg](/sql/t-sql/functions/checksum-agg-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) ve [GROUPING_ID](/sql/t-sql/functions/grouping-id-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) hariç olmak üzere Transact-SQL yerleşik toplamaları | Transact-SQL yerleşik toplamaları [dize \_ AGG](/sql/t-sql/functions/string-agg-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) hariç |
| **İşleçler** | Evet, [! >](/sql/t-sql/language-elements/not-greater-than-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) ve [! <](/sql/t-sql/language-elements/not-less-than-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) harıç tüm [Transact-SQL işleçleri](/sql/t-sql/language-elements/operators-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Evet, tüm [Transact-SQL işleçleri](/sql/t-sql/language-elements/operators-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)  |
| **Flow denetimi** | Evet. [CONTINUE](/sql/t-sql/language-elements/continue-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [goto](/sql/t-sql/language-elements/goto-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [Return](/sql/t-sql/language-elements/return-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [Use](/sql/t-sql/language-elements/use-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve [waıtm](/sql/t-sql/language-elements/waitfor-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) hariç tüm [Transact-SQL-Flow ekstresi](/sql/t-sql/language-elements/control-of-flow?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Evet. Koşul ve SELECT [sorgusu hariç tüm](/sql/t-sql/language-elements/return-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) [Transact-SQL Flow ekstresi](/sql/t-sql/language-elements/control-of-flow?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) `WHILE (...)` |
| **DDL deyimleri (oluştur, DEĞIŞTIR, bırak)** | Evet. Desteklenen nesne türleri için geçerli olan tüm Transact-SQL DDL deyimleri | Evet. Desteklenen nesne türleri için geçerli olan tüm Transact-SQL DDL deyimleri |

## <a name="security"></a>Güvenlik

SYNAPSE SQL, verilerinizi güvenli hale getirmek ve erişimi denetlemek için yerleşik güvenlik özelliklerini kullanmanıza olanak sağlar. Aşağıdaki tabloda SYNAPSE SQL tüketim modelleri arasındaki üst düzey farklılıklar karşılaştırılır.

|   | Oluşturulamadı | Sunucusuz |
| --- | --- | --- |
| **Oturum açma bilgileri** | Yok (veritabanlarında yalnızca kapsanan kullanıcılar desteklenir) | Yes |
| **Kullanıcılar** |  Yok (veritabanlarında yalnızca kapsanan kullanıcılar desteklenir) | Yes |
| **[Bağımsız kullanıcılar](/sql/relational-databases/security/contained-database-users-making-your-database-portable?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)** | Evet. **Note:** yalnızca BIR Azure AD kullanıcısı Kısıtlanmamış yönetici olabilir | Yes |
| **SQL Kullanıcı adı/parola kimlik doğrulaması**| Yes | Yes |
| **Azure Active Directory (AAD) kimlik doğrulaması**| Evet, Azure AD kullanıcıları | Evet, Azure AD oturum açmaları ve kullanıcılar |
| **Depolama Azure Active Directory (AAD) geçiş kimlik doğrulaması** | Yes | Yes |
| **Storage SAS belirteci kimlik doğrulaması** | Hayır | Evet, [dış VERI kaynağı](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) veya örnek düzeyi [kimlik BILGILERI](/sql/t-sql/statements/create-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)IÇINDE [veritabanı kapsamlı kimlik bilgileri](/sql/t-sql/statements/create-database-scoped-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) kullanılıyor. |
| **Depolama erişim anahtarı kimlik doğrulaması** | Evet, [dış VERI kaynağında](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) [VERITABANı kapsamlı kimlik bilgisi](/sql/t-sql/statements/create-database-scoped-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) kullanma | Hayır |
| **Depolama [yönetilen kimlik](../security/synapse-workspace-managed-identity.md) doğrulaması** | Evet, [yönetilen hizmet kimliği kimlik bilgisi](../../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) kullanma | Evet, `Managed Identity` kimlik bilgisi kullanılıyor. |
| **Depolama uygulama kimliği kimlik doğrulaması** | [Evet](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Hayır |
| **İzinler-nesne düzeyi** | Evet, kullanıcılar için izin verme, reddetme ve Iptal etme özelliği dahil | Evet, desteklenen Sistem nesnelerinde Kullanıcı/oturum açma izinleri verme, reddetme ve Iptal etme özelliği de dahil olmak. |
| **İzinler-şema düzeyi** | Evet, şemadaki kullanıcılara/oturum açma izinlerini verme, reddetme ve Iptal etme özelliği dahil | Evet, şemadaki kullanıcılara/oturum açma izinlerini verme, reddetme ve Iptal etme özelliği dahil |
| **İzinler- [veritabanı düzeyi](/sql/relational-databases/security/authentication-access/database-level-roles?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)** | Yes | Yes |
| **İzinler- [sunucu düzeyi](/sql/relational-databases/security/authentication-access/server-level-roles)** | Hayır | Evet, sysadmin ve diğer sunucu rolleri destekleniyor |
| **İzinler- [sütun düzeyinde güvenlik](/azure/synapse-analytics/sql-data-warehouse/column-level-security?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)** | Yes | Yes |
| **Roller/gruplar** | Evet (veritabanı kapsamlı) | Evet (sunucu ve veritabanı kapsamlı) |
| **Güvenlik &amp; kimliği işlevleri** | Bazı Transact-SQL güvenlik işlevleri ve işleçleri: `CURRENT_USER` , `HAS_DBACCESS` , `IS_MEMBER` , `IS_ROLEMEMBER` , `SESSION_USER` , `SUSER_NAME` , `SUSER_SNAME` , `SYSTEM_USER` , `USER` , `USER_NAME` , `EXECUTE AS` ,`OPEN/CLOSE MASTER KEY` | Bazı Transact-SQL güvenlik işlevleri ve işleçleri: `CURRENT_USER` , `HAS_DBACCESS` , `HAS_PERMS_BY_NAME` , `IS_MEMBER', 'IS_ROLEMEMBER` , `IS_SRVROLEMEMBER` , `SESSION_USER` , `SUSER_NAME` , `SUSER_SNAME` , `SYSTEM_USER` , `USER` , `USER_NAME` , `EXECUTE AS` ve `REVERT` . Güvenlik işlevleri, dış verileri sorgulamak için kullanılamaz (sonucu sorguda kullanılabilen değişkende depolayın).  |
| **VERITABANı KAPSAMLı KIMLIK BILGILERI** | Yes | Yes |
| **SUNUCU KAPSAMLı KIMLIK BILGILERI** | Hayır | Evet |
| **Satır düzeyi güvenlik** | [Evet](https://docs.microsoft.com/sql/relational-databases/security/row-level-security?toc=%2Fazure%2Fsynapse-analytics%2Ftoc.json&bc=%2Fazure%2Fsynapse-analytics%2Fbreadcrumb%2Ftoc.json&view=sql-server-ver15) | Hayır |
| **Saydam Veri Şifrelemesi (TDE)** | [Evet](/azure/sql-database/transparent-data-encryption-azure-sql?toc=%2Fazure%2Fsynapse-analytics%2Ftoc.json&bc=%2Fazure%2Fsynapse-analytics%2Fbreadcrumb%2Ftoc.json&tabs=azure-portal) | Hayır | 
| **Veri bulma & sınıflandırması** | [Evet](/azure/sql-database/sql-database-data-discovery-and-classification?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) | Hayır |
| **Güvenlik açığı değerlendirmesi** | [Evet](/azure/sql-database/sql-vulnerability-assessment?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) | Hayır |
| **Gelişmiş Tehdit Koruması** | [Evet](/azure/sql-database/sql-database-threat-detection-overview?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
| **Denetim** | [Evet](/azure/sql-database/sql-database-auditing?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) | Hayır |
| **[Güvenlik duvarı kuralları](../security/synapse-workspace-ip-firewall.md)**| Yes | Yes |
| **[Özel uç nokta](../security/synapse-workspace-managed-private-endpoints.md)**| Yes | Yes |

SQL havuzu ve isteğe bağlı SQL, verileri sorgulamak için standart Transact-SQL dilini kullanır. Ayrıntılı farklılıklar için [Transact-SQL dil başvurusuna](/sql/t-sql/language-reference)bakın.

## <a name="tools"></a>Araçlar

Verileri sorgulamak için SYNAPSE SQL 'e bağlanmak üzere çeşitli araçlar kullanabilirsiniz.

|   | Oluşturulamadı | Sunucusuz |
| --- | --- | --- |
| **Synapse Studio** | Evet, SQL betikleri | Evet, SQL betikleri |
| **Power BI** | Yes | [Evet](tutorial-connect-power-bi-desktop.md) |
| **Azure Analysis Service** | Yes | Yes |
| **Azure Data Studio** | Yes | Evet, sürüm 1.18.0 veya üzeri. SQL betikleri ve SQL Not defterleri desteklenir. |
| **SQL Server Management Studio** | Yes | Evet, sürüm 18,5 veya üzeri |

> [!NOTE]
> SSMS 'yi isteğe bağlı SQL (Önizleme) ve sorguya bağlanmak için kullanabilirsiniz. Sürüm 18,5 ' den itibaren kısmen desteklenir, bunu yalnızca bağlanmak ve sorgulamak için kullanabilirsiniz.

Çoğu uygulama standart Transact-SQL dilini kullanır, SYNAPSE SQL 'in hem sağlanan hem de sunucusuz tüketim modellerini sorgulayabilir.

## <a name="storage-options"></a>Depolama seçenekleri

Analiz edilen veriler, çeşitli depolama türlerinde depolanabilir. Aşağıdaki tabloda tüm kullanılabilir depolama seçenekleri listelenmektedir:

|   | Oluşturulamadı | Sunucusuz |
| --- | --- | --- |
| **İç depolama** | Evet | Hayır |
| **Azure Data Lake v2** | Yes | Yes |
| **Azure Blob Depolama** | Yes | Yes |
| **Azure CosmosDB analitik depolama** | Hayır | Evet ( [geçitli önizleme](/azure/cosmos-db/synapse-link?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#limitations)altında) [SYNAPSE bağlantısı](/azure/cosmos-db/synapse-link?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) kullanma |

## <a name="data-formats"></a>Veri biçimleri

Analiz edilen veriler, çeşitli depolama biçimlerinde depolanabilir. Aşağıdaki tabloda, çözümlenebilecek tüm kullanılabilir veri biçimleri listelenmektedir:

|   | Oluşturulamadı | Sunucusuz |
| --- | --- | --- |
| **Ted** | [Evet](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | [Evet](query-single-csv-file.md) |
| **CSV** | Evet (çoklu karakter sınırlayıcıları desteklenmez) | [Evet](query-single-csv-file.md) |
| **Parquet** | [Evet](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | [Evet](query-parquet-files.md), [iç içe geçmiş türler](query-parquet-nested-types.md) içeren dosyalar |
| **Hive ORC** | [Evet](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Hayır |
| **Hive RC** | [Evet](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Hayır |
| **JSON** | Yes | [Evet](query-json-files.md) |
| **Avro** | Hayır | Hayır |
| **[Delta-Gölü](https://delta.io/)** | Hayır | Hayır |
| **[CDM](https://docs.microsoft.com/common-data-model/)** | Hayır | Hayır |

## <a name="next-steps"></a>Sonraki adımlar
SQL havuzu için en iyi uygulamalar ve isteğe bağlı SQL hakkında ek bilgiler aşağıdaki makalelerde bulunabilir:

- [SQL havuzu için en iyi uygulamalar](best-practices-sql-pool.md)
- [İsteğe bağlı SQL için en iyi uygulamalar](best-practices-sql-on-demand.md)
