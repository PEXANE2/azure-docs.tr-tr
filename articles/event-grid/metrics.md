---
title: Azure Event Grid tarafından desteklenen ölçümler
description: Bu makalede, Azure Event Grid hizmeti tarafından desteklenen Azure Izleyici ölçümleri sunulmaktadır.
ms.topic: conceptual
ms.date: 03/17/2021
ms.openlocfilehash: 321e318f9dab87fde20b33a6a3a906b020ada622
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "104588741"
---
# <a name="metrics-supported-by-azure-event-grid"></a>Azure Event Grid tarafından desteklenen ölçümler
Bu makalede, ad alanlarına göre kategorilere ayrılan Event Grid ölçümleri listelenmiştir. 

## <a name="system-topics"></a>Sistem konu başlıkları

|Metric|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Description|Boyutlar|
|---|---|---|---|---|---|---|
|AdvancedFilterEvaluationCount|Yes|Gelişmiş Filtre değerlendirmeleri|Count|Toplam|Bu konu için olay abonelikleri genelinde değerlendirilen gelişmiş filtrelerin toplam sayısı.|EventSubscriptionName|
|DeadLetteredCount|Yes|Kullanılmayan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|DeadLetterReason, EventSubscriptionName|
|Ityattemptfailcount|No|Teslim başarısız olaylar|Count|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Hata, ErrorType, EventSubscriptionName|
|Deliverybaşarılı sayısı|Yes|Teslim edilen olaylar|Count|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|EventSubscriptionName|
|DestinationProcessingDurationInMs|No|Hedef Işleme süresi|Mayacak|Ortalama|Milisaniye cinsinden hedef işleme süresi|EventSubscriptionName|
|DroppedEventCount|Yes|Bırakılan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|DropReason, EventSubscriptionName|
|MatchedEventCount|Yes|Eşleşen olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|EventSubscriptionName|
|PublishFailCount|Yes|Başarısız olayları Yayımla|Count|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|ErrorType, hata|
|Publishbaşarılı sayısı|Yes|Yayımlanan olaylar|Count|Toplam|Bu konuda yayınlanan toplam olay sayısı|Boyut yok|
|Publishbaşarıyla Latencınms|Yes|Yayımlama başarı gecikmesi|Mayacak|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Boyut yok|
|UnmatchedEventCount|Yes|Eşleşmeyen olaylar|Count|Toplam|Bu konu için olay abonelikleriyle eşleşmeyen toplam olay sayısı|Boyut yok|


## <a name="custom-topics"></a>Özel konu başlıkları

|Metric|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Description|Boyutlar|
|---|---|---|---|---|---|---|
|AdvancedFilterEvaluationCount|Yes|Gelişmiş Filtre değerlendirmeleri|Count|Toplam|Bu konu için olay abonelikleri genelinde değerlendirilen gelişmiş filtrelerin toplam sayısı.|EventSubscriptionName|
|DeadLetteredCount|Yes|Kullanılmayan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|DeadLetterReason, EventSubscriptionName|
|Ityattemptfailcount|No|Teslim başarısız olaylar|Count|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Hata, ErrorType, EventSubscriptionName|
|Deliverybaşarılı sayısı|Yes|Teslim edilen olaylar|Count|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|EventSubscriptionName|
|DestinationProcessingDurationInMs|No|Hedef Işleme süresi|Mayacak|Ortalama|Milisaniye cinsinden hedef işleme süresi|EventSubscriptionName|
|DroppedEventCount|Yes|Bırakılan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|DropReason, EventSubscriptionName|
|MatchedEventCount|Yes|Eşleşen olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|EventSubscriptionName|
|PublishFailCount|Yes|Başarısız olayları Yayımla|Count|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|ErrorType, hata|
|Publishbaşarılı sayısı|Yes|Yayımlanan olaylar|Count|Toplam|Bu konuda yayınlanan toplam olay sayısı|Boyut yok|
|Publishbaşarıyla Latencınms|Yes|Yayımlama başarı gecikmesi|Mayacak|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Boyut yok|
|UnmatchedEventCount|Yes|Eşleşmeyen olaylar|Count|Toplam|Bu konu için olay abonelikleriyle eşleşmeyen toplam olay sayısı|Boyut yok|

## <a name="domains"></a>Etki Alanları

|Metric|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Description|Boyutlar|
|---|---|---|---|---|---|---|
|AdvancedFilterEvaluationCount|Yes|Gelişmiş Filtre değerlendirmeleri|Count|Toplam|Bu konu için olay abonelikleri genelinde değerlendirilen gelişmiş filtrelerin toplam sayısı.|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName|
|DeadLetteredCount|Yes|Kullanılmayan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName, DeadLetterReason|
|Ityattemptfailcount|No|Teslim başarısız olaylar|Count|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName, hata, ErrorType|
|Deliverybaşarılı sayısı|Yes|Teslim edilen olaylar|Count|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName|
|DestinationProcessingDurationInMs|No|Hedef Işleme süresi|Mayacak|Ortalama|Milisaniye cinsinden hedef işleme süresi|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName|
|DroppedEventCount|Yes|Bırakılan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName, DropReason|
|MatchedEventCount|Yes|Eşleşen olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName|
|PublishFailCount|Yes|Başarısız olayları Yayımla|Count|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|Konu, ErrorType, hata|
|Publishbaşarılı sayısı|Yes|Yayımlanan olaylar|Count|Toplam|Bu konuda yayınlanan toplam olay sayısı|Konu|
|Publishbaşarıyla Latencınms|Yes|Yayımlama başarı gecikmesi|Mayacak|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Boyut yok|

