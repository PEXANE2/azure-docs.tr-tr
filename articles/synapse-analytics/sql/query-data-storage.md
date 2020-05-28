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
ms.openlocfilehash: 8501f9d07ffa2d04915d4d1a351317cc145f9844
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84118259"
---
# <a name="overview-query-data-in-storage"></a>Genel Bakış: depolamada verileri sorgulama

Bu bölümde, Azure SYNAPSE Analytics 'te SQL isteğe bağlı (Önizleme) kaynağını denemek için kullanabileceğiniz örnek sorgular yer almaktadır.
Şu anda desteklenen dosya biçimleri şunlardır:  
- CSV
- Parquet
- JSON

## <a name="prerequisites"></a>Ön koşullar

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

İlk adımınız sorguları yürütebileceğiniz **bir veritabanı oluşturmaktır** . Sonra bu veritabanında [kurulum betiğini](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) yürüterek nesneleri başlatın. Bu kurulum betiği veri kaynaklarını, veritabanı kapsamlı kimlik bilgilerini ve bu örneklerde verileri okumak için kullanılan harici dosya biçimlerini oluşturacaktır.

> [!NOTE]
> Veritabanları, gerçek veriler için değil yalnızca meta verileri görüntülemek için kullanılır.  Kullandığınız veritabanı adını, daha sonra ihtiyacınız olacak şekilde yazın.

```sql
CREATE DATABASE mydbname;
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

### <a name="sample-query"></a>Örnek sorgu

Doğrulamanın son adımı aşağıdaki sorguyu yürütmeniz olur:

```sql
SELECT
    COUNT_BIG(*)
FROM  
    OPENROWSET(
        BULK 'parquet/taxi/year=2017/month=9/*.parquet',
        DATA_SOURCE = 'sqlondemanddemo',
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
