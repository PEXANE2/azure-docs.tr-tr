---
title: İsteğe bağlı SQL (Önizleme) kullanma
description: Bu hızlı başlangıçta, SQL isteğe bağlı (Önizleme) kullanarak çeşitli dosya türlerini nasıl sorgulayabileceğinizi göreceksiniz.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: d49918fc67a45419e5c7ca123642c48e689a1496
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82113791"
---
# <a name="quickstart-using-sql-on-demand"></a>Hızlı başlangıç: isteğe bağlı SQL kullanma

SYNAPSE SQL isteğe bağlı (Önizleme), Azure depolama 'ya yerleştirilmiş dosyalarınızda SQL sorgularını çalıştırmanıza olanak tanıyan sunucusuz bir sorgu hizmetidir. Bu hızlı başlangıçta, isteğe bağlı SQL kullanarak çeşitli dosya türlerini sorgulamayı öğreneceksiniz.

Aşağıdaki dosya türleri desteklenir: JSON, CSV, Apache Parquet

## <a name="prerequisites"></a>Önkoşullar

Sorgu vermek için bir SQL istemcisi seçin:

- [Azure SYNAPSE Studio](quickstart-synapse-studio.md) , depolama alanındaki dosyalara GÖZATıP SQL sorgusu oluşturmayı kullanabileceğiniz bir web aracıdır.
- [Azure Data Studio](sql/get-started-azure-data-studio.md) , isteğe bağlı veritabanınızda SQL sorguları ve Not defterleri çalıştırmanızı sağlayan bir istemci aracıdır.
- [SQL Server Management Studio](sql/get-started-ssms.md) , isteğe bağlı veritabanınızda SQL sorguları çalıştırmanızı sağlayan bir istemci aracıdır.

Hızlı Başlangıç parametreleri:

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

Tanıtım amacıyla kendi veritabanınızı oluşturun. Bu, görünümlerinizi oluşturduğunuz veritabanıdır. Bu makaledeki örnek sorgularda bu veritabanını kullanın.

> [!NOTE]
> Veritabanları, gerçek veriler için değil yalnızca görünüm meta verileri için kullanılır.
>
> Daha sonra hızlı başlangıçta kullanmak üzere kullandığınız veritabanı adını yazın.

Seçtiğiniz bir ada geçiş `mydbname` yapmak için aşağıdaki sorguyu kullanın:

```sql
CREATE DATABASE mydbname
```

### <a name="create-credentials"></a>Kimlik bilgileri oluştur

SQL 'i isteğe bağlı olarak kullanarak sorguları çalıştırmak için, depolama alanındaki dosyalara erişmek üzere kullanılacak SQL isteğe bağlı olarak kimlik bilgileri oluşturun.

> [!NOTE]
> Bu bölümdeki örnekleri başarılı bir şekilde çalıştırmak için SAS belirtecini kullanmanız gerekir.
>
> SAS belirteçlerini kullanmaya başlamak için, aşağıdaki [makalede](sql/develop-storage-files-storage-access-control.md#disable-forcing-azure-ad-pass-through)açıklanan UserIdentity öğesini bırakmalısınız.
>
> İsteğe bağlı SQL, varsayılan olarak her zaman AAD geçişli geçiş kullanır.

Depolama erişim denetimini yönetme hakkında daha fazla bilgi için bu [bağlantıyı](sql/develop-storage-files-storage-access-control.md)inceleyin.

Bu bölümdeki örneklerde kullanılan kimlik bilgilerini oluşturmak için aşağıdaki kod parçacığını yürütün:

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

## <a name="querying-csv-files"></a>CSV dosyaları sorgulanıyor

Aşağıdaki görüntüde Sorgulanacak dosyanın önizlemesi verilmiştir:

![Üst bilgi içermeyen CSV dosyasının ilk 10 satırı, Windows stili yeni satır.](./sql/media/query-single-csv-file/population.png)

Aşağıdaki sorgu, üst bilgi satırı içermeyen bir CSV dosyasının nasıl okunacağını, Windows stili yeni satırla ve virgülle ayrılmış sütunlarla nasıl okunacağını gösterir:

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

Şemayı, sorgu derleme zamanında belirtebilirsiniz.
Daha fazla örnek için bkz. [CSV dosyasını sorgulama](sql/query-single-csv-file.md).

## <a name="querying-parquet-files"></a>Parquet dosyaları sorgulanıyor

Aşağıdaki örnek, Parquet dosyalarını sorgulamak için otomatik Şema çıkarımı özelliklerini gösterir. Şemayı belirtmeden, Eylül 2017 ' deki satır sayısını döndürür.

> [!NOTE]
> Parquet dosyalarını okurken yan tümce içinde `OPENROWSET WITH` sütunlar belirtmeniz gerekmez. Bu durumda, SQL isteğe bağlı, Parquet dosyasındaki meta verileri kullanır ve sütunları ada göre bağlar.

```sql
SELECT COUNT_BIG(*)
FROM OPENROWSET
  (
      BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=2017/month=9/*.parquet'
    , FORMAT='PARQUET'
  ) AS nyc
```

[Parquet dosyalarını sorgulama](sql/query-parquet-files.md)hakkında daha fazla bilgi edinin].

## <a name="querying-json-files"></a>JSON dosyaları sorgulanıyor

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

### <a name="querying-json-files"></a>JSON dosyaları sorgulanıyor

Aşağıdaki sorguda *, Cryptology 'de dayalı ve Istatistiksel Yöntemler başlığı altında, seçilen makalelere bir giriş*olan bir kitapta skaler değerler (başlık, yayımcı) almak için [JSON_VALUE](/sql/t-sql/functions/json-value-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) nasıl kullanılacağı gösterilmektedir:

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
> JSON dosyasının tamamını tek satır/sütun olarak okuyoruz. bu nedenle, dosya içinde bulmayı beklemediğimiz için FIELDSONLANDıRıCı, FIELDQUOTE ve ROWSONLANDıRıCı, 0x0B olarak ayarlanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi aşağıdaki hızlı başlangıç makaleleriyle çalışmaya hazırsınız:

- [Tek CSV dosyasını sorgula](sql/query-single-csv-file.md)
- [Sorgu klasörleri ve birden çok CSV dosyası](sql/query-folders-multiple-csv-files.md)
- [Belirli dosyaları sorgula](sql/query-specific-files.md)
- [Parquet dosyalarını sorgulama](sql/query-parquet-files.md)
- [Parquet iç içe türlerini sorgulama](sql/query-parquet-nested-types.md)
- [JSON dosyalarını sorgulama](sql/query-json-files.md)
- [Görünümleri oluşturma ve kullanma](sql/create-use-views.md)
- [Dış tablolar oluşturma ve kullanma](sql/create-use-external-tables.md)
- [Sorgu sonucunu Azure depolama 'ya kalıcı yap](sql/create-external-table-as-select.md)

Tek bir CSV dosyasının nasıl sorgulanalınacağını öğrenmek için sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Tek CSV dosyasını sorgula](sql/query-single-csv-file.md)
