---
title: dosya dahil etme
description: dosya dahil etme
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: d03579f704879bd8d012bb0bb326659d1f778dee
ms.sourcegitcommit: 6571e34e609785e82751f0b34f6237686470c1f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84793334"
---
REST API [uç nokta durumu Al](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) , uç noktaların sistem durumunu ve bilinen son hatayı, bir uç noktanın sağlıklı olma nedenini belirlemek için verir. Aşağıdaki tabloda en yaygın hatalar listelenmektedir.

|Bilinen son hata|Açıklama/gerçekleştiği zaman|Olası risk azaltma|
|-----|-----|-----|
|Larsa|Geçici bir hata oluştu ve IoT Hub işlemi yeniden deneyecek.|Rota [tanılama günlüklerini](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitor-resource-health#routes)gözlemleyin.|
|InternalError|Bir uç noktaya ileti teslim edilirken bir hata oluştu.|Bu bir iç özel durumdur, ancak yolların [tanılama günlüklerini](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitor-resource-health#routes)de gözlemleyin.|
|Yetkisiz|IoT Hub belirtilen uç noktaya ileti gönderme yetkisi yok.|Uç nokta için bağlantı dizesinin güncel olduğunu doğrulayın. Değiştirildiyse, IoT Hub bir güncelleştirmeyi göz önünde bulundurun. Uç nokta yönetilen kimlik kullanıyorsa, IoT Hub sorumlunun hedefte gerekli izinlere sahip olup olmadığını denetleyin.|
|Sürecek|Uç noktalara ileti yazılırken IoT Hub kısıtlanıyor.|Etkilenen uç nokta için azaltma sınırlarını gözden geçirin. Gerekirse ölçeği genişletmek için uç nokta yapılandırmasını değiştirin.|
|Zaman aşımı|İşlem zaman aşımı.|İşlemi yeniden deneyin.|
|Bulunamadı|Hedef kaynak yok.|Hedef kaynağın mevcut olduğundan emin olun.|
|Kapsayıcı bulunamadı|Depolama kapsayıcısı yok.|Depolama kapsayıcısının mevcut olduğundan emin olun.|
|Kapsayıcı devre dışı|Depolama kapsayıcısı devre dışı.|Depolama kapsayıcısının etkinleştirildiğinden emin olun.|
|Maxiletiizeaşılmıştır|İleti yönlendirmesinde 256 KB 'lik bir ileti boyutu sınırı vardır. yönlendirilmekte olan ileti boyutu bu sınırı aştı.|İleti boyutunun daha az uygulama özelliği veya daha az ileti zenginleştirme kullanılarak küçültüleceğinden emin olun.|
|PartitioningAndDuplicateDetectionNotSupported|Service Bus 'da yinelenen algılama etkinleştirilmemiş olabilir.|Yinelenen algılamayı Service Bus devre dışı bırakın veya yinelenen algılama olmadan bir varlık kullanmayı düşünün.|
|SessionfulEntityNotSupported|Service Bus 'da oturum etkinleştirilmemiş olabilir.|Oturumu Service Bus devre dışı bırakın veya oturum içermeyen bir varlık kullanmayı deneyin.|
|NoMatchingSubscriptionsForMessage|Service Bus konusunda ileti yazılacak abonelik yok.|IoT Hub iletilerin yönlendirileceği bir abonelik oluşturun.|
|Endpointexüçlü Allydisabled|Uç nokta etkin bir durumda olmadığından IoT Hub ileti gönderebilirler.|Uç noktayı etkin duruma geri getirmek için etkinleştirin.|
|DeviceMaximumQueueDepthExceeded|Service Bus boyut sınırına ulaşıldı.|Yeni iletilerin Event Hubs belirtilmesine izin vermek için hedef Event Hubs iletileri kaldırmayı göz önünde bulundurun.|