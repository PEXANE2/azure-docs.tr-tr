---
title: Hızlı başlangıç-Azure Iletişim hizmetlerini kullanarak telefon numaralarını yönetme
description: Azure Iletişim hizmetlerini kullanarak telefon numaralarını yönetmeyi öğrenin
author: prakulka
manager: nmurav
services: azure-communication-services
ms.author: prakulka
ms.date: 03/10/2021
ms.topic: quickstart
ms.service: azure-communication-services
ms.custom: references_regions
zone_pivot_groups: acs-azp-java-net-python-csharp-js
ms.openlocfilehash: 0f4a461ac5d459c6e3311400785e34bc22f40a00
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "105728462"
---
# <a name="quickstart-manage-phone-numbers"></a>Hızlı başlangıç: telefon numaralarını yönetme

[!INCLUDE [Regional Availability Notice](../../includes/regional-availability-include.md)]

::: zone pivot="platform-azp"
[!INCLUDE [Azure portal](./includes/phone-numbers-portal.md)]
::: zone-end

::: zone pivot="programming-language-csharp"
[!INCLUDE [Azure portal](./includes/phone-numbers-net.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Java](./includes/phone-numbers-java.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Python](./includes/phone-numbers-python.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [JavaScript](./includes/phone-numbers-js.md)]
::: zone-end

## <a name="troubleshooting"></a>Sorun giderme

Yaygın sorular ve sorunlar:

- Satın alma telefonu yalnızca ABD 'de desteklenmektedir. Telefon numaralarını satın almak için şunları doğrulayın:
  - İlişkili Azure aboneliği faturalandırma adresi Birleşik Devletler bulunmaktadır. Şu anda bir kaynağı başka bir aboneliğe taşıyamazsınız.
  - Iletişim Hizmetleri kaynağınız Birleşik Devletler veri konumunda sağlanır. Kaynağı şu anda başka bir veri konumuna taşıyamazsınız.

- Telefon numarası bırakıldığında, telefon numarası serbest bırakılır veya faturalandırma döngüsünün sonuna kadar yeniden kullanılamaz.

- Bir Iletişim Hizmetleri kaynağı silindiğinde, bu kaynakla ilişkilendirilmiş telefon numaraları otomatik olarak aynı anda serbest bırakılır.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunları öğrenirsiniz:

> [!div class="checklist"]
> * Telefon numarası satın alın
> * Telefon numaranızı yönetin
> * Telefon numarası yayınlama

> [!div class="nextstepaction"]
> [SMS gönder](../telephony-sms/send.md) 
>  [Çağırma ile çalışmaya başlayın](../voice-video-calling/getting-started-with-calling.md)
