---
title: Azure 'da isteğe bağlı SQL SYNAPSE bağlantısı kullanarak verileri Azure Cosmos DB sorgulama (Önizleme)
description: Bu makalede, Azure SYNAPSE bağlantısı 'nda (Önizleme) SQL isteğe bağlı SQL kullanarak Azure Cosmos DB sorgulama hakkında bilgi edineceksiniz.
services: synapse analytics
author: jovanpop-msft
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 09/15/2020
ms.author: jovanpop
ms.reviewer: jrasnick
ms.openlocfilehash: c64a42c66a3b1c1810c17347e18979d599b36b6f
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941298"
---
# <a name="query-azure-cosmos-db-data-using-sql-on-demand-in-azure-synapse-link-preview"></a>Azure 'da isteğe bağlı SQL SYNAPSE bağlantısı kullanarak verileri Azure Cosmos DB sorgulama (Önizleme)

SQL sunucusuz (daha önce isteğe bağlı SQL), işlemsel iş yüklerinizin performansını etkilemeden neredeyse gerçek zamanlı olarak [Azure SYNAPSE bağlantısı](../../cosmos-db/synapse-link.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) ile etkinleştirilen Azure Cosmos DB kapsayıcılarınızdaki verileri analiz etmenizi sağlar. [Analitik depodan](../../cosmos-db/analytical-store-introduction.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) veri sorgulama ve t-SQL arabirimi aracılığıyla çok çeşitli bı ve geçici sorgulama araçlarıyla tümleşik bağlantı sunan tanıdık bir T-SQL söz dizimi sunar.

> [!NOTE]
> İsteğe bağlı SQL ile Azure Cosmos DB analitik depoyu sorgulama desteği şu anda geçitli önizlemededir. 

