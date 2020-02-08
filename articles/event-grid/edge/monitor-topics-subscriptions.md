---
title: Konuları ve olay aboneliklerini izleme-Azure Event Grid IoT Edge | Microsoft Docs
description: Konuları ve olay aboneliklerini izleme
author: banisadr
ms.author: babanisa
ms.reviewer: spelluru
ms.date: 01/09/2020
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: ce7c92f121fb458d528d63d0af0aad025b377386
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2020
ms.locfileid: "77086666"
---
# <a name="monitor-topics-and-event-subscriptions"></a>Konuları ve olay aboneliklerini izleme

Event Grid Edge 'de, [Prometheus Exposition biçimindeki](https://prometheus.io/docs/instrumenting/exposition_formats/)konular ve olay abonelikleri için bir dizi ölçüm kullanıma sunar. Bu makalede, kullanılabilir ölçümler ve bunların nasıl etkinleştirileceği açıklanır.

## <a name="enable-metrics"></a>Ölçümleri etkinleştir

`metrics__reporterType` ortam değişkenini kapsayıcı oluşturma seçeneklerinde `prometheus` olarak ayarlayarak ölçümleri yayma için modülü yapılandırın:

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

Ölçümler, http için modülün `5888/metrics` ve https için `4438/metrics` kullanılabilir. Örneğin, http için `http://<modulename>:5888/metrics?api-version=2019-01-01-preview`. Bu noktada, ölçüm modülü bu [örnek mimaride](https://github.com/veyalla/ehm)olduğu gibi ölçümleri toplamak için uç noktayı yoklayabilirler.

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
| deliveryFailureCounts | Yapılandırılmış uç noktaya teslim edilemedi olay sayısı
| Deliveryms | Milisaniye olarak başarıyla teslim edilen olayların gecikmesi
| Deliveryfailurelat, MS | Milisaniye cinsinden olayların teslim hatalarının gecikmesi
| systemDelayForFirstAttemptMs | İlk teslim denemesinden önce olayların sistem gecikmesi (milisaniye olarak)
| Ityattemptscount | Olay teslim denemesi sayısı-başarılı ve başarısız
| expiredCounts | Zaman aşımına uğradığı ve yapılandırılan uç noktaya teslim edilmemiş olay sayısı