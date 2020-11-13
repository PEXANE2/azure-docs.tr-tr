---
title: Azure SYNAPSE link 'te sunucusuz SQL havuzu kullanarak verileri Azure Cosmos DB sorgulama (Önizleme)
description: Bu makalede, Azure SYNAPSE link (Önizleme) içinde sunucusuz SQL havuzu kullanarak Azure Cosmos DB sorgulamayı öğreneceksiniz.
services: synapse analytics
author: jovanpop-msft
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 09/15/2020
ms.author: jovanpop
ms.reviewer: jrasnick
ms.openlocfilehash: 087ee796fbd3c0563b8019a062acab9c7ad80bb1
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94579394"
---
# <a name="query-azure-cosmos-db-data-with-serverless-sql-pool-in-azure-synapse-link-preview"></a>Azure SYNAPSE link 'te sunucusuz SQL havuzu ile Azure Cosmos DB verileri sorgulama (Önizleme)

SYNAPSE sunucusuz SQL havuzu, işlem iş yüklerinizin performansını etkilemeden neredeyse gerçek zamanlı olarak [Azure SYNAPSE bağlantısı](../../cosmos-db/synapse-link.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) ile etkinleştirilen Azure Cosmos DB kapsayıcılarınızdaki verileri analiz etmenize olanak tanır. [Analitik depodan](../../cosmos-db/analytical-store-introduction.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) veri sorgulama ve t-SQL arabirimi aracılığıyla çok çeşitli bı ve geçici sorgulama araçlarıyla tümleşik bağlantı sunan tanıdık bir T-SQL söz dizimi sunar.

