---
title: Azure Event Grid tarafından desteklenen ölçümler
description: Bu makalede, Azure Event Grid hizmeti tarafından desteklenen Azure Izleyici ölçümleri sunulmaktadır.
ms.topic: conceptual
ms.date: 08/13/2020
ms.openlocfilehash: 06c43b1990efc977cae33ced3f66f02e2de0b9c4
ms.sourcegitcommit: 152c522bb5ad64e5c020b466b239cdac040b9377
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88225181"
---
# <a name="metrics-supported-by-azure-event-grid"></a>Azure Event Grid tarafından desteklenen ölçümler
Bu makalede, ad alanlarına göre kategorilere ayrılan Event Grid ölçümleri listelenmiştir. 

## <a name="microsofteventgriddomains"></a>Microsoft. EventGrid/Domains

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Evet|Kullanılmayan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName, DeadLetterReason|
|Ityattemptfailcount|Hayır|Teslim başarısız olaylar|Count|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName, hata, ErrorType|
|Deliverybaşarılı sayısı|Evet|Teslim edilen olaylar|Count|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName|
|DestinationProcessingDurationInMs|Hayır|Hedef Işleme süresi|Mayacak|Ortalama|Milisaniye cinsinden hedef işleme süresi|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName|
|DroppedEventCount|Evet|Bırakılan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName, DropReason|
|MatchedEventCount|Evet|Eşleşen olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName|
|PublishFailCount|Evet|Başarısız olayları Yayımla|Count|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|Konu, ErrorType, hata|
|Publishbaşarılı sayısı|Evet|Yayımlanan olaylar|Count|Toplam|Bu konuda yayınlanan toplam olay sayısı|Konu|
|Publishbaşarıyla Latencınms|Evet|Yayımlama başarı gecikmesi|Mayacak|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Boyut yok|


## <a name="microsofteventgrideventsubscriptions"></a>Microsoft. EventGrid/Eventabonelikleri

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Evet|Kullanılmayan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|DeadLetterReason|
|Ityattemptfailcount|Hayır|Teslim başarısız olaylar|Count|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Hata, ErrorType|
|Deliverybaşarılı sayısı|Evet|Teslim edilen olaylar|Count|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|Boyut yok|
|DestinationProcessingDurationInMs|Hayır|Hedef Işleme süresi|Mayacak|Ortalama|Milisaniye cinsinden hedef işleme süresi|Boyut yok|
|DroppedEventCount|Evet|Bırakılan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|DropReason|
|MatchedEventCount|Evet|Eşleşen olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|Boyut yok|


## <a name="microsofteventgridextensiontopics"></a>Microsoft. EventGrid/Extensionkonuları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|PublishFailCount|Evet|Başarısız olayları Yayımla|Count|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|ErrorType, hata|
|Publishbaşarılı sayısı|Evet|Yayımlanan olaylar|Count|Toplam|Bu konuda yayınlanan toplam olay sayısı|Boyut yok|
|Publishbaşarıyla Latencınms|Evet|Yayımlama başarı gecikmesi|Mayacak|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Boyut yok|
|UnmatchedEventCount|Evet|Eşleşmeyen olaylar|Count|Toplam|Bu konu için olay abonelikleriyle eşleşmeyen toplam olay sayısı|Boyut yok|


## <a name="microsofteventgridsystemtopics"></a>Microsoft. EventGrid/Systemkonuları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Evet|Kullanılmayan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|DeadLetterReason, EventSubscriptionName|
|Ityattemptfailcount|Hayır|Teslim başarısız olaylar|Count|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Hata, ErrorType, EventSubscriptionName|
|Deliverybaşarılı sayısı|Evet|Teslim edilen olaylar|Count|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|EventSubscriptionName|
|DestinationProcessingDurationInMs|Hayır|Hedef Işleme süresi|Mayacak|Ortalama|Milisaniye cinsinden hedef işleme süresi|EventSubscriptionName|
|DroppedEventCount|Evet|Bırakılan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|DropReason, EventSubscriptionName|
|MatchedEventCount|Evet|Eşleşen olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|EventSubscriptionName|
|PublishFailCount|Evet|Başarısız olayları Yayımla|Count|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|ErrorType, hata|
|Publishbaşarılı sayısı|Evet|Yayımlanan olaylar|Count|Toplam|Bu konuda yayınlanan toplam olay sayısı|Boyut yok|
|Publishbaşarıyla Latencınms|Evet|Yayımlama başarı gecikmesi|Mayacak|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Boyut yok|
|UnmatchedEventCount|Evet|Eşleşmeyen olaylar|Count|Toplam|Bu konu için olay abonelikleriyle eşleşmeyen toplam olay sayısı|Boyut yok|


## <a name="microsofteventgridtopics"></a>Microsoft. EventGrid/konuları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Evet|Kullanılmayan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|DeadLetterReason, EventSubscriptionName|
|Ityattemptfailcount|Hayır|Teslim başarısız olaylar|Count|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Hata, ErrorType, EventSubscriptionName|
|Deliverybaşarılı sayısı|Evet|Teslim edilen olaylar|Count|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|EventSubscriptionName|
|DestinationProcessingDurationInMs|Hayır|Hedef Işleme süresi|Mayacak|Ortalama|Milisaniye cinsinden hedef işleme süresi|EventSubscriptionName|
|DroppedEventCount|Evet|Bırakılan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|DropReason, EventSubscriptionName|
|MatchedEventCount|Evet|Eşleşen olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|EventSubscriptionName|
|PublishFailCount|Evet|Başarısız olayları Yayımla|Count|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|ErrorType, hata|
|Publishbaşarılı sayısı|Evet|Yayımlanan olaylar|Count|Toplam|Bu konuda yayınlanan toplam olay sayısı|Boyut yok|
|Publishbaşarıyla Latencınms|Evet|Yayımlama başarı gecikmesi|Mayacak|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Boyut yok|
|UnmatchedEventCount|Evet|Eşleşmeyen olaylar|Count|Toplam|Bu konu için olay abonelikleriyle eşleşmeyen toplam olay sayısı|Boyut yok|

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleye bakın: [tanılama günlükleri](diagnostic-logs.md)
