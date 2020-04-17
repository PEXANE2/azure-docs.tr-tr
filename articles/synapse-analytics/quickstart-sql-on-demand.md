---
title: İsteğe bağlı SQL kullanma (önizleme)
description: Bu hızlı başlatmada, isteğe bağlı SQL (önizleme) kullanarak çeşitli dosya türlerini sorgulamanın ne kadar kolay olduğunu görür ve öğrenirsiniz.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: 0d543abc88c1e45f2c1f5503473d8e92566fc582
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457391"
---
# <a name="quickstart-using-sql-on-demand"></a>Quickstart: SQL isteğe bağlı kullanma

Synapse SQL isteğe bağlı (önizleme), Azure Depolama'ya yerleştirilen dosyalarınızdaki SQL sorgularını çalıştırmanızı sağlayan sunucusuz bir sorgu hizmetidir. Bu hızlı başlatmada, isteğe bağlı SQL kullanarak çeşitli dosya türlerini nasıl sorgulayacağınızı öğreneceksiniz.

Aşağıdaki dosya türleri desteklenir: JSON, CSV, Apache Parke

## <a name="prerequisites"></a>Ön koşullar

Sorgu vermek için bir SQL istemcisi seçin:

- [Azure Synapse Studio,](quickstart-synapse-studio.md) depolama alanında dosyalara göz atmak ve SQL sorgusu oluşturmak için kullanabileceğiniz bir web aracıdır.
- [Azure Veri Stüdyosu,](sql/get-started-azure-data-studio.md) Isteğe bağlı veritabanınızda SQL sorguları ve not defterleri çalıştırmanızı sağlayan bir istemci aracıdır.
- [SQL Server Management Studio,](sql/get-started-ssms.md) Isteğe bağlı veritabanınızda SQL sorguları çalıştırmanızı sağlayan bir istemci aracıdır.

Hızlı başlatma parametreleri:

| Parametre                                 | Açıklama                                                   |
| ----------------------------------------- | ------------------------------------------------------------- |
| SQL isteğe bağlı hizmet bitiş noktası adresi    | Sunucu adı olarak kullanılır                                   |
| SQL isteğe bağlı hizmet bitiş noktası bölgesi     | Örneklerde hangi depolama alanını kullanacağımızı belirlemek için kullanılır |
| Uç nokta erişimi için kullanıcı adı ve parola | Uç noktaya erişmek için kullanılır                               |
| Görünüm oluşturmak için kullanılan veritabanı         | Örneklerde başlangıç noktası olarak kullanılan veritabanı       |

## <a name="first-time-setup"></a>İlk kez kurulum

Örnekleri kullanmadan önce:

- Görünümleriniz için veritabanı oluşturma (görünümleri kullanmak istediğinizde)
- Depolama daki dosyalara erişmek için isteğe bağlı OLARAK SQL tarafından kullanılacak kimlik bilgileri oluşturma

### <a name="create-database"></a>Veritabanı oluşturma

Demo amacıyla kendi veritabanınızı oluşturun. Bu, görünümlerinizi oluşturduğunuz veritabanıdır. Bu makaledeki örnek sorgularda bu veritabanını kullanın.

> [!NOTE]
> Veritabanları yalnızca gerçek verileri görüntülemek için değil, meta verileri görüntülemek için kullanılır.
>
> Daha sonra Quickstart'ta kullanmak üzere kullandığınız veritabanı adını yazın.

Seçtiğiniz bir ada `mydbname` göre değiştirerek aşağıdaki sorguyu kullanın:

```sql
CREATE DATABASE mydbname
```

### <a name="create-credentials"></a>Kimlik bilgileri oluşturma

İsteğe bağlı SQL kullanarak sorguları çalıştırmak için, depolamadaki dosyalara erişmek için kullanmak üzere isteğe bağlı SQL için kimlik bilgileri oluşturun.

> [!NOTE]
> Depolama hesabına erişmek için kimlik bilgisi oluşturmanız gerektiğini unutmayın. İsteğe bağlı SQL farklı bölgelerden depolama lara erişebilse de, aynı bölgede depolama ve Azure Synapse çalışma alanına sahip olmak daha iyi performans deneyimi sağlar.

CSV, JSON ve Parke kapları için kimlik bilgisi oluşturmak için aşağıdaki kod parçacıklarını değiştirin:

