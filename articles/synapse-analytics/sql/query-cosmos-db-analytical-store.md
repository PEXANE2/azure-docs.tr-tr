---
title: Azure SYNAPSE link 'te SQL Server 'ı kullanarak Azure Cosmos DB verileri sorgulama (Önizleme)
description: Bu makalede, Azure SYNAPSE bağlantısı 'nda (Önizleme) SQL isteğe bağlı SQL kullanarak Azure Cosmos DB sorgulama hakkında bilgi edineceksiniz.
services: synapse analytics
author: jovanpop-msft
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 09/15/2020
ms.author: jovanpop
ms.reviewer: jrasnick
ms.openlocfilehash: d0f8fa313687b3bd45bd95f1c9ea864567821775
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92102366"
---
# <a name="query-azure-cosmos-db-data-using-sql-serverless-in-azure-synapse-link-preview"></a>Azure SYNAPSE link 'te SQL Server 'ı kullanarak Azure Cosmos DB verileri sorgulama (Önizleme)

SYNAPSE SQL sunucusuz (daha önce isteğe bağlı SQL), işlemsel iş yüklerinizin performansını etkilemeden [Azure SYNAPSE bağlantısı](../../cosmos-db/synapse-link.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) ile neredeyse gerçek zamanlı olarak etkinleştirilen Azure Cosmos DB kapsayıcılarınızdaki verileri analiz etmenizi sağlar. [Analitik depodan](../../cosmos-db/analytical-store-introduction.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) veri sorgulama ve t-SQL arabirimi aracılığıyla çok çeşitli bı ve geçici sorgulama araçlarıyla tümleşik bağlantı sunan tanıdık bir T-SQL söz dizimi sunar.

