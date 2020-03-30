---
title: Azure Veri Gezgini'ne veri almak için akış oluşturma yıkıntma işlemini kullanma
description: Akış akışını kullanarak Azure Veri Gezgini'ne nasıl veri ödenecek (yüklenir) hakkında bilgi edinin.
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/30/2019
ms.openlocfilehash: d7d2bcf487c37fbb523b648d5aa4c572add5dfa9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79297093"
---
# <a name="streaming-ingestion-preview"></a>Akış akışı (Önizleme)

Çeşitli hacim verileri için 10 saniyeden daha kısa bir yutma süresiyle düşük gecikme süresi gerektiren akış alımını kullanın. Her tabloya veri akışının nispeten küçük olduğu (saniyede birkaç kayıt) ancak genel veri alma hacminin yüksek olduğu (saniyede binlerce kayıt) bir veya daha fazla veritabanlarında, birçok tablonun operasyonel işlemesini optimize etmek için kullanılır. 

Veri miktarı tablo başına saniyede 1 MB'dan fazla arttığında akış işlemi yerine toplu alım kullanın. Çeşitli yutma yöntemleri hakkında daha fazla bilgi edinmek için [Veri alımına genel bakışı](/azure/data-explorer/ingest-data-overview) okuyun.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* Web Web [UI'da](https://dataexplorer.azure.com/)oturum açın.
* [Azure Veri Gezgini kümesi ve veritabanı](create-cluster-database-portal.md) oluşturma

## <a name="enable-streaming-ingestion-on-your-cluster"></a>Kümenizde akış oluşturmayı etkinleştirme

> [!WARNING]
> Lütfen buharda yutulmayı etkinleştirmeden önce [sınırlamaları](#limitations) gözden geçirin.

1. Azure portalında Azure Veri Gezgini kümenize gidin. **Ayarlar'da** **Yapılandırmalar'ı**seçin. 
1. **Yapılandırmalar** bölmesinde, **Akış işlemini**etkinleştirmek için **Açık'ı** seçin.
1. **Kaydet'i**seçin.
 
    ![üzerinde akış](media/ingest-data-streaming/streaming-ingestion-on.png)
 
1. Web [UI'de,](https://dataexplorer.azure.com/)akış verileri alacak tablo(lar) veya veritabanı(lar) üzerinde [akış alma ilkesini](/azure/kusto/management/streamingingestionpolicy) tanımlayın. 

    > [!NOTE]
    > * İlke veritabanı düzeyinde tanımlanırsa, veritabanındaki tüm tablolar akış için etkinleştirilir.
    > * Uygulanan ilke, veritabanındaki diğer tablolara değil, yalnızca yeni alınan verilere başvurulabilir.

## <a name="use-streaming-ingestion-to-ingest-data-to-your-cluster"></a>Kümenize veri almak için akış oluşturma yıkıntMa işlemini kullanma

Desteklenen iki akış türü vardır:


* [**Veri**](/azure/data-explorer/ingest-data-event-hub)kaynağı olarak kullanılan Event Hub
* **Özel kabul,** Azure Veri Gezgini istemci kitaplıklarından birini kullanan bir uygulama yazmanızı gerektirir. Örnek bir uygulama için [akış alma örneğine](https://github.com/Azure/azure-kusto-samples-dotnet/tree/master/client/StreamingIngestionSample) bakın.

### <a name="choose-the-appropriate-streaming-ingestion-type"></a>Uygun akış alma türünü seçin

|   |Olay Hub'ı  |Özel Yutma  |
|---------|---------|---------|
|Başlatma başlatma işlemi ile sorgu için kullanılabilir veriler arasındaki veri gecikmesi   |    Daha uzun gecikme     |   Daha kısa gecikme      |
|Geliştirme yükü    |   Hızlı ve kolay kurulum, hiçbir geliştirme yükü    |   Hataları işlemek ve veri tutarlılığı sağlamak için uygulama için yüksek geliştirme yükü     |

## <a name="disable-streaming-ingestion-on-your-cluster"></a>Kümenizdeki akış akışını devre dışı

> [!WARNING]
> Akış akışını devre dışı bırakmak birkaç saat sürebilir.

1. İlgili tüm tablolardan ve veritabanlarından [akış alma ilkesini](/azure/kusto/management/streamingingestionpolicy) bırakın. Akış alma ilkesikaldırma, ilk depolamadan sütun deposundaki kalıcı depolamaya (kapsamlar veya kırıklar) akış veri hareketini tetikler. Veri hareketi, ilk depolamadaki veri miktarına ve CPU ve belleğin küme tarafından nasıl kullanıldığına bağlı olarak birkaç saniye ile birkaç saat arasında sürebilir.
1. Azure portalında Azure Veri Gezgini kümenize gidin. **Ayarlar'da** **Yapılandırmalar'ı**seçin. 
1. **Yapılandırmalar** bölmesinde, Akış akışını devre dışı ksayamak için **Kapalı'yı** seçin. **Streaming ingestion**
1. **Kaydet'i**seçin.

    ![Akış işlemi](media/ingest-data-streaming/streaming-ingestion-off.png)

## <a name="limitations"></a>Sınırlamalar

* Akış alma Veritabanı [imleçleri](/azure/kusto/management/databasecursor) veya [Veri eşleme](/azure/kusto/management/mappings)desteklemez. Yalnızca [önceden oluşturulmuş](/azure/kusto/management/create-ingestion-mapping-command) veri eşleme desteklenir. 
* Artırılmış VM ve küme boyutlarıyla akış alma performansı ve kapasite ölçekleri. Eşzamanlı alımlar çekirdek başına altı yutma ile sınırlıdır. Örneğin, D14 ve L16 gibi 16 çekirdekli SNU için maksimal destekli yük 96 eşzamanlı yutmadır. D11 gibi iki çekirdekli SNU için maksimal destekli yük 12 eşzamanlı yutmadır.
* Yutma isteği başına veri boyutu sınırlaması 4 MB'dır.
* Tabloların oluşturulması ve değiştirilmesi ve alım eşlemeleri gibi şema güncelleştirmeleri, akış alma hizmeti için beş dakika kadar sürebilir.
* Veriler akış yoluyla yutulmadığı halde, küme üzerinde akış akışını etkinleştirmek, akış verilerini akışiçin küme makinelerinin yerel SSD diskinin bir kısmını kullanır ve sıcak önbellek için kullanılabilir depolamayı azaltır.
* Akış verisi üzerinde [extent etiketleri](/azure/kusto/management/extents-overview#extent-tagging) ayarlanamıyor.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini'nde verileri sorgula](web-query-data.md)
