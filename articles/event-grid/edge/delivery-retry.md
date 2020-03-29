---
title: Teslim ve yeniden deneme - Azure Event Grid IoT Edge | Microsoft Dokümanlar
description: IoT Edge'deki Event Grid'de teslimat ve yeniden deneme.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/29/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 7df283b12a0d04d2b785c13a2f12b03115581e79
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76841721"
---
# <a name="delivery-and-retry"></a>Teslim ve yeniden deneme

Olay Izgara dayanıklı teslimat sağlar. Eşleşen her abonelik için her iletiyi en az bir kez hemen iletmeye çalışır. Abonenin bitiş noktası bir olayın alındığını kabul etmiyorsa veya bir hata varsa, Olay Idamı teslimini sabit bir **yeniden deneme zamanlamasına** göre yeniden dene ve yeniden deneme ilkesine göre yeniden **denediğinde.**  Varsayılan olarak, Olay Izgara modülü aboneye aynı anda bir olay sunar. Ancak yük, tek bir olayiçeren bir dizidir. Çıktı toplu iş özelliğini etkinleştirerek modülün aynı anda birden fazla olay sunmasını sağlayabilirsiniz. Bu özellik hakkında ayrıntılı bilgi için [çıktı toplu işlemi'ne](delivery-output-batching.md)bakın.  

> [!IMPORTANT]
>Olay verileri için kalıcılık desteği yoktur. Bu, Olay Izgara modülünün yeniden dağıtılması veya yeniden başlatılmasının henüz teslim edilmemiş olayları kaybetmenize neden olacağı anlamına gelir.

## <a name="retry-schedule"></a>Yeniden deneme çizelgesi

Olay Grid, bir ileti gönderdikten sonra yanıt için 60 saniyeye kadar bekler. Abonenin bitiş noktası yanıtı ACK değilse, ileti sonraki yeniden denemeler için arka sıralarımızdan birinde sıraya alınır.

Yeniden denemenin deneneceği zamanlamayı belirleyen önceden yapılandırılmış geri dönüş kuyrukları vardır. Bunlar:

| Zamanlama | Açıklama |
| ---------| ------------ |
| 1 dakika | Burada son veren mesajlar her dakika denenir.
| 10 dakika | Burada son veren mesajlar her 10 dakikada bir denenir.

### <a name="how-it-works"></a>Nasıl çalışır?

1. İleti Olay Izgara modülüne gelir. Hemen teslim etmek için girişimde bulunulması.
1. Teslim başarısız olursa, ileti 1 dakikalık sıraya sıralanır ve bir dakika sonra yeniden denener.
1. Teslim başarısız olmaya devam ederse, ileti 10 dakikalık sıraya sıralanır ve her 10 dakikada bir yeniden denendir.
1. Teslimatlar, başarılı olana veya yeniden deneme ilkesi sınırlarına ulaşılına kadar denenir.

## <a name="retry-policy-limits"></a>İlke sınırlarını yeniden deneyin

Yeniden deneme ilkesini belirleyen iki yapılandırma vardır. Bunlar:

* Maksimum deneme sayısı
* Etkinlik süresi canlı (TTL)

Yeniden deneme ilkesinin sınırlarından herhangi biri aşılırsa bir olay bırakılır. Yeniden deneme çizelgesinin kendisi Yeniden Deneme Çizelgesi bölümünde açıklanmıştır. Bu sınırların yapılandırması tüm aboneler için veya abonelik bazında yapılabilir. Aşağıdaki bölümde her biri daha ayrıntılı açıklar.

## <a name="configuring-defaults-for-all-subscribers"></a>Tüm aboneler için varsayılanları yapılandırma

İki özellik `brokers__defaultMaxDeliveryAttempts` vardır: `broker__defaultEventTimeToLiveInSeconds` ve tüm aboneler için yeniden deneme ilkesi varsayılanlarını denetleyen Olay Izgara dağıtımının bir parçası olarak yapılandırılabilir.

| Özellik Adı | Açıklama |
| ---------------- | ------------ |
| `broker__defaultMaxDeliveryAttempts` | Bir olayı teslim etmek için maksimum sayıda deneme. Varsayılan değer: 30.
| `broker__defaultEventTimeToLiveInSeconds` | Olay TTL saniye sonra bir olay teslim edilmezse bırakılır. Varsayılan değer: **7200** saniye

## <a name="configuring-defaults-per-subscriber"></a>Varsayılanları abone başına yapılandırma

Abonelik başına yeniden deneme ilkesi sınırlarını da belirtebilirsiniz.
Abone başına varsayılanları yapılandırmanın nasıl yapılacağı hakkında bilgi için [API belgelerimize](api.md) bakın. Abonelik düzeyi varsayılanları modül düzeyi yapılandırmalarını geçersiz kılar.

## <a name="examples"></a>Örnekler

Aşağıdaki örnek, maksimumNumberOfGirişimleri = 3 ve Olay TTL 30 dakika ile Olay Izgara modülünde yeniden deneme ilkesi ayarlar

```json
{
  "Env": [
    "broker__defaultMaxDeliveryAttempts=3",
    "broker__defaultEventTimeToLiveInSeconds=1800"
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

Aşağıdaki örnekmaxNumberOfGirişimleri = 3 ve Olay TTL 30 dakika ile bir Web hook abonelik kurar

```json
{
 "properties": {
  "destination": {
   "endpointType": "WebHook",
   "properties": {
    "endpointUrl": "<your_webhook_url>",
    "eventDeliverySchema": "eventgridschema"
   }
  },
  "retryPolicy": {
   "eventExpiryInMinutes": 30,
   "maxDeliveryAttempts": 3
  }
 }
}
```
