---
title: 'Öğretici: isteğe bağlı SQL ile verileri çözümlemeye başlama'
description: Bu öğreticide, Spark veritabanlarında bulunan verileri kullanarak verileri SQL isteğe bağlı olarak çözümlemeyi öğreneceksiniz.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 4011cd93879d9203d8231f24bbf531d14e6e815a
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87101814"
---
# <a name="analyze-data-with-sql-on-demand"></a>İsteğe bağlı SQL ile verileri analiz etme

Bu öğreticide, Spark veritabanlarında bulunan verileri kullanarak verileri SQL isteğe bağlı olarak çözümlemeyi öğreneceksiniz. 

## <a name="analyze-nyc-taxi-data-in-spark-databases-using-sql-on-demand"></a>İstek üzerine SQL kullanarak Spark veritabanlarında NYC TAXI verilerini çözümleyin

Spark veritabanlarındaki tablolar otomatik olarak görünür ve isteğe bağlı olarak SQL tarafından sorgulanırlar.

1. SYNAPSE Studio 'da, **geliştirme** merkezine gidin ve yenı bir SQL betiği oluşturun.
1. **İsteğe bağlı SQL**'e **bağlanma** ayarı yapın.
1. Aşağıdaki metni betiğe yapıştırın ve betiği çalıştırın.

    ```sql
    SELECT *
    FROM nyctaxi.dbo.passengercountstats
    ```

    > [!NOTE]
    > İsteğe bağlı SQL kullanan bir sorguyu ilk kez çalıştırdığınızda, SQL istek üzerine sorguları çalıştırmak için gereken SQL kaynaklarını toplamak için yaklaşık 10 saniye sürer. Sonraki sorgular çok daha hızlı olacaktır.
  


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Spark kullanarak çözümleme](get-started-analyze-spark.md)
