---
title: Hızlı başlangıç-SMS iletisi gönderme
titleSuffix: An Azure Communication Services quickstart
description: Azure Iletişim hizmetlerini kullanarak SMS iletisi gönderme hakkında bilgi edinin.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 09/30/2020
ms.topic: overview
ms.service: azure-communication-services
ms.custom: tracking-python, devx-track-js
zone_pivot_groups: acs-js-csharp-java-python
ms.openlocfilehash: 52cec861ba81a804081c4b879ab73333a3281e61
ms.sourcegitcommit: 6a902230296a78da21fbc68c365698709c579093
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93360301"
---
# <a name="quickstart-send-an-sms-message"></a>Hızlı başlangıç: SMS iletisi gönder

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

::: zone pivot="programming-language-csharp"
[!INCLUDE [Send SMS with .NET client library](./includes/send-sms-net.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [Send SMS with JavaScript client library](./includes/send-sms-js.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Send SMS with Python client library](./includes/send-sms-python.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Send SMS with Java client library](./includes/send-sms-java.md)]
::: zone-end

## <a name="troubleshooting"></a>Sorun giderme

SMS tesliminde ilgili sorunları gidermek için, teslim ayrıntılarını yakalamak üzere [Event Grid ile teslim raporlamayı etkinleştirebilirsiniz](./handle-sms-events.md) .

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir Iletişim Hizmetleri aboneliğini temizleyip kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler. [Kaynakları Temizleme](../create-communication-resource.md#clean-up-resources)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Iletişim hizmetlerini kullanarak SMS iletileri gönderme hakkında daha fazla öğrendiniz.

> [!div class="nextstepaction"]
> [SMS olaylarına abone olma](./handle-sms-events.md)

> [!div class="nextstepaction"]
> [PSTN çözümünüzü planlayın](../../concepts/telephony-sms/plan-solution.md)

> [!div class="nextstepaction"]
> [SMS hakkında daha fazla bilgi](../../concepts/telephony-sms/concepts.md)