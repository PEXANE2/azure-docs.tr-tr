---
title: 'Öğretici: depolama hesaplarında verileri çözümlemeye başlama'
description: Bu öğreticide, depolama hesabında bulunan verileri çözümlemeyi öğreneceksiniz.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: workspace
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: fabfdce72202f79e2ac5bad08d124df7ce2de542
ms.sourcegitcommit: 1cf157f9a57850739adef72219e79d76ed89e264
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94592592"
---
# <a name="analyze-data-in-a-storage-account"></a>Depolama hesabındaki verileri analiz etme

Bu öğreticide, depolama hesabında bulunan verileri çözümlemeyi öğreneceksiniz.

## <a name="overview"></a>Genel Bakış

Şimdiye kadar, çalışma alanındaki veritabanlarında verilerin bulunduğu senaryolar kapsandık. Artık depolama hesaplarında dosyalarla nasıl çalışacağız gösterilmektedir. Bu senaryoda, çalışma alanının ve çalışma alanını oluştururken belirttiğimiz kapsayıcının birincil depolama hesabını kullanacağız.

* Depolama hesabının adı: **contosolake**
* Depolama hesabındaki kapsayıcının adı: **Kullanıcılar**

### <a name="create-csv-and-parquet-files-in-your-storage-account"></a>Depolama hesabınızda CSV ve Parquet dosyaları oluşturma

Aşağıdaki kodu bir not defterinde çalıştırın. Depolama hesabında bir CSV dosyası ve bir Parquet dosyası oluşturur.

```py
%%pyspark
df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df = df.repartition(1) # This ensure we'll get a single file during write()
df.write.mode("overwrite").csv("/NYCTaxi/PassengerCountStats_csvformat")
df.write.mode("overwrite").parquet("/NYCTaxi/PassengerCountStats_parquetformat")
```

### <a name="analyze-data-in-a-storage-account"></a>Depolama hesabındaki verileri analiz etme

1. SYNAPSE Studio 'da **veri** merkezine gidin ve **bağlı** ' yı seçin.
1. **Depolama hesapları**  >  **MyWorkspace (birincil-contosolake)** sayfasına gidin.
1. **Kullanıcıları (birincil)** seçin. **NYCTaxi** klasörünü görmeniz gerekir. İçinde **PassengerCountStats_csvformat** ve **PassengerCountStats_parquetformat** adlı iki klasör görmeniz gerekir.
1. **PassengerCountStats_parquetformat** klasörünü açın. İçinde, benzer bir adı olan bir Parquet dosyası görürsünüz `part-00000-2638e00c-0790-496b-a523-578da9a15019-c000.snappy.parquet` .
1. **. Parquet** öğesine sağ tıklayın ve ardından **Yeni Not defteri** ' ni seçin. Şöyle bir hücre içeren bir not defteri oluşturur:

    ```py
    %%pyspark
    data_path = spark.read.load('abfss://users@contosolake.dfs.core.windows.net/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet', format='parquet')
    data_path.show(100)
    ```

1. Hücreyi çalıştırın.
1. İçindeki Parquet dosyasına sağ tıklayın ve ardından **Yeni SQL betiği**  >  **en üstteki 100 satırları** Seç ' i seçin. Şöyle bir SQL betiği oluşturur:

    ```sql
    SELECT TOP 100 *
    FROM OPENROWSET(
        BULK 'https://contosolake.dfs.core.windows.net/users/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet',
        FORMAT='PARQUET'
    ) AS [r];
    ```

    Betik penceresinde **Bağlan** alanı **sunucusuz SQL havuzu** olarak ayarlanır.

1. Betiği çalıştırın.



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [İşlem hatları ile etkinlikleri düzenleme](get-started-pipelines.md)
