---
title: Konuları ve olay aboneliklerini izleme-Azure Event Grid IoT Edge | Microsoft Docs
description: Konuları ve olay aboneliklerini izleme
ms.date: 07/08/2020
ms.topic: article
ms.openlocfilehash: d230be4f74abd61ad7b7f0fdb3facb32ee63da73
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86171542"
---
# <a name="monitor-topics-and-event-subscriptions"></a>Konuları ve olay aboneliklerini izleme

Event Grid Edge 'de, [Prometheus Exposition biçimindeki](https://prometheus.io/docs/instrumenting/exposition_formats/)konular ve olay abonelikleri için bir dizi ölçüm kullanıma sunar. Bu makalede, kullanılabilir ölçümler ve bunların nasıl etkinleştirileceği açıklanır.

## <a name="enable-metrics"></a>Ölçümleri etkinleştir

`metrics__reporterType`Ortam değişkenini kapsayıcı oluşturma seçeneklerinde olarak ayarlayarak ölçümleri yayma için modülü yapılandırın `prometheus` :

 ```json
        {
          "Env": [
            "metrics__reporterType=prometheus"
          ],
          "HostConfig": {
            "PortBindings": {
              "4438/tcp": [
                {
                    "HostPort": "4438"
                }
              ]
            }
          }
        }
 ```    

Ölçümler `5888/metrics` , http ve https için modülünde kullanılabilir olacaktır `4438/metrics` . Örneğin, `http://<modulename>:5888/metrics?api-version=2019-01-01-preview` http için. Bu noktada, ölçüm modülü bu [örnek mimaride](https://github.com/veyalla/ehm)olduğu gibi ölçümleri toplamak için uç noktayı yoklayabilirler.

## <a name="available-metrics"></a>Mevcut ölçümler

Hem konular hem de olay abonelikleri, olay teslimi ve modül performansı hakkında Öngörüler sunmak için ölçümleri yayar.

### <a name="topic-metrics"></a>Konu ölçümleri

| Ölçüm | Açıklama |
| ------ | ----------- |
| Alınan olayları | Konuya yayınlanan olay sayısı
| UnmatchedEvents | Konuya yayımlanan ve olay aboneliğiyle eşleşmeyen ve bırakılan olay sayısı
| Başarılı Istek sayısı | Konu tarafından alınan gelen yayımlama isteği sayısı
| SystemErrorRequests | İç sistem hatası nedeniyle başarısız olan gelen yayımlama isteği sayısı
| UserErrorRequests | Hatalı biçimlendirilmiş JSON gibi Kullanıcı hatası nedeniyle gelen yayımlama isteklerinin sayısı başarısız oldu
| Başarılı Istek | Milisaniye cinsinden yayımlama isteği yanıt gecikmesi


### <a name="event-subscription-metrics"></a>Olay aboneliği ölçümleri

| Ölçüm | Açıklama |
| ------ | ----------- |
| Deliverybaşarılı sayısı | Yapılandırılmış uç noktaya başarıyla teslim edilen olay sayısı
| DeliveryFailureCounts | Yapılandırılmış uç noktaya teslim edilemedi olay sayısı
| Deliveryms | Milisaniye olarak başarıyla teslim edilen olayların gecikmesi
| Deliveryfailurelat, MS | Milisaniye cinsinden olayların teslim hatalarının gecikmesi
| SystemDelayForFirstAttemptMs | İlk teslim denemesinden önce olayların sistem gecikmesi (milisaniye olarak)
| Ityattemptscount | Olay teslim denemesi sayısı-başarılı ve başarısız
| ExpiredCounts | Zaman aşımına uğradığı ve yapılandırılan uç noktaya teslim edilmemiş olay sayısı