Azure Cosmos DB sorgulamak için, tam [seçim](/sql/t-sql/queries/select-transact-sql.md?view=sql-server-ver15&preserve-view=true) yüzeyi alanı [SQL işlevlerinin ve işleçlerin](overview-features.md)çoğunluğu dahil olmak üzere [OPENROWSET](develop-openrowset.md) işlevi aracılığıyla desteklenir. Ayrıca, Azure Blob depolama alanındaki verilerle birlikte Azure Cosmos DB verileri okuyan sorgunun sonuçlarını veya [dış tablo oluştur](develop-tables-cetas.md#cetas-in-sql-on-demand)' u kullanarak Azure Data Lake Storage ' i seçin. Şu anda [Cetas](develop-tables-cetas.md#cetas-in-sql-on-demand)kullanarak Azure Cosmos DB için SQL isteğe bağlı sorgu sonuçlarını depolayamaz.

Bu makalede, SYNAPSE bağlantısı etkinleştirilmiş Azure Cosmos DB kapsayıcılarından verileri sorgulayan SQL isteğe bağlı olarak kullanarak bir sorgu yazmayı öğreneceksiniz. Daha sonra Azure Cosmos DB kapsayıcılarından SQL isteğe bağlı görünümleri oluşturma ve [Bu](./tutorial-data-analyst.md) öğreticide bunları Power BI modellere bağlama hakkında daha fazla bilgi edinebilirsiniz. 

## <a name="overview"></a>Genel Bakış

Azure Cosmos DB analitik depodaki verileri sorgulamayı ve çözümlemeyi desteklemek için, isteğe bağlı SQL aşağıdaki `OPENROWSET` sözdizimini kullanır:

```sql
OPENROWSET( 
       'CosmosDB',
       '<Azure Cosmos DB connection string>',
       <Container name>
    )  [ < with clause > ]
```

Azure Cosmos DB bağlantı dizesi, çalışacak Azure Cosmos DB hesap adı, veritabanı adı, veritabanı hesabı ana anahtarı ve isteğe bağlı bir bölge adı belirtir `OPENROWSET` . Bağlantı dizesi aşağıdaki biçimdedir:
```sql
'account=<database account name>;database=<database name>;region=<region name>;key=<database account master key>'
```

Azure Cosmos DB kapsayıcı adı, sözdiziminde tırnak işareti olmadan belirtilir `OPENROWSET` . Kapsayıcı adının herhangi bir özel karakteri varsa (örn. bir tire '-') varsa, adın `[]` söz dizimi içinde (köşeli ayraç) içinde sarmalanması gerekir `OPENROWSET` .

> [!NOTE]
> İsteğe bağlı SQL, Azure Cosmos DB işlem deposunun sorgulanmasını desteklemez.

## <a name="sample-data-set"></a>Örnek veri kümesi

Bu makaledeki örneklerde, [kuzi önleme ve denetim (ECDC) COVı-19 örnekleri](https://azure.microsoft.com/services/open-datasets/catalog/ecdc-covid-19-cases/) ve [Covı-19 açık araştırma VERI kümesi (kablo-19), DOI: 10.5281/zenodo. 3715505](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/)için Avrupa merkezi 'nin verileri temel alınır. 

Bu sayfalardaki verilerin lisansını ve yapısını görebilir ve [ECDC](https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases/latest/ecdc_cases.json) ve [Cord19](https://azureopendatastorage.blob.core.windows.net/covid19temp/comm_use_subset/pdf_json/000b7d1517ceebb34e1e3e817695b6de03e2fa78.json) veri kümeleri için örnek verileri indirebilirsiniz.

İsteğe bağlı SQL ile Cosmos DB verilerinin nasıl sorgulanalınacağını gösteren bu makaledeki adımları takip etmek için aşağıdaki kaynakları oluşturduğunuzdan emin olun:
* [SYNAPSE bağlantısı etkin](../../cosmos-db/configure-synapse-link.md) olan bir Azure Cosmos DB veritabanı hesabı
* Adlı bir Azure Cosmos DB veritabanı `covid`
* `EcdcCases` `Cord19` Yukarıdaki örnek veri kümeleri ile birlikte adlandırılan iki Azure Cosmos DB kapsayıcısı yüklendi.

## <a name="explore-azure-cosmos-db-data-with-automatic-schema-inference"></a>Otomatik Şema çıkarımı ile Azure Cosmos DB verileri keşfet

Azure Cosmos DB verileri araştırmanın en kolay yolu, otomatik Şema çıkarımı özelliğinden yararlanarak yapılır. `WITH`Deyimden yan tümcesini atlayarak `OPENROWSET` , SQL isteğe bağlı olarak Azure Cosmos DB kapsayıcısının analitik deposunun şemasını otomatik olarak algılamaya (çıkarması) bildirebilirsiniz.

```sql
SELECT TOP 10 *
FROM OPENROWSET( 
       'CosmosDB',
       'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       EcdcCases) as documents
```
Yukarıdaki örnekte, SQL isteğe bağlı olarak, `covid` `MyCosmosDbAccount` Azure Cosmos DB anahtarı (Yukarıdaki örnekte kukla) kullanılarak kimlik doğrulaması yapılan Azure Cosmos DB hesabı 'nda veritabanına bağlanacak şekilde bağlanıyorsunuz. Daha sonra `EcdcCases` bölgede bulunan analitik depoya erişeceğiz `West US 2` . Belirli özelliklerin projeksiyonu olmadığından, `OPENROWSET` işlev Azure Cosmos DB öğelerinden tüm özellikleri döndürür.

Aynı Azure Cosmos DB veritabanındaki diğer kapsayıcıdan verileri araştırmanıza gerek varsa, aynı bağlantı dizesini kullanabilir ve gereken kapsayıcıya üçüncü parametre olarak başvurabilirsiniz:

```sql
SELECT TOP 10 *
FROM OPENROWSET( 
       'CosmosDB',
       'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Cord19) as cord19
```

## <a name="explicitly-specify-schema"></a>Açıkça şema belirt

' De otomatik Şema çıkarımı özelliği `OPENROWSET` basit ve kullanımı kolay bir querience sağlarken, iş senaryolarınız Azure Cosmos DB verilerinden yalnızca ilgili özellikleri okumak için şemayı açıkça belirtmenizi gerektirebilir.

`OPENROWSET` kapsayıcıdaki verilerden hangi özellikleri okumak istediğinizi açıkça belirtmenizi ve veri türlerini belirtmenizi sağlar. Aşağıdaki yapıyla [ECDC COVıD veri kümesinden](https://azure.microsoft.com/services/open-datasets/catalog/ecdc-covid-19-cases/) bazı verileri içeri aktardığımızda Azure Cosmos DB:

```json
{"date_rep":"2020-08-13","cases":254,"countries_and_territories":"Serbia","geo_id":"RS"}
{"date_rep":"2020-08-12","cases":235,"countries_and_territories":"Serbia","geo_id":"RS"}
{"date_rep":"2020-08-11","cases":163,"countries_and_territories":"Serbia","geo_id":"RS"}
```

Azure Cosmos DB içindeki bu düz JSON belgeleri, SYNAPSE SQL 'de bir dizi satır ve sütun olarak gösterilebilir. `OPENROWSET` işlevi, okumak istediğiniz özelliklerin bir alt kümesini ve yan tümcesindeki tam sütun türlerini belirtmenize olanak sağlar `WITH` :

```sql
SELECT TOP 10 *
FROM OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       EcdcCases
    ) with ( date_rep varchar(20), cases bigint, geo_id varchar(6) ) as rows
```

Bu sorgunun sonucu şöyle görünebilir:

| date_rep | çalışmaların | geo_id |
| --- | --- | --- |
| 2020-08-13 | 254 | RS |
| 2020-08-12 | 235 | RS |
| 2020-08-11 | 163 | RS |

Azure Cosmos DB değer için kullanılması gereken SQL türleri hakkında daha fazla bilgi için makalenin sonundaki [SQL tür eşlemeleri kurallarını](#azure-cosmos-db-to-sql-type-mappings) gözden geçirin.

## <a name="querying-nested-objects-and-arrays"></a>İç içe nesneleri ve dizileri sorgulama

Azure Cosmos DB, bunları iç içe geçmiş nesneler veya diziler olarak oluşturarak daha karmaşık veri modellerini temsil etmenize olanak tanır. Azure Cosmos DB için SYNAPSE bağlantısının otomatik eşitleme özelliği, şema gösterimini, isteğe bağlı SQL 'de zengin sorgulama yapılmasına izin veren iç içe geçmiş veri türlerini yönetmek de dahil olmak üzere, yerleşik olarak bulunan analitik mağaza 'da yönetir.

Örneğin, [kablo-19](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/) veri kümesinin aşağıdaki yapıyı takıp eden JSON belgeleri vardır:

```json
{
    "paper_id": <str>,                   # 40-character sha1 of the PDF
    "metadata": {
        "title": <str>,
        "authors": <array of objects>    # list of author dicts, in order
        ...
     }
     ...
}
```

Azure Cosmos DB iç içe geçmiş nesneler ve diziler, `OPENROWSET` işlev onları okuduğunda sorgu sonucunda json dizeleri olarak gösterilir. SQL sütunları olarak bu karmaşık türlerin değerlerini okumak için bir seçenek SQL JSON işlevlerini kullanır:

```sql
SELECT
    title = JSON_VALUE(metadata, '$.title'),
    authors = JSON_QUERY(metadata, '$.authors'),
    first_author_name = JSON_VALUE(metadata, '$.authors[0].first')
FROM
    OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Cord19
    WITH ( metadata varchar(MAX) ) AS docs;
```

Bu sorgunun sonucu şöyle görünebilir:

| başlık | düzenliyor | first_autor_name |
| --- | --- | --- |
| Tamamlayıcı bilgiler ekonomik-epidemi... |   `[{"first":"Julien","last":"Mélade","suffix":"","affiliation":{"laboratory":"Centre de Recher…` | Julien |  

Alternatif bir seçenek olarak, using yan tümcesinde nesneler içinde iç içe geçmiş değerlerin yollarını da belirtebilirsiniz `WITH` :

```sql
SELECT
    *
FROM
    OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Cord19
    WITH ( title varchar(1000) '$.metadata.title',
           authors varchar(max) '$.metadata.authors'
    ) AS docs;
```

[SYNAPSE bağlantısında karmaşık veri türlerini](../how-to-analyze-complex-schema.md) çözümleme ve [isteğe bağlı SQL 'de iç içe yapılar](query-parquet-nested-types.md)hakkında daha fazla bilgi edinin.

> [!IMPORTANT]
> Metniniz yerine metinlerinizi beklenmedik bir şekilde görürseniz, `MÃƒÂ©lade` `Mélade` veritabanı harmanlamalarınız [UTF8](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support#utf8) harmanlama olarak ayarlanamaz. 
> Gibi bir SQL ifadesini kullanarak [veritabanının harmanlamasını](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-database-collation#to-change-the-database-collation) bazı UTF8 harmanlamasına değiştirin `ALTER DATABASE MyLdw COLLATE LATIN1_GENERAL_100_CI_AS_SC_UTF8` .


## <a name="flattening-nested-arrays"></a>İç içe dizileri düzleştirme

Azure Cosmos DB veriler, [Cord19](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/) veri kümesindeki yazarlar dizisi gibi iç içe geçmiş alt dizilere sahip olabilir:

```json
{
    "paper_id": <str>,                      # 40-character sha1 of the PDF
    "metadata": {
        "title": <str>,
        "authors": [                        # list of author dicts, in order
            {
                "first": <str>,
                "middle": <list of str>,
                "last": <str>,
                "suffix": <str>,
                "affiliation": <dict>,
                "email": <str>
            },
            ...
        ],
        ...
}
```

Bazı durumlarda, üst öğeden (meta veriler) özellikleri dizinin (yazarlar) tüm öğeleriyle "katılmanız" gerekebilir. İsteğe bağlı SQL `OPENJSON` , iç içe geçmiş diziye işlev uygulayarak iç içe yapıları düzleştirmenize olanak sağlar:

```sql
SELECT
    *
FROM
    OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Cord19
    ) WITH ( title varchar(1000) '$.metadata.title',
             authors varchar(max) '$.metadata.authors' ) AS docs
      CROSS APPLY OPENJSON ( authors )
                  WITH (
                       first varchar(50),
                       last varchar(50),
                       affiliation nvarchar(max) as json
                  ) AS a
```

Bu sorgunun sonucu şöyle görünebilir:

| başlık | düzenliyor | adı | Son | ilişkisi |
| --- | --- | --- | --- | --- |
| Tamamlayıcı bilgiler ekonomik-epidemi... |   `[{"first":"Julien","last":"Mélade","suffix":"","affiliation":{"laboratory":"Centre de Recher…` | Julien | Mélade | `   {"laboratory":"Centre de Recher…` |
Tamamlayıcı bilgiler ekonomik-epidemi... | `[{"first":"Nicolas","last":"4#","suffix":"","affiliation":{"laboratory":"","institution":"U…` | Nicolas | 4 # |`{"laboratory":"","institution":"U…` | 
| Tamamlayıcı bilgiler ekonomik-epidemi... |   `[{"first":"Beza","last":"Ramazindrazana","suffix":"","affiliation":{"laboratory":"Centre de Recher…` | Beza | Ramazindrazana | `{"laboratory":"Centre de Recher…` |
| Tamamlayıcı bilgiler ekonomik-epidemi... |   `[{"first":"Olivier","last":"Flores","suffix":"","affiliation":{"laboratory":"UMR C53 CIRAD, …` | Zeytin yeşili | Flores |`{"laboratory":"UMR C53 CIRAD, …` |     

> [!IMPORTANT]
> Metniniz yerine metinlerinizi beklenmedik bir şekilde görürseniz, `MÃƒÂ©lade` `Mélade` veritabanı harmanlamalarınız [UTF8](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support#utf8) harmanlama olarak ayarlanamaz. 
> Gibi bir SQL ifadesini kullanarak [veritabanının harmanlamasını](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-database-collation#to-change-the-database-collation) bazı UTF8 harmanlamasına değiştirin `ALTER DATABASE MyLdw COLLATE LATIN1_GENERAL_100_CI_AS_SC_UTF8` .

## <a name="azure-cosmos-db-to-sql-type-mappings"></a>SQL tür eşlemelerine Azure Cosmos DB

Azure Cosmos DB işlem deposu şemanın belirsiz olduğu sırada analitik deponun analitik sorgu performansı için optimize edilmesi gerektiğini unutmayın. SYNAPSE bağlantısının otomatik eşitleme özelliği sayesinde, Azure Cosmos DB iç içe geçmiş veri türlerini işlemeyi içeren analitik depodaki şema gösterimini yönetir. İsteğe bağlı SQL, analitik depoyu sorguladığı için, Azure Cosmos DB giriş veri türlerini SQL veri türlerine nasıl eşleneceğini anlamak önemlidir.

Azure Cosmos DB SQL (Core) API 'SI hesaplarının JSON özelliği türlerini destekler, dize, Boolean, null, iç içe geçmiş nesne veya dizi. İçinde yan tümce kullanıyorsanız, bu JSON türleriyle eşleşen SQL türlerini seçmeniz gerekir `WITH` `OPENROWSET` . Azure Cosmos DB farklı özellik türleri için kullanılması gereken SQL sütun türlerinin altına bakın.

| Azure Cosmos DB Özellik türü | SQL sütun türü |
| --- | --- |
| Boole | bit |
| Tamsayı | bigint |
| Ondalık | float |
| Dize | varchar (UTF8 veritabanı harmanlaması) |
| Tarih saat (ISO biçimli dize) | varchar (30) |
| Tarih saat (Unix zaman damgası) | bigint |
| Null | `any SQL type` 
| İç içe nesne veya dizi | JSON metni olarak seri hale getirilmiş varchar (max) (UTF8 veritabanı harmanlaması) |

Mongo DB API türü Azure Cosmos DB hesaplarını sorgulamak için, analitik depoda tam uygunluk şeması gösterimi ve [burada](../../cosmos-db/analytical-store-introduction.md#analytical-schema)kullanılacak genişletilmiş özellik adları hakkında daha fazla bilgi edinebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makaleleri inceleyin:

- [SQL 'de isteğe bağlı olarak görünüm oluşturma ve kullanma](create-use-views.md) 
- [Azure Cosmos DB üzerinde SQL isteğe bağlı görünümler oluşturma ve bunları DirectQuery aracılığıyla Power BI modellerine bağlama hakkında öğretici](./tutorial-data-analyst.md)
