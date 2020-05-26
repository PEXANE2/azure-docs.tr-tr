---
title: İsteğe bağlı SQL (Önizleme) kullanma
description: Bu hızlı başlangıçta, SQL isteğe bağlı (Önizleme) kullanarak çeşitli dosya türlerini sorgulamak için ne kadar kolay olduğunu göreceksiniz.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: 6d107dcbdc31a0049c7685e6dd8223bda694a526
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83836813"
---
# <a name="quickstart-use-sql-on-demand"></a>Hızlı başlangıç: isteğe bağlı SQL kullanma

SYNAPSE SQL isteğe bağlı (Önizleme), Azure depolama 'ya yerleştirilmiş dosyalarda SQL sorguları çalıştırmanızı sağlayan sunucusuz bir sorgu hizmetidir. Bu hızlı başlangıçta, isteğe bağlı SQL kullanarak çeşitli dosya türlerini sorgulamayı öğreneceksiniz. Desteklenen biçimler [OPENROWSET](sql/develop-openrowset.md)'de listelenmiştir.

Bu hızlı başlangıçta sorgulama: CSV, Apache Parquet ve JSON dosyaları gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Sorgu vermek için bir SQL istemcisi seçin:

- [Azure SYNAPSE Studio](quickstart-synapse-studio.md) , depolama alanındaki dosyalara GÖZATıP SQL sorguları oluşturmayı kullanabileceğiniz bir web aracıdır.
- [Azure Data Studio](sql/get-started-azure-data-studio.md) , isteğe bağlı veritabanınızda SQL sorguları ve Not defterleri çalıştırmanızı sağlayan bir istemci aracıdır.
- [SQL Server Management Studio](sql/get-started-ssms.md) , isteğe bağlı veritabanınızda SQL sorguları çalıştırmanızı sağlayan bir istemci aracıdır.

Bu hızlı başlangıç için Parametreler:

| Parametre                                 | Açıklama                                                   |
| ----------------------------------------- | ------------------------------------------------------------- |
| SQL isteğe bağlı hizmet uç noktası adresi    | Sunucu adı olarak kullanılır                                   |
| SQL isteğe bağlı hizmet uç noktası bölgesi     | Örneklerde hangi depolamanın kullanılacağını belirlemek için kullanılır |
| Uç nokta erişimi için Kullanıcı adı ve parola | Uç noktaya erişmek için kullanılır                               |
| Görünümler oluşturmak için kullanılan veritabanı         | Örneklerde başlangıç noktası olarak kullanılan veritabanı       |

## <a name="first-time-setup"></a>İlk kez kurulum

Örnekleri kullanmadan önce:

- Görünümleriniz için veritabanı oluşturma (görünümleri kullanmak istediğiniz durumlarda)
- Depolamadaki dosyalara erişmek için SQL isteğe bağlı olarak kullanılacak kimlik bilgilerini oluşturun

### <a name="create-database"></a>Veritabanı oluşturma

Tanıtım amacıyla kendi veritabanınızı oluşturun. Bu veritabanını, bu makaledeki görünümlerinizi ve örnek sorguları oluşturmak için kullanacaksınız.

> [!NOTE]
> Veritabanları, gerçek veriler için değil yalnızca görünüm meta verileri için kullanılır.
>Daha sonra hızlı başlangıçta kullanmak üzere kullandığınız veritabanı adını yazın.

`mydbname`Seçtiğiniz bir ada geçiş yapmak için aşağıdaki sorguyu kullanın:

```sql
CREATE DATABASE mydbname
```

### <a name="create-data-source"></a>Veri kaynağı oluştur

Sorguları SQL ile isteğe bağlı olarak çalıştırmak için, SQL isteğe bağlı SQL 'in depolama alanındaki dosyalara erişmek için kullanabileceği veri kaynağı oluşturun.
Bu bölümdeki örneklerde kullanılan veri kaynağını oluşturmak için aşağıdaki kod parçacığını yürütün:

```sql
-- create credentials for containers in our demo storage account
CREATE DATABASE SCOPED CREDENTIAL sqlondemand
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = 'sv=2018-03-28&ss=bf&srt=sco&sp=rl&st=2019-10-14T12%3A10%3A25Z&se=2061-12-31T12%3A10%3A00Z&sig=KlSU2ullCscyTS0An0nozEpo4tO5JAgGBvw%2FJX2lguw%3D'
GO
CREATE EXTERNAL DATA SOURCE SqlOnDemandDemo WITH (
    LOCATION = 'https://sqlondemandstorage.blob.core.windows.net',
    CREDENTIAL = sqlondemand
);
```

## <a name="query-csv-files"></a>CSV dosyalarını sorgula

Aşağıdaki görüntüde Sorgulanacak dosyanın önizlemesi verilmiştir:

![Üst bilgi içermeyen CSV dosyasının ilk 10 satırı, Windows stili yeni satır.](./sql/media/query-single-csv-file/population.png)

