---
title: Genel Bakış - SQL isteğe bağlı (önizleme) kullanarak depolama daki verileri sorgula
description: Bu bölüm, Azure Synapse Analytics içindeki SQL isteğe bağlı (önizleme) kaynağını denemek için kullanabileceğiniz örnek sorgular içerir.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: cdad95b1a910a45629e85bcc716218b272afd9de
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424905"
---
# <a name="overview-query-data-in-storage"></a>Genel Bakış: Depolamadaki verileri sorgula

Bu bölüm, Azure Synapse Analytics içindeki SQL isteğe bağlı (önizleme) kaynağını denemek için kullanabileceğiniz örnek sorgular içerir.
Şu anda desteklenen dosyalar şunlardır: 
- CSV
- Parke
- JSON

## <a name="prerequisites"></a>Ön koşullar

Sorgu vermek için gereken araçlar:

- Seçtiğiniz SQL istemcisi:
    - Azure Synapse Studio (önizleme)
    - Azure Data Studio
    - SQL Server Management Studio

Ayrıca, parametreler aşağıdaki gibidir:

| Parametre                                 | Açıklama                                                   |
| ----------------------------------------- | ------------------------------------------------------------- |
| SQL isteğe bağlı hizmet bitiş noktası adresi    | Sunucu adı olarak kullanılacaktır.                                   |
| SQL isteğe bağlı hizmet bitiş noktası bölgesi     | Örneklerde kullanılan depolamayı belirlemek için kullanılacaktır. |
| Uç nokta erişimi için kullanıcı adı ve parola | Bitiş noktasına erişmek için kullanılır.                               |
| Görünümoluşturmak için kullanacağınız veritabanı     | Bu veritabanı örnekler için bir başlangıç noktası olarak kullanılacaktır.       |

## <a name="first-time-setup"></a>İlk kez kurulum

Bu makalede daha sonra dahil edilen örnekleri kullanmadan önce iki adım var:

- Görünümleriniz için bir veritabanı oluşturma (görünümleri kullanmak istediğinizde)
- Depolamadaki dosyalara erişmek için isteğe bağlı OLARAK SQL tarafından kullanılacak kimlik bilgileri oluşturma

### <a name="create-database"></a>Veritabanı oluşturma

Görünümoluşturmak için bir veritabanına ihtiyacınız vardır. Bu veritabanını, bu belgedeki örnek sorgulardan bazıları için kullanırsınız.

> [!NOTE]
> Veritabanları yalnızca meta verileri görüntülemek için kullanılır, gerçek veriler için değil.  Kullandığınız veritabanı adını yazın, daha sonra ihtiyacınız olacak.

```sql
CREATE DATABASE mydbname;
```

### <a name="create-credentials"></a>Kimlik bilgileri oluşturma

Sorguları çalıştıramadan önce kimlik bilgileri oluşturmanız gerekir. Bu kimlik bilgisi, depolama daki dosyalara erişmek için SQL on-demand hizmeti tarafından kullanılacaktır.

