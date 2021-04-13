---
title: 'Öğretici: sunucusuz bir SQL havuzu ile verileri çözümlemeye başlama'
description: Bu öğreticide, Spark veritabanlarında bulunan verileri kullanarak sunucusuz bir SQL havuzuyla verileri çözümlemeyi öğreneceksiniz.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: sql
ms.topic: tutorial
ms.date: 12/31/2020
ms.openlocfilehash: 7c228bfe5897b45e6345234f2ed8e0f5cfbec73a
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2021
ms.locfileid: "107312799"
---
# <a name="analyze-data-with-a-serverless-sql-pool"></a>Sunucusuz bir SQL havuzu ile verileri analiz etme

Bu öğreticide, sunucusuz SQL havuzu ile verileri çözümlemeyi öğreneceksiniz. 

## <a name="the-built-in-serverless-sql-pool"></a>Yerleşik sunucusuz SQL havuzu

Sunucusuz SQL havuzları, kapasiteyi ayırmak zorunda kalmadan SQL kullanmanıza olanak sağlar. Sunucusuz bir SQL havuzu için faturalandırma, sorguyu çalıştırmak için işlenen veri miktarına bağlı olarak, sorguyu çalıştırmak için kullanılan düğüm sayısını temel alır.

Her çalışma alanı, **yerleşik** olarak adlandırılan önceden yapılandırılmış sunucusuz bir SQL havuzu ile gelir. 

## <a name="analyze-nyc-taxi-data-with-a-serverless-sql-pool"></a>NYC TAXI verilerini sunucusuz bir SQL havuzuyla çözümle


1. SYNAPSE Studio 'da **geliştirme** merkezine gidin
1. Yeni bir SQL betiği oluşturun.
1. Aşağıdaki kodu betiğe yapıştırın.

    ```
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
                BULK 'https://contosolake.dfs.core.windows.net/users/NYCTripSmall.parquet',
            FORMAT='PARQUET'
        ) AS [result]
    ```
1. **Çalıştır** 'a tıklayın

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sunucusuz Spark havuzu ile verileri analiz etme](get-started-analyze-spark.md)