```sql
-- create credentials for containers in our demo storage account
IF EXISTS
   (SELECT * FROM sys.credentials
   WHERE name = 'https://sqlondemandstorage.blob.core.windows.net')
   DROP CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net]
GO

CREATE CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net]
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = 'sv=2018-03-28&ss=bf&srt=sco&sp=rl&st=2019-10-14T12%3A10%3A25Z&se=2061-12-31T12%3A10%3A00Z&sig=KlSU2ullCscyTS0An0nozEpo4tO5JAgGBvw%2FJX2lguw%3D'
GO
```

## <a name="querying-csv-files"></a>CSV dosyalarını sorgulama

Aşağıdaki resim sorgulanacak dosyanın bir önizlemesidir:

![Üstbilgi olmadan CSV dosyasının ilk 10 satır, Windows tarzı yeni satır.](./sql/media/query-single-csv-file/population.png)

Aşağıdaki sorgu, Windows stili yeni satırve virgül le sınırlı sütunlar içeren bir üstbilgi satırı içermeyen bir CSV dosyasının nasıl okunduğunu gösterir:

```sql
SELECT TOP 10 *
FROM OPENROWSET
  (
      BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population/*.csv'
    , FORMAT = 'CSV'
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

Sorgu derleme zamanında şema belirtebilirsiniz.
Daha fazla örnek için, [CSV dosyasının](sql/query-single-csv-file.md)nasıl sorgulanır olduğunu görün.

## <a name="querying-parquet-files"></a>Parke dosyalarını sorgulama

Aşağıdaki örnek, Parke dosyalarını sorgulamak için otomatik şema çıkarım özelliklerini gösterir. Şema belirtmeden 2017 Eylül ayında satır sayısını döndürür.

> [!NOTE]
> Parke dosyalarını okurken `OPENROWSET WITH` yan tümcede sütun belirtmeniz gerekmez. Bu durumda, SQL isteğe bağlı parke dosyasındaki meta verileri kullanır ve sütunları ada göre bağlar.

```sql
SELECT COUNT_BIG(*)
FROM OPENROWSET
  (
      BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=2017/month=9/*.parquet'
    , FORMAT='PARQUET'
  ) AS nyc
```

[Parke dosyalarını sorgulama](sql/query-parquet-files.md)hakkında daha fazla bilgi edinin ].

## <a name="querying-json-files"></a>JSON dosyalarını sorgulama

### <a name="json-sample-file"></a>JSON örnek dosya

Dosyalar *json* kapsayıcı, klasör *kitapları*saklanır ve aşağıdaki yapıile tek kitap girişi içerir:

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

### <a name="querying-json-files"></a>JSON dosyalarını sorgulama

Aşağıdaki sorgu, *Kriptolojide Olasılıksal ve İstatistiksel Yöntemler*başlıklı bir kitaptan skaler değerleri (başlık, yayıncı) almak için [JSON_VALUE](/sql/t-sql/functions/json-value-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) nasıl kullanılacağını gösterir, Seçili makalelere Göre Giriş:

```sql
SELECT
    JSON_VALUE(jsonContent, '$.title') AS title
  , JSON_VALUE(jsonContent, '$.publisher') as publisher
  , jsonContent
FROM OPENROWSET
  (
      BULK 'https://sqlondemandstorage.blob.core.windows.net/json/books/*.json'
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
> Biz dosyada bulmak için beklemeyin çünkü FIELDTERMINATOR, FIELDQUOTE ve ROWTERMINATOR 0x0b ayarlanır, bu yüzden tek satır / sütun olarak tüm JSON dosyası okuyoruz.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi Quickstart makaleleri aşağıdaki ile başlamak için hazırsınız:

- [Tek CSV dosyasorgulama](sql/query-single-csv-file.md)
- [Sorgu klasörleri ve birden çok CSV dosyası](sql/query-folders-multiple-csv-files.md)
- [Belirli dosyaları sorgula](sql/query-specific-files.md)
- [Parke dosyalarını sorgula](sql/query-parquet-files.md)
- [Sorgu Parke iç içe türleri](sql/query-parquet-nested-types.md)
- [JSON dosyalarını sorgula](sql/query-json-files.md)
- [Görünüm oluşturma ve kullanma](sql/create-use-views.md)
- [Dış tablolar oluşturma ve kullanma](sql/create-use-external-tables.md)
- [Sorgu sonucunu Azure depolamasına devam edin](sql/create-external-table-as-select.md)

Tek bir CSV dosyasını nasıl sorgulayın öğrenmek için bir sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Tek CSV dosyasorgulama](sql/query-single-csv-file.md)
