---
title: Konuları ve etkinlik aboneliklerini izleyin - Azure Event Grid IoT Edge | Microsoft Dokümanlar
description: Konuları ve etkinlik aboneliklerini izleme
author: banisadr
ms.author: babanisa
ms.reviewer: spelluru
ms.date: 01/09/2020
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: ce7c92f121fb458d528d63d0af0aad025b377386
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77086666"
---
# <a name="monitor-topics-and-event-subscriptions"></a>Konuları ve etkinlik aboneliklerini izleme

Kenardaki Olay Izgarası, [Prometheus fuar biçiminde](https://prometheus.io/docs/instrumenting/exposition_formats/)konular ve olay abonelikleri için bir dizi ölçüm ortaya çıkarır. Bu makalede, kullanılabilir ölçümler ve bunları etkinleştirmek için nasıl açıklanmaktadır.

## <a name="enable-metrics"></a>Ölçümleri etkinleştirme

`metrics__reporterType` Ortam değişkenini `prometheus` kapsayıcıdaki seçeneklere ayarlayarak modülü ölçümler ibareleri yatacak şekilde yapılandırın:

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

Ölçümler http ve `5888/metrics` `4438/metrics` https için modülün kullanılabilir olacaktır. Örneğin, `http://<modulename>:5888/metrics?api-version=2019-01-01-preview` http için. Bu noktada, bir metrik modülü bu [örnek mimaride](https://github.com/veyalla/ehm)olduğu gibi ölçümleri toplamak için bitiş noktasını yoklayabilir.

## <a name="available-metrics"></a>Mevcut ölçümler

Hem konular hem de etkinlik abonelikleri, etkinlik teslimi ve modül performansı hakkında bilgi vermek için ölçümler yontarak.

### <a name="topic-metrics"></a>Konu ölçümleri

| Ölçüm | Açıklama |
| ------ | ----------- |
| Alınan Etkinlikler | Konuyla ilgili yayınlanan etkinlik sayısı
| Eşsiz Etkinlikler | Olay Aboneliğiyle eşleşmeyen ve bırakılan konuya göre yayınlanan etkinlik sayısı
| Başarı İstekleri | Konu tarafından alınan gelen yayımlama isteklerinin sayısı
| SystemErrorRequests | İç sistem hatası nedeniyle başarısız olan gelen yayımlama isteklerinin sayısı
| UserErrorRequests | Gelen yayımlama isteklerindeki numara, hatalı biçimlendirilmiş JSON gibi kullanıcı hatası nedeniyle başarısız oldu
| SuccessRequestLatencyMs | İstek yanıtı gecikmesi milisaniye cinsinden yayımlama


### <a name="event-subscription-metrics"></a>Olay abonelik ölçümleri

| Ölçüm | Açıklama |
| ------ | ----------- |
| TeslimatBaşarı Sayıları | Yapılandırılan bitiş noktasına başarıyla teslim edilen olay sayısı
| TeslimatBaşarısızlık Sayıları | Yapılandırılan bitiş noktasına teslim edilmeyen olay sayısı
| TeslimatBaşarıLatencyMs | Milisaniye cinsinden başarıyla teslim edilen olayların gecikmesi
| TeslimatFailureLatencyMs | Olayların teslim hatalarının milisaniye cinsinden gecikmesi
| SystemDelayForFirstattemptms | Milisaniye cinsinden ilk teslimat denemesinden önceki olayların sistem gecikmesi
| TeslimatDenemeSayısı | Olay teslim girişimleri nin sayısı - başarı ve başarısızlık
| Süresi Dolmuş Sayımlar | Süresi dolan ve yapılandırılan bitiş noktasına teslim edilmeyen olayların sayısı