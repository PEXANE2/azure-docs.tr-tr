---
title: Sorgularda dosya meta verilerini kullanma
description: OPENROWSET işlevi, dosya adı ve/veya klasör yoluna göre verileri filtrelemek veya çözümlemek için sorguda kullanılan her dosya hakkında dosya ve yol bilgilerini sağlar.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 05/20/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 6beda409f03938f471f089bceebaa97c6d02ae7e
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87496228"
---
# <a name="use-file-metadata-in-queries"></a>Sorgularda dosya meta verilerini kullan

SQL isteğe bağlı sorgu hizmeti, [sorgu klasörleri ve birden çok dosya](query-folders-multiple-csv-files.md) makalesinde açıklandığı gibi birden çok dosya ve klasörü ele alabilir. Bu makalede, sorgularda dosya ve klasör adları hakkında meta veri bilgilerini nasıl kullanacağınızı öğreneceksiniz.

Bazen, sonuç kümesindeki belirli bir satırla hangi dosya veya klasör kaynağının ilişkili olduğunu bilmeniz gerekebilir.

`filepath` `filename` Sonuç kümesindeki dosya adlarını ve/veya yolu döndürmek için işlevini kullanabilirsiniz. Ya da bunları, dosya adı ve/veya klasör yoluna göre filtrelemek için kullanabilirsiniz. Bu işlevler, [dosya adı işlevi](query-data-storage.md#filename-function) ve [FilePath işlevinde](query-data-storage.md#filepath-function)sözdizimi bölümünde açıklanmaktadır. Aşağıda örnek olarak kısa açıklamalar bulacaksınız.

## <a name="prerequisites"></a>Ön koşullar

İlk adımınız, depolama hesabına başvuran bir veri kaynağı ile **veritabanı oluşturmaktır** . Sonra bu veritabanında [kurulum betiğini](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) yürüterek nesneleri başlatın. Bu kurulum betiği, veri kaynaklarını, veritabanı kapsamlı kimlik bilgilerini ve bu örneklerde kullanılan harici dosya biçimlerini oluşturacaktır.

## <a name="functions"></a>İşlevler

### <a name="filename"></a>Kısaltın

Bu işlev, satırın kaynaklandığı dosya adını döndürür.

Aşağıdaki örnek, 2017 'nin son üç ayında bulunan NYC sarı TAXI veri dosyalarını okur ve dosya başına bayıldığı sayısını döndürür. Sorgunun OPENROWSET bölümü, hangi dosyaların okunacağını belirler.

```sql
SELECT
    nyc.filename() AS [filename]
    ,COUNT_BIG(*) AS [rows]
FROM  
    OPENROWSET(
        BULK 'parquet/taxi/year=2017/month=9/*.parquet',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='PARQUET'
    ) nyc
GROUP BY nyc.filename();
```

Aşağıdaki örnek *Dosya adının ()* , okunacak dosyaları FILTRELEMEK için WHERE yan tümcesinde nasıl kullanılabileceğini gösterir. Sorgunun OPENROWSET bölümünde klasörün tamamına erişir ve WHERE yan tümcesindeki dosyaları filtreler.

Sonuçlarınız, önceki örnekle aynı olacaktır.

```sql
SELECT
    r.filename() AS [filename]
    ,COUNT_BIG(*) AS [rows]
FROM OPENROWSET(
    BULK 'csv/taxi/yellow_tripdata_2017-*.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT = 'CSV',
        PARSER_VERSION = '2.0',
        FIRSTROW = 2) 
        WITH (C1 varchar(200) ) AS [r]
WHERE
    r.filename() IN ('yellow_tripdata_2017-10.csv', 'yellow_tripdata_2017-11.csv', 'yellow_tripdata_2017-12.csv')
GROUP BY
    r.filename()
ORDER BY
    [filename];
```

### <a name="filepath"></a>Null

FilePath işlevi bir tam veya kısmi yol döndürür:

- Parametresi olmadan çağrıldığında, satırın kaynaklandığı tam dosya yolunu döndürür.
- Parametresi ile çağrıldığında, parametrede belirtilen konumdaki joker karakterle eşleşen yolun bir bölümünü döndürür. Örneğin, parametre değeri 1 ilk joker karakterle eşleşen yolun bir bölümünü döndürür.

Aşağıdaki örnek, 2017 'in son üç ayı için NYC sarı TAXI veri dosyalarını okur. Dosya yolu başına bayıldığı sayısını döndürür. Sorgunun OPENROWSET bölümü, hangi dosyaların okunacağını belirler.

```sql
SELECT
    r.filepath() AS filepath
    ,COUNT_BIG(*) AS [rows]
FROM OPENROWSET(
        BULK 'csv/taxi/yellow_tripdata_2017-1*.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT = 'CSV',
        PARSER_VERSION = '2.0',
        FIRSTROW = 2
    )
    WITH (
        vendor_id INT
    ) AS [r]
GROUP BY
    r.filepath()
ORDER BY
    filepath;
```

Aşağıdaki örnek, dosya süzgecinin *()* okunacak dosyaları FILTRELEMEK için WHERE yan tümcesinde nasıl kullanılabileceğini gösterir.

Sorgunun OPENROWSET bölümünde joker karakterleri kullanabilir ve WHERE yan tümcesindeki dosyaları filtreleyebilirsiniz. Sonuçlarınız, önceki örnekle aynı olacaktır.

```sql
SELECT
    r.filepath() AS filepath
    ,r.filepath(1) AS [year]
    ,r.filepath(2) AS [month]
    ,COUNT_BIG(*) AS [rows]
FROM OPENROWSET(
        BULK 'csv/taxi/yellow_tripdata_*-*.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT = 'CSV',
        PARSER_VERSION = '2.0',        
        FIRSTROW = 2
    )
WITH (
    vendor_id INT
) AS [r]
WHERE
    r.filepath(1) IN ('2017')
    AND r.filepath(2) IN ('10', '11', '12')
GROUP BY
    r.filepath()
    ,r.filepath(1)
    ,r.filepath(2)
ORDER BY
    filepath;
```

## <a name="next-steps"></a>Sonraki adımlar

Sonraki makalede, [Parquet dosyalarını sorgulamayı](query-parquet-files.md)öğreneceksiniz.