## <a name="event-subscriptions"></a>Olay abonelikleri

|Metric|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Description|Boyutlar|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Yes|Kullanılmayan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|DeadLetterReason|
|Ityattemptfailcount|No|Teslim başarısız olaylar|Count|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Hata, ErrorType|
|Deliverybaşarılı sayısı|Yes|Teslim edilen olaylar|Count|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|Boyut yok|
|DestinationProcessingDurationInMs|No|Hedef Işleme süresi|Mayacak|Ortalama|Milisaniye cinsinden hedef işleme süresi|Boyut yok|
|DroppedEventCount|Yes|Bırakılan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|DropReason|
|MatchedEventCount|Yes|Eşleşen olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|Boyut yok|


## <a name="extension-topics"></a>Uzantı konuları

|Metric|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Description|Boyutlar|
|---|---|---|---|---|---|---|
|PublishFailCount|Yes|Başarısız olayları Yayımla|Count|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|ErrorType, hata|
|Publishbaşarılı sayısı|Yes|Yayımlanan olaylar|Count|Toplam|Bu konuda yayınlanan toplam olay sayısı|Boyut yok|
|Publishbaşarıyla Latencınms|Yes|Yayımlama başarı gecikmesi|Mayacak|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Boyut yok|
|UnmatchedEventCount|Yes|Eşleşmeyen olaylar|Count|Toplam|Bu konu için olay abonelikleriyle eşleşmeyen toplam olay sayısı|Boyut yok|

## <a name="partner-namespaces"></a>İş ortağı ad alanları

|Metric|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Description|Boyutlar|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Yes|Kullanılmayan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|DeadLetterReason, EventSubscriptionName|
|Ityattemptfailcount|No|Teslim başarısız olaylar|Count|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Hata, ErrorType, EventSubscriptionName|
|Deliverybaşarılı sayısı|Yes|Teslim edilen olaylar|Count|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|EventSubscriptionName|
|DestinationProcessingDurationInMs|No|Hedef Işleme süresi|Mayacak|Ortalama|Milisaniye cinsinden hedef işleme süresi|EventSubscriptionName|
|DroppedEventCount|Yes|Bırakılan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|DropReason, EventSubscriptionName|
|MatchedEventCount|Yes|Eşleşen olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|EventSubscriptionName|
|PublishFailCount|Yes|Başarısız olayları Yayımla|Count|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|ErrorType, hata|
|Publishbaşarılı sayısı|Yes|Yayımlanan olaylar|Count|Toplam|Bu konuda yayınlanan toplam olay sayısı|Boyut yok|
|Publishbaşarıyla Latencınms|Yes|Yayımlama başarı gecikmesi|Mayacak|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Boyut yok|
|UnmatchedEventCount|Yes|Eşleşmeyen olaylar|Count|Toplam|Bu konu için olay abonelikleriyle eşleşmeyen toplam olay sayısı|Boyut yok|


## <a name="partner-topics"></a>İş ortağı konuları

|Metric|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Description|Boyutlar|
|---|---|---|---|---|---|---|
|AdvancedFilterEvaluationCount|Yes|Gelişmiş Filtre değerlendirmeleri|Count|Toplam|Bu konu için olay abonelikleri genelinde değerlendirilen gelişmiş filtrelerin toplam sayısı.|EventSubscriptionName|
|DeadLetteredCount|Yes|Kullanılmayan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|DeadLetterReason, EventSubscriptionName|
|Ityattemptfailcount|No|Teslim başarısız olaylar|Count|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Hata, ErrorType, EventSubscriptionName|
|Deliverybaşarılı sayısı|Yes|Teslim edilen olaylar|Count|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|EventSubscriptionName|
|DestinationProcessingDurationInMs|No|Hedef Işleme süresi|Mayacak|Ortalama|Milisaniye cinsinden hedef işleme süresi|EventSubscriptionName|
|DroppedEventCount|Yes|Bırakılan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|DropReason, EventSubscriptionName|
|MatchedEventCount|Yes|Eşleşen olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|EventSubscriptionName|
|PublishFailCount|Yes|Başarısız olayları Yayımla|Count|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|ErrorType, hata|
|Publishbaşarılı sayısı|Yes|Yayımlanan olaylar|Count|Toplam|Bu konuda yayınlanan toplam olay sayısı|Boyut yok|
|UnmatchedEventCount|Yes|Eşleşmeyen olaylar|Count|Toplam|Bu konu için olay abonelikleriyle eşleşmeyen toplam olay sayısı|Boyut yok|


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleye bakın: [tanılama günlükleri](diagnostic-logs.md)
