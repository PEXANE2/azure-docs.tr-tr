---
title: Azure Veri Gezgini 'de örnek verileri alma
description: Hava durumu ile ilgili örnek verileri Azure Veri Gezgini 'a alma (yükleme) hakkında bilgi edinin.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 08/12/2019
ms.openlocfilehash: 3ece5a9d225e48654a0a3a96c3b7b78327565841
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74975185"
---
# <a name="quickstart-ingest-sample-data-into-azure-data-explorer"></a>Hızlı başlangıç: Azure Veri Gezgini 'de örnek verileri alma

Bu makalede, örnek verileri bir Azure Veri Gezgini veritabanına alma (yükleme) işlemi gösterilmektedir. [Verileri almanın birkaç yolu](ingest-data-overview.md)vardır; Bu makale, test amacıyla uygun olan temel bir yaklaşıma odaklanmaktadır.

> [!NOTE]
> [Azure Veri Gezgini Python kitaplığını kullanarak verileri](python-ingest-data.md)alma işlemi yaptıysanız bu verilere zaten sahipsiniz.

## <a name="prerequisites"></a>Önkoşullar

[Test kümesi ve veritabanı](create-cluster-database-portal.md)

## <a name="ingest-data"></a>Veriyi çekme

**StormEvents** örnek veri kümesi, [Ulusal Çevre Bilgileri Merkezleri](https://www.ncdc.noaa.gov/stormevents/)'nden gelen hava durumu verilerini içerir.

1. [https://dataexplorer.azure.com](https://dataexplorer.azure.com) adresinde oturum açın.

1. Uygulamanın sol üst köşesinden **Küme ekle**'yi seçin.

1. **Küme Ekle** iletişim kutusunda, `https://<ClusterName>.<Region>.kusto.windows.net/`form URL 'nizi girin ve ardından **Ekle**' yi seçin.

1. Aşağıdaki komutu yapıştırın ve **Çalıştır** ' ı seçerek StormEvents tablosu oluşturun.

    ```Kusto
    .create table StormEvents (StartTime: datetime, EndTime: datetime, EpisodeId: int, EventId: int, State: string, EventType: string, InjuriesDirect: int, InjuriesIndirect: int, DeathsDirect: int, DeathsIndirect: int, DamageProperty: int, DamageCrops: int, Source: string, BeginLocation: string, EndLocation: string, BeginLat: real, BeginLon: real, EndLat: real, EndLon: real, EpisodeNarrative: string, EventNarrative: string, StormSummary: dynamic)
    ```
1. Aşağıdaki komutu yapıştırın ve verileri StormEvents tablosuna almak için **Çalıştır** ' ı seçin.

    ```Kusto
    .ingest into table StormEvents h'https://kustosamplefiles.blob.core.windows.net/samplefiles/StormEvents.csv?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D' with (ignoreFirstRecord=true)
    ```

1. Alma işlemi tamamlandıktan sonra, aşağıdaki sorguyu yapıştırın, penceredeki sorguyu seçin ve **Çalıştır**' ı seçin.

    ```Kusto
    StormEvents
    | sort by StartTime desc
    | take 10
    ```
    Sorgu, alınan örnek verilerden aşağıdaki sonuçları döndürür.

    ![Sorgu sonuçları](media/ingest-sample-data/query-results.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini veri](ingest-data-overview.md) alımı, alım yöntemleri hakkında daha fazla bilgi edinmekdedir.
* [Hızlı başlangıç: Azure Veri Gezgini 'da veri sorgulama](web-query-data.md) Web Kullanıcı arabirimi.
* [Sorguları](write-queries.md) kusto sorgu diliyle yazın.
