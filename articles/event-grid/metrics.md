---
title: Azure Event Grid tarafından desteklenen ölçümler
description: Bu makalede, Azure Event Grid hizmeti tarafından desteklenen Azure Izleyici ölçümleri sunulmaktadır.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 3b22beafc9f88d2d95b25fd7ad2f2308a4df9097
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86116429"
---
# <a name="metrics-supported-by-azure-event-grid"></a>Azure Event Grid tarafından desteklenen ölçümler
Bu makalede, ad alanlarına göre kategorilere ayrılan Event Grid ölçümleri listelenmiştir. 

## <a name="microsofteventgriddomains"></a>Microsoft. EventGrid/Domains

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Description|Boyutlar|
|---|---|---|---|---|---|
|Publishbaşarılı sayısı|Yayımlanan olaylar|Sayı|Toplam|Bu konuda yayınlanan toplam olay sayısı|Konu başlığı|
|PublishFailCount|Başarısız olayları Yayımla|Sayı|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|Konu, ErrorType, hata|
|Publishbaşarıyla Latencınms|Yayımlama başarı gecikmesi|Mayacak|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Hiçbiri|
|MatchedEventCount|Eşleşen olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName|
|Ityattemptfailcount|Teslim başarısız olaylar|Sayı|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName, hata, ErrorType|
|Deliverybaşarılı sayısı|Teslim edilen olaylar|Sayı|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName|
|DestinationProcessingDurationInMs|Hedef Işleme süresi|Mayacak|Ortalama|Milisaniye cinsinden hedef işleme süresi|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName|
|DroppedEventCount|Bırakılan olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName, DropReason|
|DeadLetteredCount|Kullanılmayan olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName, DeadLetterReason|

## <a name="microsofteventgridtopics"></a>Microsoft. EventGrid/konuları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Description|Boyutlar|
|---|---|---|---|---|---|
|Publishbaşarılı sayısı|Yayımlanan olaylar|Sayı|Toplam|Bu konuda yayınlanan toplam olay sayısı|Hiçbiri|
|PublishFailCount|Başarısız olayları Yayımla|Sayı|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|ErrorType, hata|
|UnmatchedEventCount|Eşleşmeyen olaylar|Sayı|Toplam|Bu konu için olay abonelikleriyle eşleşmeyen toplam olay sayısı|Hiçbiri|
|Publishbaşarıyla Latencınms|Yayımlama başarı gecikmesi|Mayacak|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Hiçbiri|
|MatchedEventCount|Eşleşen olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|EventSubscriptionName|
|Ityattemptfailcount|Teslim başarısız olaylar|Sayı|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Hata, ErrorType, EventSubscriptionName|
|Deliverybaşarılı sayısı|Teslim edilen olaylar|Sayı|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|EventSubscriptionName|
|DestinationProcessingDurationInMs|Hedef Işleme süresi|Mayacak|Ortalama|Milisaniye cinsinden hedef işleme süresi|EventSubscriptionName|
|DroppedEventCount|Bırakılan olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|DropReason, EventSubscriptionName|
|DeadLetteredCount|Kullanılmayan olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|DeadLetterReason, EventSubscriptionName|

## <a name="microsofteventgridsystemtopics"></a>Microsoft. EventGrid/Systemkonuları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Description|Boyutlar|
|---|---|---|---|---|---|
|Publishbaşarılı sayısı|Yayımlanan olaylar|Sayı|Toplam|Bu konuda yayınlanan toplam olay sayısı|Hiçbiri|
|PublishFailCount|Başarısız olayları Yayımla|Sayı|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|ErrorType, hata|
|UnmatchedEventCount|Eşleşmeyen olaylar|Sayı|Toplam|Bu konu için olay abonelikleriyle eşleşmeyen toplam olay sayısı|Hiçbiri|
|Publishbaşarıyla Latencınms|Yayımlama başarı gecikmesi|Mayacak|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Hiçbiri|
|MatchedEventCount|Eşleşen olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|EventSubscriptionName|
|Ityattemptfailcount|Teslim başarısız olaylar|Sayı|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Hata, ErrorType, EventSubscriptionName|
|Deliverybaşarılı sayısı|Teslim edilen olaylar|Sayı|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|EventSubscriptionName|
|DestinationProcessingDurationInMs|Hedef Işleme süresi|Mayacak|Ortalama|Milisaniye cinsinden hedef işleme süresi|EventSubscriptionName|
|DroppedEventCount|Bırakılan olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|DropReason, EventSubscriptionName|
|DeadLetteredCount|Kullanılmayan olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|DeadLetterReason, EventSubscriptionName|

## <a name="microsofteventgrideventsubscriptions"></a>Microsoft. EventGrid/Eventabonelikleri

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Description|Boyutlar|
|---|---|---|---|---|---|
|MatchedEventCount|Eşleşen olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|Hiçbiri|
|Ityattemptfailcount|Teslim başarısız olaylar|Sayı|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Hata, ErrorType|
|Deliverybaşarılı sayısı|Teslim edilen olaylar|Sayı|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|Hiçbiri|
|DestinationProcessingDurationInMs|Hedef Işleme süresi|Mayacak|Ortalama|Milisaniye cinsinden hedef işleme süresi|Hiçbiri|
|DroppedEventCount|Bırakılan olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|DropReason|
|DeadLetteredCount|Kullanılmayan olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|DeadLetterReason|

## <a name="microsofteventgridextensiontopics"></a>Microsoft. EventGrid/Extensionkonuları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Description|Boyutlar|
|---|---|---|---|---|---|
|Publishbaşarılı sayısı|Yayımlanan olaylar|Sayı|Toplam|Bu konuda yayınlanan toplam olay sayısı|Hiçbiri|
|PublishFailCount|Başarısız olayları Yayımla|Sayı|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|ErrorType, hata|
|UnmatchedEventCount|Eşleşmeyen olaylar|Sayı|Toplam|Bu konu için olay abonelikleriyle eşleşmeyen toplam olay sayısı|Hiçbiri|
|Publishbaşarıyla Latencınms|Yayımlama başarı gecikmesi|Mayacak|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Hiçbiri|

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleye bakın: [tanılama günlükleri](diagnostic-logs.md)
