---
title: Verileri Azure Veri Gezgini almak için akış alımı kullanın
description: Akış alma kullanarak verileri Azure Veri Gezgini 'a alma (yükleme) hakkında bilgi edinin.
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/30/2019
ms.openlocfilehash: d7d2bcf487c37fbb523b648d5aa4c572add5dfa9
ms.sourcegitcommit: c29b7870f1d478cec6ada67afa0233d483db1181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79297093"
---
# <a name="streaming-ingestion-preview"></a>Akış alma (Önizleme)

Değişen birim verileri için 10 saniyeden daha az bir alım süresiyle düşük gecikme süresi gerektirdiğinde akış alımı kullanın. Bir veya daha fazla veritabanında, her bir tabloya veri akışının görece küçük (saniyede birkaç kayıt) olduğu, ancak genel veri alma birimi yüksek (saniyede binlerce kayıt) olduğu bir veya daha fazla veritabanında işlemsel işlemeyi iyileştirmek için kullanılır. 

Veri miktarı her tablo için saniyede 1 MB 'a kadar büyürken akış alımı yerine toplu alma kullanın. Çeşitli alma yöntemleri hakkında daha fazla bilgi edinmek için [veri alımı genel bakış](/azure/data-explorer/ingest-data-overview) makalesini okuyun.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Web Kullanıcı arabiriminde](https://dataexplorer.azure.com/)oturum açın.
* [Azure Veri Gezgini kümesi ve veritabanı](create-cluster-database-portal.md) oluşturma

## <a name="enable-streaming-ingestion-on-your-cluster"></a>Kümenizde akış alımını etkinleştirme

> [!WARNING]
> Lütfen akış alma işlemi etkinleştirilmeden önce [sınırlamaları](#limitations) gözden geçirin.

1. Azure portal Azure Veri Gezgini kümenize gidin. **Ayarlar**' da, **Konfigürasyonlar**' ı seçin. 
1. **Yapılandırma** bölmesinde, **akış**alımı 'nı etkinleştirmek için **Açık** ' ı seçin.
1. **Kaydet**’i seçin.
 
    ![akış alımı](media/ingest-data-streaming/streaming-ingestion-on.png)
 
1. [Web Kullanıcı arabiriminde](https://dataexplorer.azure.com/), veri akışı verileri alacak tablo (ler) veya veritabanları üzerinde [akış alma ilkesi](/azure/kusto/management/streamingingestionpolicy) tanımlayın. 

    > [!NOTE]
    > * İlke veritabanı düzeyinde tanımlanmışsa, veritabanındaki tüm tablolar akış alımı için etkinleştirilir.
    > * Uygulanan ilke, veritabanındaki diğer tablolara değil, yalnızca yeni alınan verilere başvurabilir.

## <a name="use-streaming-ingestion-to-ingest-data-to-your-cluster"></a>Verileri kümenize almak için akış alımı kullanın

Desteklenen iki akış alma türü vardır:


* Veri kaynağı olarak kullanılan [**Olay Hub**](/azure/data-explorer/ingest-data-event-hub)'ı
* **Özel** Alım, Azure Veri Gezgini istemci kitaplıklarından birini kullanan bir uygulama yazmanızı gerektirir. Örnek bir uygulama için bkz. [akış alma örneği](https://github.com/Azure/azure-kusto-samples-dotnet/tree/master/client/StreamingIngestionSample) .

### <a name="choose-the-appropriate-streaming-ingestion-type"></a>Uygun akış alma türünü seçin

|   |Olay Hub'ı  |Özel Alım  |
|---------|---------|---------|
|Sorgu başlatma ve sorgu için kullanılabilen veriler arasındaki veri gecikmesi   |    daha uzun gecikme     |   daha kısa gecikme      |
|Geliştirme ek yükü    |   hızlı ve kolay kurulum, geliştirme yükü yok    |   uygulamanın hataları işlemesi ve veri tutarlılığı sağlamak için yüksek geliştirme yükü     |

## <a name="disable-streaming-ingestion-on-your-cluster"></a>Kümenizde akış alımını devre dışı bırakma

> [!WARNING]
> Akış alımını devre dışı bırakmak birkaç saat sürebilir.

1. Tüm ilgili tablo ve veritabanlarından akış alma giriş [ilkesini](/azure/kusto/management/streamingingestionpolicy) bırakın. Akış alma ilkesi kaldırma işlemi, ilk depolamadan, sütun deposundaki (kapsamlar veya parçalar) kalıcı depolamaya giriş verilerini taşımayı tetikler. Veri taşıma işlemi, ilk depolama alanındaki veri miktarına ve CPU ve belleğin küme tarafından nasıl kullanıldığına bağlı olarak birkaç saniyelik birkaç saate kadar sürebilir.
1. Azure portal Azure Veri Gezgini kümenize gidin. **Ayarlar**' da, **Konfigürasyonlar**' ı seçin. 
1. **Yapılandırma** bölmesinde, **akış alımını**devre dışı bırakmak için **kapalı** ' yı seçin.
1. **Kaydet**’i seçin.

    ![Akış alma kapalı](media/ingest-data-streaming/streaming-ingestion-off.png)

## <a name="limitations"></a>Sınırlamalar

* Akış alımı, [veritabanı imleçleri](/azure/kusto/management/databasecursor) veya [veri eşlemeyi](/azure/kusto/management/mappings)desteklemez. Yalnızca [önceden oluşturulmuş](/azure/kusto/management/create-ingestion-mapping-command) veri eşleme destekleniyor. 
* Artan VM ve küme boyutları ile akış alma performansı ve kapasitesi ölçeklenir. Eş zamanlı içeri lamalar, çekirdek başına altı alma ile sınırlıdır. Örneğin, D14 ve L16 gibi 16 çekirdekli SKU 'Lar için, desteklenen en fazla 96 yükü eşzamanlı Alım olur. D11 gibi iki çekirdek SKU 'su için, en fazla desteklenen yük 12 eşzamanlı Alım olur.
* Alma isteği başına veri boyutu sınırlaması 4 MB 'tır.
* Tablo ve giriş eşlemelerinin oluşturulması ve değiştirilmesi gibi şema güncelleştirmeleri, akış alma hizmeti için beş dakikaya kadar sürebilir.
* Veri akışı aracılığıyla yapılmasa bile, küme üzerinde akış alımı etkinleştirme, verileri akışa alma için küme makinelerinin yerel SSD disk bir kısmını kullanır ve etkin önbellek için kullanılabilir depolama alanını azaltır.
* Akış alma verilerinde [kapsam etiketleri](/azure/kusto/management/extents-overview#extent-tagging) ayarlanamaz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini 'de verileri sorgulama](web-query-data.md)
