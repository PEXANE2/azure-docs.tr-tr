---
title: dosya dahil etme
description: dosya dahil etme
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/28/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: a0d1de622eefad4ae5e55a427f8b0b1bf4360c0a
ms.sourcegitcommit: 6571e34e609785e82751f0b34f6237686470c1f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84792107"
---
Uç noktaların sistem durumunu almak için [uç nokta durumu al](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) REST API kullanabilirsiniz. Uç nokta bu durumlardan birinde olduğunda gecikme süresinin daha yüksek olması Beklendiğimiz için, uç nokta durumu ölü veya sağlıksız olduğunda hataları tanımlamak ve hatalarını ayıklamak için ileti gecikmesini yönlendirme ile ilgili [IoT Hub ölçümlerini](../articles/iot-hub/iot-hub-metrics.md) kullanmanızı öneririz.


|Sistem Durumu|Description|
|---|---|
|sağlıklı|Uç nokta iletileri beklendiği gibi kabul ediyor.|
|sağlıksız|Uç nokta iletileri kabul etmiyor ve IoT Hub bu uç noktaya ileti gönderimi yeniden deniyor.|
|bilinmeyen|IoT Hub bu uç noktaya ileti teslim etmeyi denemedi.|
|düzey|Uç nokta, beklenenden yavaş iletileri kabul ediyor veya sağlıksız bir durumdan kurtarıyor.|
|teslim edilemeyen iletiler|IoT Hub artık bu uç noktaya ileti teslim ediyor. Bu uç noktaya ileti gönderme denemeleri başarısız oldu.|