Aşağıdaki sorgu, üst bilgi satırı içermeyen bir CSV dosyasının nasıl okunacağını, Windows stili yeni satırla ve virgülle ayrılmış sütunlarla nasıl okunacağını gösterir:

```sql
SELECT TOP 10 *
FROM OPENROWSET
  (
      BULK 'csv/population/*.csv',
      DATA_SOURCE = 'SqlOnDemandDemo',
      FORMAT = 'CSV', PARSER_VERSION = '2.0'
  )
WITH
  (
      country_code VARCHAR (5)
    , country_name VARCHAR (100)
    , year smallint
    , population bigint
  ) AS r
WHERE
  country_name = 'Luxembourg' AND year = 2017
```

Şemayı, sorgu derleme zamanında belirtebilirsiniz.
Daha fazla örnek için bkz. [CSV dosyasını sorgulama](sql/query-single-csv-file.md).

## <a name="query-parquet-files"></a>Parquet dosyalarını sorgulama

Aşağıdaki örnek, Parquet dosyalarını sorgulamak için otomatik Şema çıkarımı özelliklerini gösterir. Şemayı belirtmeden, Eylül 2017 ' deki satır sayısını döndürür.

> [!NOTE]
> `OPENROWSET WITH`Parquet dosyalarını okurken yan tümce içinde sütunlar belirtmeniz gerekmez. Bu durumda, SQL isteğe bağlı, verileri Parquet dosyasında kullanır ve sütunları ada göre bağlar.

```sql
SELECT COUNT_BIG(*)
FROM OPENROWSET
  (
      BULK 'parquet/taxi/year=2017/month=9/*.parquet',
      DATA_SOURCE = 'SqlOnDemandDemo',
      FORMAT='PARQUET'
  ) AS nyc
```

[Parquet dosyalarını sorgulama](sql/query-parquet-files.md)hakkında daha fazla bilgi edinin.

## <a name="query-json-files"></a>JSON dosyalarını sorgulama

### <a name="json-sample-file"></a>JSON örnek dosyası

Dosyalar *JSON* kapsayıcısına, klasör *kitaplarına*depolanır ve aşağıdaki yapıyla tek defter girişi içerir:

```json
{  
   "_id":"ahokw88",
   "type":"Book",
   "title":"The AWK Programming Language",
   "year":"1988",
   "publisher":"Addison-Wesley",
   "authors":[  
      "Alfred V. Aho",
      "Brian W. Kernighan",
      "Peter J. Weinberger"
   ],
   "source":"DBLP"
}
```

### <a name="query-json-files"></a>JSON dosyalarını sorgulama

Aşağıdaki sorgu, *Cryptology 'de dayalı ve Istatistiksel Yöntemler*başlığına sahip bir kitapta skalar değerler (başlık, yayımcı) almak için [JSON_VALUE](/sql/t-sql/functions/json-value-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) kullanmayı gösterir, seçilen makalelere giriş:

```sql
SELECT
    JSON_VALUE(jsonContent, '$.title') AS title
  , JSON_VALUE(jsonContent, '$.publisher') as publisher
  , jsonContent
FROM OPENROWSET
  (
      BULK 'json/books/*.json',
      DATA_SOURCE = 'SqlOnDemandDemo'
    , FORMAT='CSV'
    , FIELDTERMINATOR ='0x0b'
    , FIELDQUOTE = '0x0b'
    , ROWTERMINATOR = '0x0b'
  )
WITH
  ( jsonContent varchar(8000) ) AS [r]
WHERE
  JSON_VALUE(jsonContent, '$.title') = 'Probabilistic and Statistical Methods in Cryptology, An Introduction by Selected Topics'
```

> [!IMPORTANT]
> Tüm JSON dosyasını tek satır/sütun olarak okuyoruz. Bu nedenle, dosya içinde bulmayı beklemediğimiz için FIELDSONLANDıRıCı, FIELDQUOTE ve ROWSONLANDıRıCı, 0x0B olarak ayarlanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi aşağıdaki makalelerle devam etmeye hazırsınız:

- [Tek CSV dosyasını sorgula](sql/query-single-csv-file.md)
- [Sorgu klasörleri ve birden çok CSV dosyası](sql/query-folders-multiple-csv-files.md)
- [Belirli dosyaları sorgula](sql/query-specific-files.md)
- [Parquet dosyalarını sorgulama](sql/query-parquet-files.md)
- [Parquet iç içe türlerini sorgulama](sql/query-parquet-nested-types.md)
- [JSON dosyalarını sorgulama](sql/query-json-files.md)
- [Görünümleri oluşturma ve kullanma](sql/create-use-views.md)
- [Dış tablolar oluşturma ve kullanma](sql/create-use-external-tables.md)
- [Sorgu sonucunu Azure depolama 'ya kalıcı yap](sql/create-external-table-as-select.md)
- [Tek CSV dosyasını sorgula](sql/query-single-csv-file.md)
