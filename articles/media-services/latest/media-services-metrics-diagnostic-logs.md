---
title: Azure Monitor ile Medya Hizmetleri ölçümleri ve tanılama günlükleri
titleSuffix: Azure Media Services
description: Azure Medya Hizmetleri ölçümlerini ve tanıgünlüklerini Azure Monitor üzerinden nasıl izleyeceğinizi öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2019
ms.author: juliako
ms.openlocfilehash: f075362f976e6abb26c9781c4b0cdeb7912c0862
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76514043"
---
# <a name="monitor-media-services-metrics-and-diagnostic-logs-via-azure-monitor"></a>Azure Monitor aracılığıyla Medya Hizmetleri ölçümlerini ve tanılama günlüklerini izleyin

[Azure Monitör,](../../azure-monitor/overview.md) uygulamalarınızın nasıl performans gösterdiğini anlamanıza yardımcı olan ölçümleri ve tanılama günlüklerini izlemenize olanak tanır. Azure Monitor tarafından toplanan tüm veriler iki temel türden birine uyar: ölçümler ve günlükler. Medya Hizmetleri tanı günlüklerini izleyebilir ve toplanan ölçümler ve günlükler için uyarılar ve bildirimler oluşturabilirsiniz. [Ölçümler explorer'ı](../../azure-monitor/platform/metrics-getting-started.md)kullanarak ölçüm verilerini görselleştirebilir ve analiz edebilirsiniz. Günlükleri [Azure Depolama'ya](https://azure.microsoft.com/services/storage/)gönderebilir, [Azure Etkinlik Hub'larına](https://azure.microsoft.com/services/event-hubs/)aktarabilir, [Günlük Analitiği'ne](https://azure.microsoft.com/services/log-analytics/)aktarabilir veya üçüncü taraf hizmetlerini kullanabilirsiniz.

Ayrıntılı bir genel bakış için Azure [Monitör Ölçümleri](../../azure-monitor/platform/data-platform.md) ve [Azure Monitör Tanılama günlüklerine](../../azure-monitor/platform/platform-logs-overview.md)bakın.

Bu konu desteklenen [Medya Hizmetleri Ölçümleri](#media-services-metrics) ve [Medya Hizmetleri Tanıgünlükleri](#media-services-diagnostic-logs)tartışır.

## <a name="media-services-metrics"></a>Medya Hizmetleri ölçümleri

Ölçümler, değerin değişip değişmemesi için düzenli aralıklarla toplanır. Sık sık örneklenebilir ve bir uyarı nispeten basit bir mantıkla hızlı bir şekilde ateşlenebilir, çünkü uyarı için yararlıdır. Metrik uyarıların nasıl oluşturulacağınız hakkında bilgi için Azure [Monitor kullanarak metrik uyarıları oluştur, görüntüleme ve yönetme](../../azure-monitor/platform/alerts-metric.md)bilgisine bakın.

Medya Hizmetleri, aşağıdaki kaynaklar için izleme ölçümlerini destekler:

* Hesap
* Akış Bitiş Noktası

### <a name="account"></a>Hesap

Aşağıdaki hesap ölçümlerini izleyebilirsiniz.

|Ölçüm adı|Görünen ad|Açıklama|
|---|---|---|
|Varlık Sayısı|Varlık sayısı|Hesabınızdaki varlıklar.|
|Varlık Kotası|Varlık kotası|Hesabınızdaki varlık kotası.|
|Varlık KotasıKullanılma Yüzdesi|Varlık kotası kullanılan yüzde|Zaten kullanılan Varlık kotasının yüzdesi.|
|İçerikKeyPolicyCount|İçerik Anahtar İlkesi sayısı|Hesabınızdaki İçerik Temel İlkeleri.|
|İçerikKeyPolicyKota|İçerik Anahtar İlkesi kotası|Hesabınızdaki İçerik Temel İlkeleri kotası.|
|İçerikKeyPolicyQuotaUsedPercentage|İçerik Anahtar İlkesi kotası kullanılan yüzde|İçerik Anahtarı İlkesi kotasının zaten kullanılan yüzdesi.|
|Akış Politikası Sayısı|Akış İlkesi sayısı|Hesabınızda Akış İlkeleri.|
|StreamingPolicyQuota|Akış İlkesi kotası|Hesabınızda Akış İlkeleri kotası.|
|StreamingPolicyQuotaUsedPercentage|Akış İlkesi kotası kullanılan yüzde|Zaten kullanılan Akış İlkesi kotasının yüzdesi.|

[Hesap kotalarını ve sınırlamalarını](limits-quotas-constraints.md)da gözden geçirmelisiniz.

### <a name="streaming-endpoint"></a>Akış Bitiş Noktası

Aşağıdaki Medya Hizmetleri [Akış Bitiş Noktaları](https://docs.microsoft.com/rest/api/media/streamingendpoints) ölçümleri desteklenir:

|Ölçüm adı|Görünen ad|Açıklama|
|---|---|---|
|İstekler|İstekler|Akış Bitiş Noktası tarafından sunulan toplam HTTP isteği sayısını sağlar.|
|Çıkış|Çıkış|Çıkış baytlarının toplam sayısı. Örneğin, Akış Bitiş Noktası tarafından akışlı baytlar.|
|SuccessE2ELatency|Gecikme sonu için başarı sona erer|Akış Bitiş Noktası'nın isteği aldığı tarihten yanıtın son baytının gönderildiği zamana kadar geçen süre.|

### <a name="why-would-i-want-to-use-metrics"></a>Neden ölçümleri kullanmak isteyeyim ki?

Aşağıda, Medya Hizmetleri ölçümlerinin izlenmesinin uygulamalarınızın nasıl performans gösterdiğini anlamanıza nasıl yardımcı olabileceğine örnekler verilmiştir. Medya Hizmetleri ölçümleri ile ele alınabilecek bazı sorular şunlardır:

* Sınırları ne zaman aştığımı öğrenmek için Standart Akış Bitiş Noktamı nasıl izleyebilirim?
* Yeterli Premium Akış Bitiş Noktası ölçeği birimim olup olmadığını nasıl anlarım?
* Akış Bitiş Noktalarımı ne zaman ölçeklendireceğimi bilmek için nasıl bir uyarı ayarlayabilirim?
* Hesapta yapılandırılan maksimum çıkışa ne zaman ulaşıldığını öğrenmek için nasıl bir uyarı ayarlıyorum?
* İsteklerin dökümünün başarısız olduğunu ve hataya neyin neden olduğunu nasıl görebiliyorum?
* Paketleyiciden kaç HLS veya DASH isteği nin çekildiğini nasıl görebiliyorum?
* Başarısız isteklerin # eşik değerinin ne zaman vurulduğunu bilmek için nasıl bir uyarı ayarlıyorum?

### <a name="example"></a>Örnek

[Bkz. Medya Hizmetleri ölçümleri nasıl izlenir.](media-services-metrics-howto.md)

## <a name="media-services-diagnostic-logs"></a>Medya Hizmetleri tanı günlükleri

Tanılama günlükleri, bir Azure kaynağının çalışması hakkında zengin ve sık veri sağlar. Daha fazla bilgi için Azure [kaynaklarınızdan günlük verilerini nasıl toplayıp tüketirize](../../azure-monitor/platform/platform-logs-overview.md)bakın.

Medya Hizmetleri aşağıdaki tanıgünlüklerini destekler:

* Anahtar teslimi

### <a name="key-delivery"></a>Anahtar teslimi

|Adı|Açıklama|
|---|---|
|Anahtar teslim hizmeti isteği|Anahtar teslim hizmeti istek bilgilerini gösteren günlükler. Daha fazla bilgi için [şemalara](media-services-diagnostic-logs-schema.md)bakın.|

### <a name="why-would-i-want-to-use-diagnostics-logs"></a>Neden tanılama günlüklerini kullanmak isteyeyim ki?

Anahtar teslim tanılama günlükleri ile inceleyebilirsiniz bazı şeyler şunlardır:

* DRM türüne göre teslim edilen lisans ların sayısına bakın.
* İlke tarafından teslim edilen lisans ların sayısına bakın.
* DRM veya ilke türüne göre hataları görün.
* İstemcilerden gelen yetkisiz lisans isteklerinin sayısına bakın.

### <a name="example"></a>Örnek

[Bkz. Medya Hizmeti tanı günlükleri nasıl izlenir.](media-services-diagnostic-logs-howto.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure kaynaklarınızdan günlük verileri toplama ve kullanma](../../azure-monitor/platform/platform-logs-overview.md)
* [Azure İzleyici'yi kullanarak ölçüm uyarıları oluşturma, görüntüleme ve yönetme](../../azure-monitor/platform/alerts-metric.md)
* [Medya Hizmetleri ölçümleri nasıl izlenir?](media-services-metrics-howto.md)
* [Media Service tanı günlükleri nasıl izlenir?](media-services-diagnostic-logs-howto.md)