Azure Cosmos DB sorgulamak için, tam [seçim](/sql/t-sql/queries/select-transact-sql?view=sql-server-ver15) yüzeyi alanı [SQL işlevlerinin ve işleçlerin](overview-features.md)çoğunluğu dahil olmak üzere [OPENROWSET](develop-openrowset.md) işlevi aracılığıyla desteklenir. Ayrıca, Azure Blob depolama alanındaki verilerle birlikte Azure Cosmos DB verileri okuyan sorgunun sonuçlarını veya [dış tablo oluştur](develop-tables-cetas.md#cetas-in-serverless-sql-pool)' u kullanarak Azure Data Lake Storage ' i seçin. Şu anda, CETAS kullanarak Azure Cosmos DB sunucusuz SQL havuzu sorgu sonuçlarını depolayamaz. 

Bu makalede, SYNAPSE bağlantısı etkinleştirilmiş Azure Cosmos DB kapsayıcılarından verileri sorgulayan sunucusuz SQL havuzu ile bir sorgu yazmayı öğreneceksiniz. Daha sonra, Azure Cosmos DB kapsayıcıları üzerinde sunucusuz SQL havuzu görünümleri oluşturma ve bunları [Bu](./tutorial-data-analyst.md) öğreticideki Power BI modellerine bağlama hakkında daha fazla bilgi edinebilirsiniz. 

> [!IMPORTANT]
> Bu öğretici [Azure Cosmos DB iyi tanımlanmış şemayla](../../cosmos-db/analytical-store-introduction.md#schema-representation)bir kapsayıcı kullanır. Sunucusuz SQL havuzunun [Azure Cosmos DB tam uygunluk şeması](#full-fidelity-schema) için sağladığı sorgu deneyimi, önizleme geri bildirimlerine göre değiştirilecek geçici bir davranış olur. `OPENROWSET` `WITH` Sorgu deneyimi değiştirilmiş ve iyi tanımlanmış şemayla hizalandığı için, tam aslına uygunluk şeması olan bir kapsayıcıdan veri okuyan yan tümce içermeyen işlevin sonuç kümesi şemasına güvenmeyin. Görüşlerinizi [Azure SYNAPSE Analytics geri bildirim forumuna](https://feedback.azure.com/forums/307516-azure-synapse-analytics) gönderin veya [SYNAPSE link ürün ekibine](mailto:cosmosdbsynapselink@microsoft.com) başvurarak geri bildirim sağlayın.

## <a name="overview"></a>Genel Bakış

Azure Cosmos DB analitik depodaki verileri sorgulamayı ve çözümlemeyi desteklemek için sunucusuz SQL havuzu aşağıdaki `OPENROWSET` sözdizimini kullanır:

```sql
OPENROWSET( 
       'CosmosDB',
       '<Azure Cosmos DB connection string>',
       <Container name>
    )  [ < with clause > ] AS alias
```

Azure Cosmos DB bağlantı dizesi, çalışacak Azure Cosmos DB hesap adı, veritabanı adı, veritabanı hesabı ana anahtarı ve isteğe bağlı bir bölge adı belirtir `OPENROWSET` . 

> [!IMPORTANT]
> `Latin1_General_100_CI_AS_SC_UTF8`Cosmos DB analitik depodaki dize DEĞERLERI UTF-8 metin olarak kodlandığından, bazı UTF-8 veritabanı harmanlaması (örneğin) kullandığınızdan emin olun.
> Dosyadaki metin kodlaması arasında uyuşmazlık var ve harmanlama beklenmeyen metin dönüştürme hatalarına neden olabilir.
> Aşağıdaki T-SQL ifadesini kullanarak geçerli veritabanının varsayılan harmanlamasını kolayca değiştirebilirsiniz: `alter database current collate Latin1_General_100_CI_AI_SC_UTF8`

Bağlantı dizesi aşağıdaki biçimdedir:
```sql
'account=<database account name>;database=<database name>;region=<region name>;key=<database account master key>'
```

Azure Cosmos DB kapsayıcı adı, sözdiziminde tırnak işareti olmadan belirtilir `OPENROWSET` . Kapsayıcı adının herhangi bir özel karakteri varsa (örneğin, bir tire '-') varsa, adın `[]` söz dizimi içinde (köşeli ayraç) içinde sarmalanması gerekir `OPENROWSET` .

> [!NOTE]
> Sunucusuz SQL havuzu Azure Cosmos DB işlem deposunun sorgulanmasını desteklemez.

## <a name="sample-data-set"></a>Örnek veri kümesi

Bu makaledeki örneklerde, [kuzi önleme ve denetim (ECDC) COVı-19 örnekleri](https://azure.microsoft.com/services/open-datasets/catalog/ecdc-covid-19-cases/) ve [Covı-19 açık araştırma VERI kümesi (kablo-19), DOI: 10.5281/zenodo. 3715505](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/)için Avrupa merkezi 'nin verileri temel alınır. 

Bu sayfalardaki verilerin lisansını ve yapısını görebilir ve [ECDC](https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases/latest/ecdc_cases.json) ve [Cord19](https://azureopendatastorage.blob.core.windows.net/covid19temp/comm_use_subset/pdf_json/000b7d1517ceebb34e1e3e817695b6de03e2fa78.json) veri kümeleri için örnek verileri indirebilirsiniz.

Sunucusuz SQL havuzu ile Cosmos DB verilerinin nasıl sorgulanalınacağını gösteren bu makaledeki adımları takip etmek için aşağıdaki kaynakları oluşturduğunuzdan emin olun:
* [SYNAPSE bağlantısı etkin](../../cosmos-db/configure-synapse-link.md) olan bir Azure Cosmos DB veritabanı hesabı
* Adlı bir Azure Cosmos DB veritabanı `covid`
* `EcdcCases` `Cord19` Yukarıdaki örnek veri kümeleri ile birlikte adlandırılan iki Azure Cosmos DB kapsayıcısı yüklendi.

## <a name="explore-azure-cosmos-db-data-with-automatic-schema-inference"></a>Otomatik Şema çıkarımı ile Azure Cosmos DB verileri keşfet

Azure Cosmos DB verileri araştırmanın en kolay yolu, otomatik Şema çıkarımı özelliğinden yararlanarak yapılır. `WITH`Deyimden yan tümcesini atlayarak `OPENROWSET` , SUNUCUSUZ SQL havuzuna Azure Cosmos DB kapsayıcısının analitik deposunun şemasını otomatik olarak algılamaya (çıkarması) bildirebilirsiniz.

```sql
SELECT TOP 10 *
FROM OPENROWSET( 
       'CosmosDB',
       'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       EcdcCases) as documents
```
Yukarıdaki örnekte, sunucusuz SQL havuzunu, `covid` `MyCosmosDbAccount` Azure Cosmos DB anahtarı (Yukarıdaki örnekte kukla) kullanarak kimliği doğrulanan Azure Cosmos DB hesapta veritabanına bağlanacak şekilde göndereceğiz. Daha sonra `EcdcCases` bölgede bulunan analitik depoya erişeceğiz `West US 2` . Belirli özelliklerin projeksiyonu olmadığından, `OPENROWSET` işlev Azure Cosmos DB öğelerinden tüm özellikleri döndürür. 

Cosmos DB kapsayıcıdaki öğelerin, ve özelliklerinin olduğu varsayımıyla `date_rep` , `cases` `geo_id` Bu sorgunun sonuçları aşağıdaki tabloda gösterilmiştir:

| date_rep | çalışmaların | geo_id |
| --- | --- | --- |
| 2020-08-13 | 254 | RS |
| 2020-08-12 | 235 | RS |
| 2020-08-11 | 163 | RS |

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

`OPENROWSET` kapsayıcıdaki verilerden hangi özellikleri okumak istediğinizi açıkça belirtmenizi ve veri türlerini belirtmenizi sağlar. Aşağıdaki yapıyla, [ECDC COVıD veri kümesindeki](https://azure.microsoft.com/services/open-datasets/catalog/ecdc-covid-19-cases/) bazı verileri Azure Cosmos DB içine aktardığınızı düşünelim:

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

Azure Cosmos DB, bunları iç içe geçmiş nesneler veya diziler olarak oluşturarak daha karmaşık veri modellerini temsil etmenize olanak tanır. Azure Cosmos DB için SYNAPSE bağlantısının oto Sync özelliği, şema gösterimini, sunucusuz SQL havuzundan zengin sorgulama yapılmasına izin veren, iç içe geçmiş veri türlerini işlemeyi içerir.

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

[SYNAPSE bağlantısında karmaşık veri türlerini](../how-to-analyze-complex-schema.md) çözümleme ve [sunucusuz SQL havuzundaki iç içe yapılar](query-parquet-nested-types.md)hakkında daha fazla bilgi edinin.

> [!IMPORTANT]
> Metniniz yerine metinlerinizi beklenmedik bir şekilde görürseniz, `MÃƒÂ©lade` `Mélade` veritabanı harmanlamalarınız [UTF8](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support#utf8) harmanlama olarak ayarlanamaz. 
> Gibi bir SQL ifadesini kullanarak [veritabanının harmanlamasını](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-database-collation#to-change-the-database-collation) bazı UTF8 harmanlamasına değiştirin `ALTER DATABASE MyLdw COLLATE LATIN1_GENERAL_100_CI_AS_SC_UTF8` .

## <a name="flattening-nested-arrays"></a>İç içe dizileri düzleştirme

Azure Cosmos DB veriler, [Cord19](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/) veri kümesinden yazarın dizisi gibi iç içe geçmiş alt dizilere sahip olabilir:

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

Bazı durumlarda, üst öğeden (meta veriler) özellikleri dizinin (yazarlar) tüm öğeleriyle "katılmanız" gerekebilir. Sunucusuz SQL havuzu, iç içe geçmiş diziye işlevi uygulayarak iç içe yapıları düzleştirmenizi sağlar `OPENJSON` :

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

Azure Cosmos DB işlem deposu şemanın belirsiz olduğu sırada analitik deponun analitik sorgu performansı için optimize edilmesi gerektiğini unutmayın. SYNAPSE link 'in oto Sync özelliği ile, Azure Cosmos DB iç içe geçmiş veri türlerini işlemeyi içeren analitik depodaki şema gösterimini yönetir. Sunucusuz SQL havuzu analitik depoyu sorguladığı için Azure Cosmos DB giriş veri türlerini SQL veri türlerine nasıl eşleneceğini anlamak önemlidir.

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

## <a name="full-fidelity-schema"></a>Tam uygunluk şeması

Azure Cosmos DB tam uygunluk şeması, bir kapsayıcıdaki her özellik için hem değerleri hem de en iyi eşleşme türlerini kaydeder.
`OPENROWSET` tam uygunluğa sahip bir kapsayıcıdaki işlev her hücrede hem tür hem de gerçek değer sağlar. Aşağıdaki sorgunun öğeleri tam uygunluk şemasına sahip bir kapsayıcıdan okuduğunu varsayalım:

```sql
SELECT *
FROM OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       EcdcCases
    ) as rows
```

Bu sorgunun sonucu, JSON metni olarak biçimlendirilen türleri ve değerleri döndürür: 

| date_rep | çalışmaların | geo_id |
| --- | --- | --- |
| {"Date": "2020-08-13"} | {"Int32": "254"} | {"String": "RS"} |
| {"Date": "2020-08-12"} | {"Int32": "235"}| {"String": "RS"} |
| {"Date": "2020-08-11"} | {"Int32": "316"} | {"String": "RS"} |
| {"Date": "2020-08-10"} | {"Int32": "281"} | {"String": "RS"} |
| {"Date": "2020-08-09"} | {"Int32": "295"} | {"String": "RS"} |
| {"String": "2020/08/08"} | {"Int32": ","} | {"String": "RS"} |
| {"Date": "2020-08-07"} | {"float64": "339.0"} | {"String": "RS"} |

Her değer için Cosmos DB kapsayıcı öğesinde tanımlanan türü görebilirsiniz. Özellik değerlerinin çoğu `date_rep` `date` değer içerir, ancak bazıları Cosmos DB dize olarak yanlış depolanır. Tam uygunluk şeması, hem doğru yazılmış değerleri hem de `date` Hatalı biçimlendirilmiş `string` değerleri döndürür.
Servis talebi sayısı, değer olarak depolanan bir bilgi, `int32` ancak ondalık sayı olarak girilen bir değer vardır. Bu değer `float64` türü. En büyük sayıyı aşan bazı değerler varsa `int32` , bunlar tür olarak depolanır `int64` . `geo_id`Bu örnekteki tüm değerler türler olarak depolanır `string` .

> [!IMPORTANT]
> `OPENROWSET` Function `WITH` WITH yan tümcesi, hem beklenen türler hem de yanlış girilmiş türler içeren değerleri gösterir. Bu funcıon, raporlama için değil veri araştırması için tasarlanmıştır. Raporları derlemek için bu işlevden döndürülen JSON değerlerini ayrıştırmayın ve raporlarınızı oluşturmak için açık [WITH yan tümcesini](#querying-items-with-full-fidelity-schema) kullanın.
> Tam uygunluk analitik deposunda corection uygulamak için Azure Cosmos DB kapsayıcısında yanlış türlere sahip değerleri temizlemeniz gerekir. 

Mongo DB API türü Azure Cosmos DB hesaplarını sorgulamak için, analitik depoda tam uygunluk şeması gösterimi ve [burada](../../cosmos-db/analytical-store-introduction.md#analytical-schema)kullanılacak genişletilmiş özellik adları hakkında daha fazla bilgi edinebilirsiniz.

### <a name="querying-items-with-full-fidelity-schema"></a>Tam uygunluk şeması olan öğeleri sorgulama

Tam uygunluk şeması sorgulanırken, SQL türünü açık bir şekilde belirtmeniz ve beklenen Cosmos DB özellik türünün `WITH` yan tümcesini belirtmeniz gerekir. `OPENROWSET`Raporlarda bir not for `WITH` yan tümcesi kullanmayın çünkü sonuç kümesi biçimi geri bildirime göre Önizleme bölümünde değiştirilebilir.

Aşağıdaki örnekte, özelliği için `string` doğru tür `geo_id` ve `int32` özellik için doğru tür olduğunu varsayacağız `cases` :

```sql
SELECT geo_id, cases = SUM(cases)
FROM OPENROWSET(
      'CosmosDB'
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       EcdcCases
    ) WITH ( geo_id VARCHAR(50) '$.geo_id.string',
             cases INT '$.cases.int32'
    ) as rows
GROUP BY geo_id
```

`geo_id`Ve `cases` diğer türlerine sahip olan değerler değer olarak döndürülür `NULL` . Bu sorgu, `cases` () ifadesinde belirtilen türle yalnızca öğesine başvuracaktır `cases.int32` .

`cases.int64`Cosmos DB kapsayıcısında temizlenemeyen diğer türler (,) ile değerleriniz varsa `cases.float64` , yan tümcesine açık bir şekilde başvurulmanız `WITH` ve sonuçları birleştirmeniz gerekir. Aşağıdaki sorgu `int32` ,, `int64` ve `float64` `cases` sütununda depolanır:

```sql
SELECT geo_id, cases = SUM(cases_int) + SUM(cases_bigint) + SUM(cases_float)
FROM OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       EcdcCases
    ) WITH ( geo_id VARCHAR(50) '$.geo_id.string', 
             cases_int INT '$.cases.int32',
             cases_bigint BIGINT '$.cases.int64',
             cases_float FLOAT '$.cases.float64'
    ) as rows
GROUP BY geo_id
```

Bu örnekte, her bir `int32` `int64` `float64` ülke için servis talebi sayısını hesaplamak üzere durum, veya değerler olarak saklanır ve tüm değerler ayıklanmalıdır. 

## <a name="known-issues"></a>Bilinen sorunlar

- Sunucusuz SQL havuzunun [Azure Cosmos DB tam uygunluk şeması](#full-fidelity-schema) için sağladığı sorgu deneyimi, önizleme geri bildirimlerine göre değiştirilecek geçici bir davranıştır. Not `OPENROWSET` `WITH` WITH yan tümcesi, sorgu deneyimi müşteri geri bildirimlerine göre iyi tanımlanmış şemayla hizalanabileceğinden, genel önizleme sırasında sağladığı şemaya güvenmeyin. Geri bildirimde bulunmak için [SYNAPSE link ürün ekibine](mailto:cosmosdbsynapselink@microsoft.com) başvurun.
- `OPENROSET`Sütun HARMANLAMASıNDA UTF-8 kodlaması yoksa sunucusuz SQL havuzu derleme zamanı hatası döndürmez. `OPENROWSET`Aşağıdaki T-SQL ifadesini kullanarak geçerli veritabanında çalışan tüm işlevler için Varsayılan harmanlamayı kolayca değiştirebilirsiniz:`alter database current collate Latin1_General_100_CI_AI_SC_UTF8`

Olası hatalar ve sorun giderme eylemleri aşağıdaki tabloda listelenmiştir:

| Hata | Kök neden |
| --- | --- |
| Sözdizimi hataları:<br/> -' OPENROWSET ' yakınında yanlış sözdizimi<br/> - `...` tanınan bir toplu OPENROWSET sağlayıcı seçeneği değil.<br/> -Yakınında yanlış söz dizimi `...` | Olası kök nedenler<br/> -İlk parametre olarak ' CosmosDB ' kullanmıyor,<br/> -Üçüncü parametrede tanımlayıcı yerine dize sabit değeri kullanma<br/> -Üçüncü parametre (kapsayıcı adı) belirtilmiyor |
| CosmosDB bağlantı dizesinde bir hata oluştu | -Hesap, veritabanı, anahtar belirtilmemiş <br/> -Bağlantı dizesinde tanınmayan bir seçenek vardır.<br/> -Noktalı virgül, `;` bağlantı dizesinin sonuna yerleştirilir |
| CosmosDB yolunu çözümleme, ' yanlış hesap adı ' veya ' yanlış veritabanı adı ' hatasıyla başarısız oldu | Belirtilen hesap adı, veritabanı adı veya kapsayıcı bulunamıyor ya da analitik depolama alanı belirtilen koleksiyonda etkinleştirilmemiş|
| CosmosDB yolu çözümlenirken ' yanlış gizli değer ' hatası ile başarısız oldu veya ' gizli dizi null veya boş ' | Hesap anahtarı geçerli değil veya eksik. |
| `column name`Türündeki sütun `type name` dış veri türüyle uyumlu değil`type name` | ' In yan tümcesindeki belirtilen sütun türü `WITH` Cosmos DB Container türüyle eşleşmiyor. [Azure Cosmos DB, SQL tür eşlemeleri](#azure-cosmos-db-to-sql-type-mappings) veya tür kullanımı için bölümünde açıklandığı gibi, sütun türünü değiştirmeyi deneyin `VARCHAR` . |
| Sütun `NULL` , tüm hücrelerdeki değerleri içerir. | Yan tümce içinde yanlış sütun adı veya yol ifadesi olabilir `WITH` . WHERE yan tümcesindeki sütun adı (veya sütun türü öğesinden sonra yol ifadesi) `WITH` Cosmos DB koleksiyonundaki bazı özellik adları ile eşleşmelidir. Karşılaştırma **büyük/küçük harfe duyarlıdır**  (örneğin, `productCode` ve `ProductCode` farklı özelliklerdir). |

[Azure SYNAPSE geri bildirim sayfasında](https://feedback.azure.com/forums/307516-azure-synapse-analytics?category_id=387862)öneriler ve sorunlar rapor edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure SYNAPSE bağlantısıyla Power BI ve sunucusuz SQL havuzu kullanma](../../cosmos-db/synapse-link-power-bi.md)
- [Sunucusuz SQL havuzunda görünüm oluşturma ve kullanma](create-use-views.md) 
- [Azure Cosmos DB üzerinde sunucusuz SQL havuzu görünümleri oluşturma ve bunları DirectQuery aracılığıyla Power BI modellerine bağlama hakkında öğretici](./tutorial-data-analyst.md)
