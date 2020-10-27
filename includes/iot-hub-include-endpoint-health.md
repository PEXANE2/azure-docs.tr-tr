---
title: include dosyası
description: include dosyası
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/28/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 36c05badb3b2292a29b8227c7f03b841474c97ad
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92548197"
---
Uç noktaların sistem durumunu almak için [uç nokta durumu al](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) REST API kullanabilirsiniz. Uç nokta bu durumlardan birinde olduğunda gecikme süresi bekleniyorsa, uç nokta durumu ölü veya sağlıksız olduğunda hataları belirlemek ve hata ayıklamak için ileti gecikmesini yönlendirme ile ilgili [IoT Hub yönlendirme ölçümlerini](../articles/iot-hub/monitor-iot-hub-reference.md#routing-metrics) kullanmanızı öneririz. IoT Hub ölçümlerini kullanma hakkında daha fazla bilgi için bkz. [Monitor IoT Hub](../articles/iot-hub/monitor-iot-hub.md).

|Sistem Durumu|Açıklama|
|---|---|
|sağlıklı|Uç nokta iletileri beklendiği gibi kabul ediyor.|
|sağlıksız|Uç nokta iletileri kabul etmiyor ve IoT Hub bu uç noktaya ileti gönderimi yeniden deniyor.|
|bilinmeyen|IoT Hub bu uç noktaya ileti teslim etmeyi denemedi.|
|düzey|Uç nokta, beklenenden yavaş iletileri kabul ediyor veya sağlıksız bir durumdan kurtarıyor.|
|teslim edilemeyen iletiler|IoT Hub artık bu uç noktaya ileti teslim ediyor. Bu uç noktaya ileti gönderme denemeleri başarısız oldu.|