> [!NOTE]
> Bu bölümde Nasıl Kullanılır'ı başarılı bir şekilde çalıştırmak için SAS belirteci kullanmanız gerekmektedir.
>
> SAS belirteçlerini kullanmaya başlamak için aşağıdaki [makalede](develop-storage-files-storage-access-control.md#disable-forcing-azure-ad-pass-through)açıklanan Kullanıcı Kimliğini bırakmanız gerekir.
>
> Varsayılan olarak isteğe bağlı SQL her zaman AAD geçişini kullanır.

Depolama erişim denetiminin nasıl yönetilenhakkında daha fazla bilgi için bu bağlantıyı kontrol [edin.](develop-storage-files-storage-access-control.md)

> [!WARNING]
> Bitiş noktası bölgenizde bulunan bir depolama hesabı için kimlik bilgileri oluşturmanız gerekir. İsteğe bağlı SQL farklı bölgelerden depolama lara erişebilse de, aynı bölgede depolama ve bitiş noktası olması daha iyi bir performans deneyimi sağlar.

CSV, JSON ve Parke kapları için kimlik bilgileri oluşturmak için aşağıdaki kodu çalıştırın:

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

## <a name="provided-demo-data"></a>Sağlanan demo verileri

Demo veriler aşağıdaki veri kümelerini içerir:

- NYC Taksi - Sarı Taksi Trip Records - kamu NYC veri kümesinin bir parçası
  - CSV formatı
  - Parquet biçimi
- Nüfus veri seti
  - CSV formatı
- İç içe sütunlu örnek Parke dosyaları
  - Parquet biçimi
- Kitaplar JSON
  - JSON biçimi

| Klasör yolu                                                  | Açıklama                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| /csv/                                                        | CSV formatındaki veriler için üst klasör                         |
| /csv/nüfus/<br />/csv/nüfus-unix/<br />/csv/nüfus-unix-hdr/<br />/csv/nüfus-unix-hdr-kaçış<br />/csv/nüfus-unix-hdr-alıntı | Farklı CSV biçimlerinde Nüfus veri dosyaları olan klasörler. |
| /csv/taksi/                                                   | CSV formatında NYC ortak veri dosyaları ile klasör              |
| /parke/                                                    | Parke formatındaki veriler için üst klasör                     |
| /parke/taksi                                                | Parke formatında NYC kamu veri dosyaları, yıl ve ay Hive / Hadoop bölümleme düzeni kullanarak bölümlenmiş. |
| /parke/iç içe/                                             | İç içe sütunlu örnek Parke dosyaları                     |
| /json/                                                       | JSON formatındaki veriler için üst klasör                        |
| /json/kitaplar/                                                 | Kitap verileri ile JSON dosyaları                                   |

## <a name="validation"></a>Doğrulama

Aşağıdaki üç sorguyu yürütün ve kimlik bilgilerinin doğru oluşturulup oluşturulmayıp oluşturulmayaçıkolmadığını denetleyin.

> [!NOTE]
> Örnek sorgularda bulunan tüm URI'ler, Kuzey Avrupa Azure bölgesinde bulunan bir depolama hesabı kullanır. Uygun kimlik bilgisi oluşturduğunuzdan emin olun. Aşağıdaki sorguyu çalıştırın ve depolama hesabının listelendirilip listelendirilediğinden emin olun.

```sql
SELECT name
FROM sys.credentials
WHERE
     name IN ( 'https://sqlondemandstorage.blob.core.windows.net/csv',
     'https://sqlondemandstorage.blob.core.windows.net/parquet',
     'https://sqlondemandstorage.blob.core.windows.net/json');
```

Uygun kimlik bilgisini bulamıyorsanız, Ilk [kez kurulum](#first-time-setup)denetleyin.

### <a name="sample-query"></a>Örnek sorgu

Doğrulamanın son adımı aşağıdaki sorguyu yürütmektir:

```sql
SELECT
    COUNT_BIG(*)
FROM  
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=2017/month=9/*.parquet',
        FORMAT='PARQUET'
    ) AS nyc;
```

Yukarıdaki Sorgu bu numarayı döndürmelidir: **8945574**.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi aşağıdaki nasıl makaleler için devam etmeye hazırsınız:

- [Tek CSV dosyasorgulama](query-single-csv-file.md)

- [Sorgu klasörleri ve birden çok CSV dosyası](query-folders-multiple-csv-files.md)

- [Belirli dosyaları sorgula](query-specific-files.md)

- [Parke dosyalarını sorgula](query-parquet-files.md)

- [Sorgu Parke iç içe türleri](query-parquet-nested-types.md)

- [JSON dosyalarını sorgula](query-json-files.md)

- [Görünüm oluşturma ve kullanma](create-use-views.md)
