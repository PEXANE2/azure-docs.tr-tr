---
title: Azure Izleyici ile ölçümleri ve tanılama günlüklerini Media Services
titleSuffix: Azure Media Services
description: Azure Izleyici aracılığıyla Azure Media Services ölçümleri ve tanılama günlüklerini izlemeyi öğrenin.
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
ms.openlocfilehash: 7058160819c9b6ccc3f63511f86e930fcbfc413b
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87011660"
---
# <a name="monitor-media-services-metrics-and-diagnostic-logs-via-azure-monitor"></a>Azure Izleyici aracılığıyla Media Services ölçümleri ve tanılama günlüklerini izleme

[Azure izleyici](../../azure-monitor/overview.md) , uygulamalarınızın nasıl çalıştığını anlamanıza yardımcı olan ölçümleri ve tanılama günlüklerini izlemenize olanak sağlar. Azure Izleyici tarafından toplanan tüm veriler iki temel türden birine uyar: ölçümler ve Günlükler. Media Services tanılama günlüklerini izleyebilir, toplanan ölçümler ve Günlükler için uyarılar ve bildirimler oluşturabilirsiniz. Ölçüm [Gezgini](../../azure-monitor/platform/metrics-getting-started.md)'ni kullanarak ölçüm verilerini görselleştirebilir ve çözümleyebilirsiniz. [Azure depolama](https://azure.microsoft.com/services/storage/)'ya Günlükler gönderebilir, bunları [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)akışa alabilir, [Log Analytics](https://azure.microsoft.com/services/log-analytics/)dışarı aktarabilir veya üçüncü taraf hizmetleri kullanabilirsiniz.

Ayrıntılı bir genel bakış için bkz. [Azure Izleyici ölçümleri](../../azure-monitor/platform/data-platform.md) ve [Azure izleyici tanılama günlükleri](../../azure-monitor/platform/platform-logs-overview.md).

Bu konu, desteklenen [Media Services ölçümlerini](#media-services-metrics) ve [Media Services tanılama günlüklerini](#media-services-diagnostic-logs)açıklamaktadır.

## <a name="media-services-metrics"></a>Media Services ölçümleri

Ölçümler, değerin değiştirilip değişmediğini düzenli aralıklarla toplanır. Bunlar genelde örneklenebilir ve bir uyarı görece basit mantık ile hızlı bir şekilde tetiklenebilir. Ölçüm uyarıları oluşturma hakkında daha fazla bilgi için bkz. [Azure izleyici kullanarak ölçüm uyarıları oluşturma, görüntüleme ve yönetme](../../azure-monitor/platform/alerts-metric.md).

Media Services aşağıdaki kaynaklar için izleme ölçümlerini destekler:

* Hesap
* Akış uç noktası

### <a name="account"></a>Hesap

Aşağıdaki hesap ölçümlerini izleyebilirsiniz.

|Ölçüm adı|Görünen ad|Açıklama|
|---|---|---|
|AssetCount|Varlık sayısı|Hesabınızdaki varlıklar.|
|AssetQuota|Varlık kotası|Hesabınızdaki varlık kotası.|
|AssetQuotaUsedPercentage|Kullanılan varlık kotası yüzdesi|Zaten kullanılan varlık kotasının yüzdesi.|
|ContentKeyPolicyCount|İçerik anahtarı Ilke sayısı|Hesabınızdaki içerik anahtarı Ilkeleri.|
|ContentKeyPolicyQuota|İçerik anahtarı Ilke kotası|Hesabınızdaki içerik anahtarı Ilkeleri kotası.|
|ContentKeyPolicyQuotaUsedPercentage|İçerik anahtarı Ilke kotası kullanılan yüzde|Zaten kullanılan Içerik anahtarı Ilke kotasının yüzdesi.|
|Streammingpolicycount|Akış Ilkesi sayısı|Hesabınızdaki akış Ilkeleri.|
|StreamingPolicyQuota|Akış Ilkesi kotası|Hesabınızdaki akış Ilkeleri kotası.|
|StreamingPolicyQuotaUsedPercentage|Akış Ilkesi kotası kullanılan yüzde|Zaten kullanılan akış Ilkesi kotasının yüzdesi.|

[Hesap kotalarını ve sınırlarını](limits-quotas-constraints.md)de gözden geçirmeniz gerekir.

### <a name="streaming-endpoint"></a>Akış uç noktası

Aşağıdaki Media Services [akış uç noktası](/rest/api/media/streamingendpoints) ölçümleri desteklenir:

|Ölçüm adı|Görünen ad|Açıklama|
|---|---|---|
|İstekler|İstekler|Akış uç noktası tarafından hizmet verilen toplam HTTP isteği sayısını sağlar.|
|Çıkış|Çıkış|Akış uç noktası başına dakika başına toplam çıkış baytı.|
|SuccessE2ELatency|Başarılı uçtan uca gecikme süresi|Akış uç noktasının, yanıtın son baytı gönderilirken isteği aldığı zaman süresi.|

### <a name="why-would-i-want-to-use-metrics"></a>Ölçümleri neden kullanmak istiyorum?

İzleme Media Services ölçümlerinin uygulamalarınızın nasıl çalıştığını anlamanıza yardımcı olabilecek örnekler aşağıda verilmiştir. Media Services ölçümleriyle giderilebililecek bazı sorular şunlardır:

* Sınırları aşdığımda emin olmak için standart akış uç noktası mi Nasıl yaparım??
* Nasıl yaparım? en fazla Premium akış uç noktası ölçek birimi olup olmadığı hakkında bilgi sahibi misiniz?
* Akış uç noktalarımı ne zaman ölçeklendirebileceğinizi öğrenmek için bir uyarı ayarlayabilirim?
* Nasıl yaparım?, hesapta yapılandırılan en fazla çıkış ne zaman ulaşılırsa emin olmak için bir uyarı ayarladı musunuz?
* Başarısız olan isteklerin dökümünü nasıl görebilirim ve hataya neden olmuş olabilir?
* Paketleyiciyi kaç tane HLS veya DASH isteğinin çekmekte olduğunu nasıl görebilirim?
* Nasıl yaparım? başarısız isteklerin eşik değeri ne zaman isabet olduğunu bildirmek için bir uyarı ayarla.

### <a name="example"></a>Örnek

Bkz. [Media Services ölçümlerini izleme](media-services-metrics-howto.md).

## <a name="media-services-diagnostic-logs"></a>Tanılama günlüklerini Media Services

Tanılama günlükleri, bir Azure kaynağının çalışması hakkında zengin ve sık veriler sağlar. Daha fazla bilgi için bkz. [Azure kaynaklarınızdan günlük verilerini toplama ve kullanma](../../azure-monitor/platform/platform-logs-overview.md).

Media Services aşağıdaki tanılama günlüklerini destekler:

* Anahtar teslimi

### <a name="key-delivery"></a>Anahtar teslimi

|Ad|Açıklama|
|---|---|
|Anahtar teslim hizmeti isteği|Anahtar teslim hizmeti istek bilgilerini gösteren Günlükler. Daha fazla bilgi için bkz. [şemalar](media-services-diagnostic-logs-schema.md).|

### <a name="why-would-i-want-to-use-diagnostics-logs"></a>Tanılama günlüklerini neden kullanmak istiyorum?

Anahtar teslimi tanılama günlükleri ile inceleyebileceğiniz bazı şeyler şunlardır:

* DRM türü tarafından sunulan lisansların sayısını görün.
* İlke tarafından teslim edilen lisansların sayısını görün.
* Bkz. DRM veya ilke türüne göre hatalar.
* İstemcilerden gelen yetkisiz lisans isteği sayısını görün.

### <a name="example"></a>Örnek

Bkz. [Media Service tanılama günlüklerini izleme](media-services-diagnostic-logs-howto.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure kaynaklarınızdan günlük verilerini toplama ve kullanma](../../azure-monitor/platform/platform-logs-overview.md)
* [Azure İzleyici'yi kullanarak ölçüm uyarıları oluşturma, görüntüleme ve yönetme](../../azure-monitor/platform/alerts-metric.md)
* [Media Services ölçümlerini izleme](media-services-metrics-howto.md)
* [Medya hizmeti tanılama günlüklerini izleme](media-services-diagnostic-logs-howto.md)
