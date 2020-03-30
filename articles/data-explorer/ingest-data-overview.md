---
title: Azure Veri Gezgini veri alımı
description: Azure Veri Gezgini'nde veri yutabileceğiniz (yükleyebileceğiniz) farklı yollar hakkında bilgi edinin
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 02/18/2019
ms.openlocfilehash: 4846a19c403cce16bed704ed4e7c70499f3b5d13
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79246402"
---
# <a name="azure-data-explorer-data-ingestion"></a>Azure Veri Gezgini veri alımı

Veri alımı, Azure Veri Gezgini'nde bir tablo oluşturmak veya güncelleştirmek için bir veya daha fazla kaynaktan veri kayıtlarını yüklemek için kullanılan işlemdir. Bir kez yutularak, veriler sorgu için kullanılabilir hale gelir. Aşağıdaki diyagram, veri alımı da dahil olmak üzere Azure Veri Gezgini'nde çalışmak için uçuça akışı gösterir.

![Veri akışı](media/ingest-data-overview/data-flow.png)

Veri alımından sorumlu Olan Azure Veri Gezgini veri yönetimi hizmeti aşağıdaki işlevleri sağlar:

1. **Veri çekme**: Dış kaynaklardan (Olay Hub'ları) veri çekin veya Azure Kuyruğu'ndan alma isteklerini okuyun.

1. **Toplu İşlem**: Alım verisini optimize etmek için aynı veritabanına ve tabloya akan toplu veri.

1. **Doğrulama**: Gerekirse ön doğrulama ve biçim dönüştürme.

1. **Veri işleme**: Şema eşleştirme, düzenleme, dizine alma, kodlama ve veri sıkıştırma.

1. **Yutma akışındaki kalıcılık noktası**: Motorüzerindeki yutma yükünü yönetin ve geçici arızalar üzerine yeniden çalışır.

1. **Veri yutmayı commit**: Verileri sorgu için kullanılabilir hale getirir.

## <a name="ingestion-methods"></a>Yutma yöntemleri

Azure Veri Gezgini, her biri kendi hedef senaryoları, avantajları ve dezavantajları olan birkaç yutma yöntemini destekler. Azure Veri Gezgini, ortak hizmetlere boru hatları ve bağlayıcılar, SDK'lar kullanılarak programlanabilir şekilde aktarıcılık ve arama amacıyla motora doğrudan erişim sunar.

### <a name="ingestion-using-pipelines-connectors-and-plugins"></a>Boru hatları, konektörler ve eklentiler kullanılarak yutma

Azure Veri Gezgini şu anda şunları destekler:

* Azure portalındaki yönetim sihirbazı kullanılarak yönetilebilen Olay Grid ardışık hattı. Daha fazla bilgi için Azure [Veri Gezgini'ne Azure Blobs'u sindirin'](ingest-data-event-grid.md)e bakın.

* Azure portalındaki yönetim sihirbazı kullanılarak yönetilebilen Event Hub ardışık hattı. Daha fazla bilgi için [Olay Hub'ından Azure Veri Gezgini'ne veri alma konusuna](ingest-data-event-hub.md)bakın.

* Logstash eklentisi, [Logstash'tan Azure Veri Gezgini'ne veri alma](ingest-data-logstash.md)hakkında bilgi verin.

* Kafka bağlayıcısı, [Kafka'dan Azure Veri Gezgini'ne veri alma](ingest-data-kafka.md)bilgisine bakın.

### <a name="ingestion-using-integration-services"></a>Entegrasyon hizmetlerini kullanarak yutma

* [Desteklenen veri depolarını ve biçimlerini](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats)kullanarak Azure Veri Gezgini'ne veri kopyalamak için Azure'daki analitik iş yükleri için tam olarak yönetilen bir veri tümleştirme hizmeti olan Azure Veri Fabrikası (ADF). Daha fazla bilgi için bkz. Azure [Veri Fabrikası'ndan Azure Veri Gezgini'ne veri kopyalama.](/azure/data-explorer/data-factory-load-data)

### <a name="programmatic-ingestion"></a>Programlı alım

Azure Veri Gezgini, sorgu ve veri alımı için kullanılabilecek SDK'lar sağlar. Programlı alım, yutma işlemi sırasında ve sonrasında depolama işlemlerini en aza indirerek, yutma maliyetlerini (COGs) azaltmak için optimize edilmiştir.

**Mevcut SDK'lar ve açık kaynak projeleri:**

Kusto, verileri yutmak ve sorgulamak için kullanılabilecek istemci SDK'yı şunları ile sunar:

* [Python SDK'sı](/azure/kusto/api/python/kusto-python-client-library)

* [.NET SDK](/azure/kusto/api/netfx/about-the-sdk)

* [Java SDK](/azure/kusto/api/java/kusto-java-client-library)

* [Node SDK](/azure/kusto/api/node/kusto-node-client-library)

* [REST API](/azure/kusto/api/netfx/kusto-ingest-client-rest)

**Programlı yutma teknikleri:**

* Azure Veri Gezgini veri yönetimi hizmeti (yüksek iş letim ve güvenilir alım) aracılığıyla veri alma:

    [**Toplu işlem**](/azure/kusto/api/netfx/kusto-ingest-queued-ingest-sample) (SDK tarafından sağlanan): İstemci verileri Azure Blob depolama alanına yükler (Azure Veri Gezgini veri yönetimi hizmeti tarafından belirlenir) ve bir Azure Kuyruğu'na bir bildirim gönderir. Toplu işlem, yüksek hacimli, güvenilir ve ucuz veri alımı için önerilen tekniktir.

* Verileri doğrudan Azure Veri Gezgini motoruna alma (arama ve prototip oluşturma için en uygun):

  * **Satır içi yutma**: bant içi verileri içeren kontrol komutu (.yutma satırı) geçici test amaçları için tasarlanmıştır.

  * **Sorgudan yutma**: denetim komutu (.set, .set-or-append, .set-or-replace) sorgu sonuçlarına işaret eden raporlar veya küçük geçici tablolar oluşturmak için kullanılır.

  * **Depolamadan yutma**: denetim komutu (.içine yutmak) harici olarak depolanan verilerle (örneğin, Azure Blob Depolama) verimli bir şekilde veri toplanmasını sağlar.

**Farklı yöntemlerin gecikmesi:**

| Yöntem | Gecikme süresi |
| --- | --- |
| **Satır içinde yutma** | Hemen |
| **Sorgudan alma** | Sorgu süresi + işlem süresi |
| **Depolamadan yutma** | İndirme süresi + işlem süresi |
| **Sıraya alma** | Toplu işlem süresi + işlem süresi |
| |

İşlem süresi veri boyutuna bağlıdır, birkaç saniyeden daha kısa. Toplu işlem süresi varsayılan olarak 5 dakikaya kadar dır.

## <a name="choosing-the-most-appropriate-ingestion-method"></a>En uygun yutma yöntemini seçme

Verileri yutmaya başlamadan önce kendinize aşağıdaki soruları sormalısınız.

* Verilerim nerede bulunur? 
* Veri biçimi nedir ve değiştirilebilir mi? 
* Sorgulanacak gerekli alanlar nelerdir? 
* Beklenen veri hacmi ve hızı nedir? 
* Kaç olay türü bekleniyor (tablo sayısı olarak yansıtılır)? 
* Olay şemasının ne sıklıkta değişmesi bekleniyor? 
* Verileri kaç düğüm oluşturur? 
* Kaynak işletim sistemi nedir? 
* Gecikme gereksinimleri nelerdir? 
* Mevcut yönetilen boru hatlarından biri kullanılabilir mi? 

Event Hub ve IoT Hub gibi bir ileti hizmetini temel alan varolan altyapıya sahip kuruluşlar için, bağlayıcı kullanmak büyük olasılıkla en uygun çözümdür. Büyük veri hacimleri için sıraya alma uygundur.

## <a name="supported-data-formats"></a>Desteklenen veri biçimleri

Sorgudan alma dışındaki tüm yutma yöntemleri için, verileri Azure Veri Gezgini'ni ayrıştıracak şekilde biçimlendirin. 
* Desteklenen veri biçimleri şunlardır: TXT, CSV, TSV, TSVE, PSV, SCSV, SOH, JSON (çizgi ayrılmış, çok çizgili), Avro, Ork ve Parke. 
* ZIP ve GZIP sıkıştırmadestekler.

> [!NOTE]
> Veriler yutulurken, hedef tablo sütunları temel alınarak veri türleri çıkarılır. Bir kayıt eksikse veya bir alan gerekli veri türü olarak ayrıştırılamazsa, karşılık gelen tablo sütunları null değerleriyle doldurulur.

## <a name="ingestion-recommendations-and-limitations"></a>Yutma önerileri ve sınırlamaları

* Yutulan verilerin etkili saklama ilkesi veritabanının bekletme ilkesinden türetilmiştir. Ayrıntılar için [bekletme ilkesine](/azure/kusto/concepts/retentionpolicy) bakın. Veri yutma tablo **yutma** veya **Veritabanı ingestor** izinleri gerektirir.
* Yutma en fazla 5 GB dosya boyutunu destekler. Öneri, 100 MB ile 1 GB arasında dosya yutmaktır.

## <a name="schema-mapping"></a>Şema haritalama

Şema eşleme, kaynak veri alanlarını hedef tablo sütunlarına bağlamaya yardımcı olur.

* [CSV Haritalama](/azure/kusto/management/mappings?branch=master#csv-mapping) (isteğe bağlı) tüm ordinal tabanlı biçimlerle çalışır. Yutucu komut parametresi kullanılarak veya [tabloda önceden oluşturulmuş](/azure/kusto/management/create-ingestion-mapping-command) ve yutulan komut parametresinden başvurulan yapılabilir.
* [JSON Haritalama](/azure/kusto/management/mappings?branch=master#json-mapping) (zorunlu) ve [Avro haritalama](/azure/kusto/management/mappings?branch=master#avro-mapping) (zorunlu) yutulması komut parametresi kullanılarak gerçekleştirilebilir. Ayrıca [tabloda önceden oluşturulabilir](/azure/kusto/management/create-ingestion-mapping-command) ve yutulan komut parametresinden başvurulabilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Olay Hub'ından Azure Veri Gezgini'ne veri alma](ingest-data-event-hub.md)

> [!div class="nextstepaction"]
> [Azure Veri Gezgini'ne Olay Ağı aboneliğini kullanarak veri alma](ingest-data-event-grid.md)

> [!div class="nextstepaction"]
> [Kafka'dan Azure Veri Gezgini'ne veri alma](ingest-data-kafka.md)

> [!div class="nextstepaction"]
> [Azure Veri Gezgini Python kitaplığını kullanarak veri alma](python-ingest-data.md)

> [!div class="nextstepaction"]
> [Azure Veri Gezgini Düğümü kitaplığını kullanarak veri alma](node-ingest-data.md)

> [!div class="nextstepaction"]
> [Azure Veri Gezgini .NET Standart SDK'yı kullanarak veri alma (Önizleme)](net-standard-ingest-data.md)

> [!div class="nextstepaction"]
> [Logstash'tan Azure Veri Gezgini'ne veri alma](ingest-data-logstash.md)
