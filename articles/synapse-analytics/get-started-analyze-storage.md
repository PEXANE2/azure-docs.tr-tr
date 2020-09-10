---
title: 'Öğretici: depolama hesaplarında verileri çözümlemeye başlama'
description: Bu öğreticide, depolama hesabında bulunan verileri çözümlemeyi öğreneceksiniz.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: a0d5c758873413e549b31e3ec4cc41791fc8c371
ms.sourcegitcommit: 0194a29a960e3615f96a2d9d8a7e681cf3e8f9ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89667407"
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
df.write.mode("overwrite").csv("/NYCTaxi/PassengerCountStats.csv")
df.write.mode("overwrite").parquet("/NYCTaxi/PassengerCountStats.parquet")
```

### <a name="analyze-data-in-a-storage-account"></a>Depolama hesabındaki verileri analiz etme

1. SYNAPSE Studio 'da **veri** merkezine gidin ve **bağlı**' yı seçin.
1. **Depolama hesapları**  >  **MyWorkspace (birincil-contosolake)** sayfasına gidin.
1. **Kullanıcıları (birincil)** seçin. **NYCTaxi** klasörünü görmeniz gerekir. İçinde **PassengerCountStats.csv** ve **Passengercountstats. Parquet**adlı iki klasör görmeniz gerekir.
1. **Passengercountstats. Parquet** klasörünü açın. İçinde, benzer bir adı olan bir Parquet dosyası görürsünüz `part-00000-2638e00c-0790-496b-a523-578da9a15019-c000.snappy.parquet` .
1. **. Parquet**öğesine sağ tıklayın ve ardından **Yeni Not defteri**' ni seçin. Şöyle bir hücre içeren bir not defteri oluşturur:

    ```py
    %%pyspark
    data_path = spark.read.load('abfss://users@contosolake.dfs.core.windows.net/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet', format='parquet')
    data_path.show(100)
    ```

1. Hücreyi çalıştırın.
1. İçindeki Parquet dosyasına sağ tıklayın ve ardından **Yeni SQL betiği**  >  **en üstteki 100 satırları**Seç ' i seçin. Şöyle bir SQL betiği oluşturur:

    ```sql
    SELECT TOP 100 *
    FROM OPENROWSET(
        BULK 'https://contosolake.dfs.core.windows.net/users/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet',
        FORMAT='PARQUET'
    ) AS [r];
    ```

    Betik penceresinde, **Bağlan** alanı **Isteğe bağlı olarak SQL**olarak ayarlanır.

1. Betiği çalıştırın.



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [İşlem hatları ile etkinlikleri düzenleme](get-started-pipelines.md)
