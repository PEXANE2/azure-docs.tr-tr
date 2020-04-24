---
title: Genel Bakış-isteğe bağlı SQL kullanarak verileri depolamada sorgulama (Önizleme)
description: Bu bölümde, Azure SYNAPSE Analytics 'te SQL isteğe bağlı (Önizleme) kaynağını denemek için kullanabileceğiniz örnek sorgular yer almaktadır.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: e18fc765385e6d703e735a1ca15c539c32f36e93
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82116256"
---
# <a name="overview-query-data-in-storage"></a>Genel Bakış: depolamada verileri sorgulama

Bu bölümde, Azure SYNAPSE Analytics 'te SQL isteğe bağlı (Önizleme) kaynağını denemek için kullanabileceğiniz örnek sorgular yer almaktadır.
Şu anda Desteklenen dosyalar: 
- CSV
- Parquet
- JSON

## <a name="prerequisites"></a>Önkoşullar

Sorgu vermek için gereken araçlar:

- Tercih ettiğiniz SQL istemcisi:
    - Azure SYNAPSE Studio (Önizleme)
    - Azure Data Studio
    - SQL Server Management Studio

Ayrıca parametreler aşağıdaki gibidir:

| Parametre                                 | Açıklama                                                   |
| ----------------------------------------- | ------------------------------------------------------------- |
| SQL isteğe bağlı hizmet uç noktası adresi    | , Sunucu adı olarak kullanılır.                                   |
| SQL isteğe bağlı hizmet uç noktası bölgesi     | Örneklerde kullanılan depolamayı belirlemede kullanılacaktır. |
| Uç nokta erişimi için Kullanıcı adı ve parola | , Uç noktaya erişmek için kullanılacaktır.                               |
| Görünümler oluşturmak için kullanacağınız veritabanı     | Bu veritabanı, örnekler için bir başlangıç noktası olarak kullanılacaktır.       |

## <a name="first-time-setup"></a>İlk kez kurulum

Bu makalenin ilerleyen kısımlarında yer alan örnekleri kullanmadan önce iki adım vardır:

- Görünümleriniz için bir veritabanı oluşturma (görünümleri kullanmak istediğiniz durumlarda)
- Depolamadaki dosyalara erişmek için SQL isteğe bağlı olarak kullanılacak kimlik bilgilerini oluşturun

### <a name="create-database"></a>Veritabanı oluşturma

Görünümler oluşturmak için bir veritabanı gerekir. Bu veritabanını, bu belgede bazı örnek sorgular için kullanacaksınız.

> [!NOTE]
> Veritabanları, gerçek veriler için değil yalnızca meta verileri görüntülemek için kullanılır.  Kullandığınız veritabanı adını, daha sonra ihtiyacınız olacak şekilde yazın.

```sql
CREATE DATABASE mydbname;
```

### <a name="create-credentials"></a>Kimlik bilgileri oluştur

Sorguları çalıştırabilmeniz için önce kimlik bilgileri oluşturmanız gerekir. Bu kimlik bilgisi, depolama alanındaki dosyalara erişmek için SQL isteğe bağlı hizmeti tarafından kullanılacaktır.

