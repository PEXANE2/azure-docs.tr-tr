---
title: Azure Veri Gezgini'ne örnek verileri alma
description: Hava durumuyla ilgili örnek verileri Azure Veri Gezgini'ne nasıl yükleyin (yükleyin) hakkında bilgi edinin.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 08/12/2019
ms.openlocfilehash: 3ece5a9d225e48654a0a3a96c3b7b78327565841
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74975185"
---
# <a name="quickstart-ingest-sample-data-into-azure-data-explorer"></a>Hızlı başlatma: Azure Veri Gezgini'ne örnek verileri alma

Bu makalede, örnek verileri bir Azure Veri Gezgini veritabanına nasıl yükleyebilirsiniz (yükleyebilirsiniz). Veri [yutmak için çeşitli yollar](ingest-data-overview.md)vardır; Bu makalede, test amaçlı uygun temel bir yaklaşım üzerinde duruluyor.

> [!NOTE]
> [Azure Veri Gezgini Python kitaplığını kullanarak verileri yutmayı](python-ingest-data.md)tamamladıysanız, bu verilere zaten sahipsiniz.

## <a name="prerequisites"></a>Ön koşullar

[Test kümesi ve veritabanı](create-cluster-database-portal.md)

## <a name="ingest-data"></a>Veriyi çekme

**StormEvents** örnek veri kümesi, [Ulusal Çevre Bilgileri Merkezleri](https://www.ncdc.noaa.gov/stormevents/)'nden gelen hava durumu verilerini içerir.

1. Oturum [https://dataexplorer.azure.com](https://dataexplorer.azure.com)aç.

1. Uygulamanın sol üst köşesinden **Küme ekle**'yi seçin.

1. Küme **Ekle** iletişim kutusuna, küme URL'nizi `https://<ClusterName>.<Region>.kusto.windows.net/`forma girin , sonra **Ekle'yi**seçin.

1. Aşağıdaki komuta yapıştırın ve StormEvents tablosu oluşturmak için **Çalıştır'ı** seçin.

    ```Kusto
    .create table StormEvents (StartTime: datetime, EndTime: datetime, EpisodeId: int, EventId: int, State: string, EventType: string, InjuriesDirect: int, InjuriesIndirect: int, DeathsDirect: int, DeathsIndirect: int, DamageProperty: int, DamageCrops: int, Source: string, BeginLocation: string, EndLocation: string, BeginLat: real, BeginLon: real, EndLat: real, EndLon: real, EpisodeNarrative: string, EventNarrative: string, StormSummary: dynamic)
    ```
1. Aşağıdaki komuta yapıştırın ve StormEvents tablosuna veri almak için **Çalıştır'ı** seçin.

    ```Kusto
    .ingest into table StormEvents h'https://kustosamplefiles.blob.core.windows.net/samplefiles/StormEvents.csv?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D' with (ignoreFirstRecord=true)
    ```

1. Yutma tamamlandıktan sonra, aşağıdaki sorguya yapıştırın, penceredeki sorguyu seçin ve **Çalıştır'ı**seçin.

    ```Kusto
    StormEvents
    | sort by StartTime desc
    | take 10
    ```
    Sorgu, yutulan örnek verilerden aşağıdaki sonuçları döndürür.

    ![Sorgu sonuçları](media/ingest-sample-data/query-results.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini veri alma](ingest-data-overview.md) işlemi hakkında daha fazla bilgi edinmek için.
* [Quickstart: Azure Veri Gezgini'nde verileri sorgula](web-query-data.md) Web Web Web Sitesi.
* Kusto Query Language ile [sorgu yazın.](write-queries.md)
