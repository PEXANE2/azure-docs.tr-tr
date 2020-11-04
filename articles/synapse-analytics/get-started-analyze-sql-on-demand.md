---
title: 'Öğretici: sunucusuz SQL havuzu ile verileri çözümlemeye başlama'
description: Bu öğreticide, Spark veritabanlarında bulunan verileri kullanarak sunucusuz bir SQL havuzuyla verileri çözümlemeyi öğreneceksiniz.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: sql
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 4ca9ababbeb7843f1a014a4bd51a5e24a74acbae
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93322947"
---
# <a name="analyze-data-with-serverless-sql-pool-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te sunucusuz SQL havuzu ile verileri çözümleme

Bu öğreticide Spark veritabanlarında bulunan verileri kullanarak sunucusuz SQL havuzu ile verileri çözümlemeyi öğreneceksiniz. 

## <a name="analyze-nyc-taxi-data-in-blob-storage-using-serverless-sql-pool"></a>Sunucusuz SQL havuzu kullanarak blob depolamada NYC TAXI verilerini çözümleme

1. **Bağlı** ' ın altındaki **veri** hub 'ında Azure Blob Storage ' a sağ tıklayıp **> örnek veri kümeleri > nyc_tlc_yellow** ve **en üstteki 100 satırı seç** ' i seçin
1. Bu, aşağıdaki kodla yeni bir SQL betiği oluşturur:

    ```
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
            BULK     'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
            FORMAT = 'parquet'
        ) AS [result];
    ```
1. **Çalıştır** 'a tıklayın

## <a name="analyze-nyc-taxi-data-in-spark-databases-using-serverless-sql-pool"></a>Sunucusuz SQL havuzu kullanarak Spark veritabanlarında NYC TAXI verilerini çözümleme

Spark veritabanlarındaki tablolar otomatik olarak görünür ve sunucusuz SQL havuzu tarafından sorgulanabilir.

1. SYNAPSE Studio 'da, **geliştirme** merkezine gidin ve yenı bir SQL betiği oluşturun.
1. **Sunucusuz SQL havuzuna** **Connect** ' i ayarlayın.
1. Aşağıdaki metni betiğe yapıştırın ve betiği çalıştırın.

    ```sql
    SELECT *
    FROM nyctaxi.dbo.passengercountstats
    ```

    > [!NOTE]
    > Sunucusuz SQL havuzu kullanan bir sorguyu ilk kez çalıştırdığınızda sunucusuz SQL havuzunun sorgularınızı çalıştırmak için gereken SQL kaynaklarını toplaması yaklaşık 10 saniye sürer. Sonraki sorgular çok daha hızlı olacaktır.
  


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Depolamadaki verileri analiz etme](get-started-analyze-storage.md)
