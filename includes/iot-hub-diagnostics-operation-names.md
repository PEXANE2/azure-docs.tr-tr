---
title: dosya dahil etme
description: dosya dahil etme
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 04/28/2020
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 362c13771e7382ead1ba5aebd99a69fd86cd718c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84793320"
---
<!-- operation names for the diag logs for IoT Hub -->

|İşlem adı|Düzey|Açıklama|
|------------- |-----|-----------|
|UndefinedRouteEvaluation|Bilgi|İleti, bir verme koşuluyla değerlendirilemiyor. Örneğin, iletide yol sorgusu koşulundaki bir özellik yoksa. [Yönlendirme sorgusu söz dizimi](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax)hakkında daha fazla bilgi edinin.|
|RouteEvaluationError|Hata|İleti biçimindeki bir sorun nedeniyle ileti değerlendirilirken bir hata oluştu. Örneğin, içerik kodlaması belirtilmemişse veya Içerik türü iletide geçerli değilse, bu hata günlüğe kaydedilir. Bunların [Sistem özelliklerinde](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax#system-properties)ayarlanması gerekir.|
|DroppedMessage|Hata|İleti bırakıldı ve yönlendirilmedi. Bu, ileti herhangi bir yönlendirme sorgusuyla eşleşmediği veya uç nokta ölü ve birkaç yeniden denemeden sonra iletilemeyen bir nedenden kaynaklanıyor olabilir. Uç nokta [alma durumunu](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth)REST API kullanarak uç nokta hakkında daha fazla ayrıntı almanızı öneririz.|
|EndpointUnhealthy|Hata|Uç nokta IoT Hub iletileri kabul etmiyor ve IoT Hub iletileri yeniden göndermeye çalışıyor. [Uç nokta durumu al](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth)REST API aracılığıyla bilinen son hatayı gözlemleyerek öneririz.|
|Endpointölü|Hata|Uç nokta, bir saat boyunca IoT Hub iletileri kabul etmiyor. [Uç nokta durumu al](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth)REST API aracılığıyla bilinen son hatayı gözlemleyerek öneririz.|
|EndpointHealthy|Bilgi|Uç nokta sağlıklı ve IoT Hub iletileri alıyor. Bu ileti sürekli olarak günlüğe kaydedilmez, ancak yalnızca uç nokta yeniden sağlıklı hale geldiğinde günlüğe kaydedilir. Bu ileti, IoT Hub uç noktaya ileti gönderemediği anlamına gelir, ancak uç nokta artık sağlıklı.|
|OrphanedMessage|Bilgi|İleti herhangi bir rota ile eşleşmiyor.|
|Invalidmessage|Hata|Uç nokta ile uyumsuzluk nedeniyle ileti geçersiz. Uç noktanın yapılandırmalarının kontrol edilmesini öneririz.|


*Undefinedrouteevaluation*, *RouteEvaluationError* ve *orphanedmessage* işlemleri, IoT Hub başına dakikada bir en çok bir kez azaltılır ve günlüğe kaydedilir.