> [!NOTE]
> Bu bölümde nasıl yapılacağını başarıyla çalıştırmak için SAS belirtecini kullanmanız gerekir.
>
> SAS belirteçlerini kullanmaya başlamak için, aşağıdaki [makalede](develop-storage-files-storage-access-control.md#disable-forcing-azure-ad-pass-through)açıklanan UserIdentity öğesini bırakmalısınız.
>
> İsteğe bağlı SQL, varsayılan olarak her zaman AAD geçişli geçiş kullanır.

Depolama erişim denetimini yönetme hakkında daha fazla bilgi için bu [bağlantıyı](develop-storage-files-storage-access-control.md)inceleyin.

CSV, JSON ve Parquet kapsayıcıları için kimlik bilgileri oluşturmak üzere aşağıdaki kodu çalıştırın:

```sql
-- create credentials for CSV container in our demo storage account
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://sqlondemandstorage.blob.core.windows.net/csv')
DROP CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net/csv];
GO

CREATE CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net/csv]
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = 'sv=2018-03-28&ss=bf&srt=sco&sp=rl&st=2019-10-14T12%3A10%3A25Z&se=2061-12-31T12%3A10%3A00Z&sig=KlSU2ullCscyTS0An0nozEpo4tO5JAgGBvw%2FJX2lguw%3D';
GO

-- create credentials for JSON container in our demo storage account
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://sqlondemandstorage.blob.core.windows.net/json')
DROP CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net/json];
GO

CREATE CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net/json]
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = 'sv=2018-03-28&ss=bf&srt=sco&sp=rl&st=2019-10-14T12%3A10%3A25Z&se=2061-12-31T12%3A10%3A00Z&sig=KlSU2ullCscyTS0An0nozEpo4tO5JAgGBvw%2FJX2lguw%3D';
GO

-- create credentials for PARQUET container in our demo storage account
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://sqlondemandstorage.blob.core.windows.net/parquet')
DROP CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net/parquet];
GO

CREATE CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net/parquet]
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = 'sv=2018-03-28&ss=bf&srt=sco&sp=rl&st=2019-10-14T12%3A10%3A25Z&se=2061-12-31T12%3A10%3A00Z&sig=KlSU2ullCscyTS0An0nozEpo4tO5JAgGBvw%2FJX2lguw%3D';
GO
```

## <a name="provided-demo-data"></a>Sunulan tanıtım verileri

Demo verileri aşağıdaki veri kümelerini içerir:

- NYC TAXI-sarı TAXI kayıtları-ortak NYC veri kümesinin parçası
  - CSV biçimi
  - Parquet biçimi
- Veri kümesini doldurma
  - CSV biçimi
- İç içe geçmiş sütunlara sahip örnek Parquet dosyaları
  - Parquet biçimi
- Books JSON
  - JSON biçimi

| Klasör yolu                                                  | Açıklama                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| CSV                                                        | CSV biçimindeki veriler için üst klasör                         |
| /csv/population/<br />/csv/population-unix/<br />/csv/population-unix-hdr/<br />/csv/population-unix-hdr-escape<br />/csv/population-unix-hdr-quoted | Farklı CSV biçimlerinde popülasyon veri dosyaları içeren klasörler. |
| /CSV/Taxi/                                                   | CSV biçiminde NYC ortak veri dosyaları içeren klasör              |
| Parquet                                                    | Parquet biçimindeki verilerin üst klasörü                     |
| /Parquet/TAXI                                                | Iquet biçimindeki ıFC ortak veri dosyaları, Hive/Hadoop bölümlendirme şeması kullanılarak yıl ve aya göre bölümlendirilir. |
| /Parquet/Nested/                                             | İç içe geçmiş sütunlara sahip örnek Parquet dosyaları                     |
| nesnesinde                                                       | JSON biçimindeki veriler için üst klasör                        |
| /JSON/Books/                                                 | Kitap verileri içeren JSON dosyaları                                   |

## <a name="validation"></a>Doğrulama

Aşağıdaki üç sorguyu yürütün ve kimlik bilgilerinin doğru şekilde oluşturulup oluşturulmadığını denetleyin.

> [!NOTE]
> Örnek sorgulardaki tüm URI 'Ler Kuzey Avrupa Azure bölgesinde bulunan bir depolama hesabı kullanır. Uygun kimlik bilgisini oluşturduğunuza emin olun. Aşağıdaki sorguyu çalıştırın ve depolama hesabının listelendiğinden emin olun.

```sql
SELECT name
FROM sys.credentials
WHERE
     name IN ( 'https://sqlondemandstorage.blob.core.windows.net/csv',
     'https://sqlondemandstorage.blob.core.windows.net/parquet',
     'https://sqlondemandstorage.blob.core.windows.net/json');
```

Uygun kimlik bilgisini bulamıyorsanız, [ilk kez kurulumunu](#first-time-setup)denetleyin.

### <a name="sample-query"></a>Örnek sorgu

Doğrulamanın son adımı aşağıdaki sorguyu yürütmeniz olur:

```sql
SELECT
    COUNT_BIG(*)
FROM  
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=2017/month=9/*.parquet',
        FORMAT='PARQUET'
    ) AS nyc;
```

Yukarıdaki sorgu şu sayıyı döndürmelidir: **8945574**.

## <a name="next-steps"></a>Sonraki adımlar

Artık şu nasıl yapılır makaleleriyle devam etmeye hazırsınız:

- [Tek CSV dosyasını sorgula](query-single-csv-file.md)

- [Sorgu klasörleri ve birden çok CSV dosyası](query-folders-multiple-csv-files.md)

- [Belirli dosyaları sorgula](query-specific-files.md)

- [Parquet dosyalarını sorgulama](query-parquet-files.md)

- [Parquet iç içe türlerini sorgulama](query-parquet-nested-types.md)

- [JSON dosyalarını sorgulama](query-json-files.md)

- [Görünümleri oluşturma ve kullanma](create-use-views.md)