Azure Cosmos DB sorgulamak için, tam [seçim](/sql/t-sql/queries/select-transact-sql?view=sql-server-ver15) yüzeyi alanı [SQL işlevlerinin ve işleçlerin](overview-features.md)çoğunluğu dahil olmak üzere [OPENROWSET](develop-openrowset.md) işlevi aracılığıyla desteklenir. Ayrıca, Azure Blob depolama alanındaki verilerle birlikte Azure Cosmos DB verileri okuyan sorgunun sonuçlarını veya [dış tablo oluştur](develop-tables-cetas.md#cetas-in-sql-on-demand)' u kullanarak Azure Data Lake Storage ' i seçin. Şu anda [Cetas](develop-tables-cetas.md#cetas-in-sql-on-demand)kullanarak Azure Cosmos db SQL sunucusuz sorgu sonuçlarını depolayamaz.

Bu makalede, SYNAPSE bağlantısı etkinleştirilmiş Azure Cosmos DB kapsayıcılarından verileri sorgulayan SQL Server 'ı kullanarak bir sorgu yazmayı öğreneceksiniz. Daha sonra, Azure Cosmos DB kapsayıcıları üzerinde SQL sunucusuz görünümleri oluşturma ve [Bu](./tutorial-data-analyst.md) öğreticide bunları Power BI modellere bağlama hakkında daha fazla bilgi edinebilirsiniz. 

## <a name="overview"></a>Genel Bakış

Azure Cosmos DB analitik depodaki verileri sorgulamayı ve çözümlemeyi desteklemek için, SQL sunucusuz aşağıdaki `OPENROWSET` sözdizimini kullanır:

```sql
OPENROWSET( 
       'CosmosDB',
       '<Azure Cosmos DB connection string>',
       <Container name>
    )  [ < with clause > ] AS alias
```

Azure Cosmos DB bağlantı dizesi, çalışacak Azure Cosmos DB hesap adı, veritabanı adı, veritabanı hesabı ana anahtarı ve isteğe bağlı bir bölge adı belirtir `OPENROWSET` . 

> [!IMPORTANT]
> Sonra diğer adı kullandığınızdan emin olun `OPENROWSET` . İşlevden sonra diğer adı belirtmezseniz, SYNAPSE sunucusuz SQL uç noktasına bağlantı sorunu oluşmasına neden olan [bilinen bir sorun](#known-issues) vardır `OPENROWSET` .

Bağlantı dizesi aşağıdaki biçimdedir:
```sql
'account=<database account name>;database=<database name>;region=<region name>;key=<database account master key>'
```

Azure Cosmos DB kapsayıcı adı, sözdiziminde tırnak işareti olmadan belirtilir `OPENROWSET` . Kapsayıcı adının herhangi bir özel karakteri varsa (örneğin, bir tire '-') varsa, adın `[]` söz dizimi içinde (köşeli ayraç) içinde sarmalanması gerekir `OPENROWSET` .

> [!NOTE]
> SQL sunucusuz, Azure Cosmos DB işlem deposunun sorgulanmasını desteklemez.

## <a name="sample-data-set"></a>Örnek veri kümesi

Bu makaledeki örneklerde, [kuzi önleme ve denetim (ECDC) COVı-19 örnekleri](https://azure.microsoft.com/services/open-datasets/catalog/ecdc-covid-19-cases/) ve [Covı-19 açık araştırma VERI kümesi (kablo-19), DOI: 10.5281/zenodo. 3715505](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/)için Avrupa merkezi 'nin verileri temel alınır. 

Bu sayfalardaki verilerin lisansını ve yapısını görebilir ve [ECDC](https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases/latest/ecdc_cases.json) ve [Cord19](https://azureopendatastorage.blob.core.windows.net/covid19temp/comm_use_subset/pdf_json/000b7d1517ceebb34e1e3e817695b6de03e2fa78.json) veri kümeleri için örnek verileri indirebilirsiniz.

SQL sunucusuz ile Cosmos DB verilerinin nasıl sorgulanalınacağını gösteren bu makaledeki adımları takip etmek için aşağıdaki kaynakları oluşturduğunuzdan emin olun:
* [SYNAPSE bağlantısı etkin](../../cosmos-db/configure-synapse-link.md) olan bir Azure Cosmos DB veritabanı hesabı
* Adlı bir Azure Cosmos DB veritabanı `covid`
* `EcdcCases` `Cord19` Yukarıdaki örnek veri kümeleri ile birlikte adlandırılan iki Azure Cosmos DB kapsayıcısı yüklendi.

## <a name="explore-azure-cosmos-db-data-with-automatic-schema-inference"></a>Otomatik Şema çıkarımı ile Azure Cosmos DB verileri keşfet

Azure Cosmos DB verileri araştırmanın en kolay yolu, otomatik Şema çıkarımı özelliğinden yararlanarak yapılır. `WITH`Deyimden yan tümcesini atlayarak `OPENROWSET` , SQL server 'ı Azure Cosmos DB kapsayıcısının analitik deposunun şemasını otomatik olarak algılamaya (çıkarması) bildirebilirsiniz.

```sql
SELECT TOP 10 *
FROM OPENROWSET( 
       'CosmosDB',
       'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       EcdcCases) as documents
```
Yukarıdaki örnekte, SQL Server 'ı, `covid` `MyCosmosDbAccount` Azure Cosmos DB anahtarı (Yukarıdaki örnekte kukla) kullanılarak kimliği doğrulanmış Azure Cosmos DB hesapta veritabanına bağlanmayı umuyoruz. Daha sonra `EcdcCases` bölgede bulunan analitik depoya erişeceğiz `West US 2` . Belirli özelliklerin projeksiyonu olmadığından, `OPENROWSET` işlev Azure Cosmos DB öğelerinden tüm özellikleri döndürür.

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

Azure Cosmos DB, bunları iç içe geçmiş nesneler veya diziler olarak oluşturarak daha karmaşık veri modellerini temsil etmenize olanak tanır. Azure Cosmos DB için SYNAPSE bağlantısının oto Sync özelliği, şema gösterimini SQL sunucusuz 'ten zengin sorgulama yapılmasına izin veren iç içe geçmiş veri türlerini yönetmek de dahil olmak üzere, analitik depodaki bir dizi kullanıma sunar.

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

[SYNAPSE bağlantısında karmaşık veri türlerini](../how-to-analyze-complex-schema.md) çözümleme ve [SQL Server 'da iç içe yapılar](query-parquet-nested-types.md)hakkında daha fazla bilgi edinin.

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

Bazı durumlarda, üst öğeden (meta veriler) özellikleri dizinin (yazarlar) tüm öğeleriyle "katılmanız" gerekebilir. SQL sunucusuz, iç içe geçmiş diziye işlev uygulayarak iç içe yapıları düzleştirmenizi sağlar `OPENJSON` :

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

Azure Cosmos DB işlem deposu şemanın belirsiz olduğu sırada analitik deponun analitik sorgu performansı için optimize edilmesi gerektiğini unutmayın. SYNAPSE link 'in oto Sync özelliği ile, Azure Cosmos DB iç içe geçmiş veri türlerini işlemeyi içeren analitik depodaki şema gösterimini yönetir. SQL sunucusuz, analitik mağazayı daha az sorguladığı için, Azure Cosmos DB giriş veri türlerini SQL veri türlerine nasıl eşleneceğini anlamak önemlidir.

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

## <a name="known-issues"></a>Bilinen sorunlar

- Diğer **ad** , işlevinden sonra belirtilmelidir `OPENROWSET` (örneğin, `OPENROWSET (...) AS function_alias` ). Takma ad atlama bağlantı sorununa neden olabilir ve SYNAPSE sunucusuz SQL uç noktası geçici olarak kullanılamıyor olabilir. Bu sorun, Kasım 2020 ' de çözümlenir.
- SYNAPSE sunucusuz SQL Şu anda [Azure Cosmos DB tam uygunluk şemasını](../../cosmos-db/analytical-store-introduction.md#schema-representation)desteklemiyor. İyi tanımlanmış şemaya Cosmos DB erişmek için yalnızca SYNAPSE sunucusuz SQL kullanın.

Olası hataların listesi ve sorun giderme eylemleri aşağıdaki tabloda listelenmiştir:

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

Daha fazla bilgi için aşağıdaki makaleleri inceleyin:

- [SQL 'de isteğe bağlı olarak görünüm oluşturma ve kullanma](create-use-views.md) 
- [Azure Cosmos DB üzerinde SQL isteğe bağlı görünümler oluşturma ve bunları DirectQuery aracılığıyla Power BI modellerine bağlama hakkında öğretici](./tutorial-data-analyst.md)
