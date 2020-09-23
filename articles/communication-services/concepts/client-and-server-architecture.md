---
title: İstemci ve sunucu mimarisi
titleSuffix: An Azure Communication Services concept document
description: Iletişim hizmetlerinin mimarisi hakkında bilgi edinin.
author: mikben
manager: mikben
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2020
ms.topic: conceptual
ms.service: azure-communication-services
ms.openlocfilehash: 38d6b131c972b4fd890af53624fdd6a36b3ecb6c
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941210"
---
# <a name="client-and-server-architecture"></a>İstemci ve sunucu mimarisi

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

<!--
> [!WARNING]
> This document is under construction and needs the following items to be addressed: 
> - Need to add security best practices for token management here
> - Reference docs:
> - https://docs.microsoft.com/windows/security/threat-protection/security-policy-settings/create-a-token-object
> - https://docs.microsoft.com/azure/aks/operator-best-practices-identity
> - https://docs.microsoft.com/cloud-app-security/api-tokens?view=gestures-1.0-->

Her Azure Iletişim Hizmetleri uygulaması, kişinin kişiye bağlantısını kolaylaştırmak için **Hizmetleri** kullanan **istemci uygulamalarına** sahip olur. Bu sayfada, yaygın mimari öğeleri çeşitli senaryolarda gösterilmektedir.

## <a name="user-access-management"></a>Kullanıcı erişim yönetimi

Azure Iletişim Hizmetleri istemci kitaplıklarının `user access tokens` Iletişim Hizmetleri kaynaklarına güvenli bir şekilde erişmesi gerekir. `User access tokens` belirtecin duyarlı doğası ve bunları oluşturmak için gereken bağlantı dizesi nedeniyle güvenilir bir hizmet tarafından oluşturulup yönetilmelidir. Erişim belirteçlerini düzgün bir şekilde yönetmemesi, kaynakların kötüye kullanılması nedeniyle ek ücretler oluşmasına neden olabilir. Kullanıcı yönetimi için güvenilen bir hizmetin kullanılması önemle önerilir. Güvenilen hizmet belirteçleri oluşturur ve uygun şifrelemeyi kullanarak onları istemciye geri iletir. Örnek mimari akışı aşağıda bulunabilir:

:::image type="content" source="../media/scenarios/archdiagram-access.png" alt-text="Kullanıcı erişim belirteci mimarisini gösteren diyagram.":::

Daha fazla bilgi için [en iyi kimlik yönetimi uygulamalarını](https://docs.microsoft.com/azure/security/fundamentals/identity-management-best-practices) inceleyin

## <a name="browser-communication"></a>Tarayıcı iletişimi

Azure Communications JavaScript istemci kitaplıkları, Web uygulamalarını zengin metin, ses ve video etkileşimi ile etkinleştirebilir. Uygulama, veri düzlemine erişmek ve gerçek zamanlı metin, ses ve video iletişimi sunmak için istemci kitaplığı aracılığıyla doğrudan Azure Iletişim hizmetleriyle etkileşime girer. Örnek mimari akışı aşağıda bulunabilir:

:::image type="content" source="../media/scenarios/archdiagram-browser.png" alt-text="Iletişim Hizmetleri için tarayıcı mimarisine tarayıcıyı gösteren diyagram.":::

## <a name="native-app-communication"></a>Yerel uygulama iletişimi

Birçok senaryo, yerel uygulamalarla en iyi şekilde sunulur. Azure Iletişim Hizmetleri, hem tarayıcıdan uygulamaya hem de uygulamadan uygulamaya iletişimi destekler.  Yerel bir uygulama deneyimi oluştururken, anında iletme bildirimlerinin olması, kullanıcıların uygulama çalışmadığı zaman bile çağrı almasına imkan tanır. Azure Iletişim Hizmetleri, Google Firebase, Apple Anında İletilen Bildirim Servisi ve Windows anında Iletme bildirimlerine yönelik tümleşik anında iletme bildirimleri sayesinde bunu kolaylaştırır. Örnek mimari akışı aşağıda bulunabilir:

:::image type="content" source="../media/scenarios/archdiagram-app.png" alt-text="Yerel uygulama iletişimi için Iletişim Hizmetleri mimarisini gösteren diyagram.":::

## <a name="voice-and-sms-over-the-public-switched-telephony-network-pstn"></a>Ortak anahtarlı telefon ağı (PSTN) üzerinden sesli ve SMS

Telefon sistemi üzerinden iletişim kurmak, uygulamanızın erişim düzeyini önemli ölçüde artırabilir. Azure Iletişim Hizmetleri, PSTN ses ve SMS senaryolarını desteklemek için doğrudan Azure portal veya REST API 'Leri ile istemci kitaplıklarını kullanarak [telefon numaralarını elde](../quickstarts/telephony-sms/get-phone-number.md) etmenize yardımcı olur. Telefon numaraları alındıktan sonra, hem gelen hem de giden senaryolarda hem PSTN çağırma hem de SMS kullanarak müşterilere ulaşmak için kullanılabilirler. Örnek mimari akışı aşağıda bulunabilir:

> [!Note]
> Genel Önizleme sırasında ABD telefon numaralarının sağlanması ABD ve Kanada 'da fatura adresi bulunan müşteriler tarafından kullanılabilir. 

:::image type="content" source="../media/scenarios/archdiagram-pstn.png" alt-text="Iletişim Hizmetleri PSTN mimarisini gösteren diyagram.":::

PSTN ve SMS çözümleri hakkında daha fazla bilgi için bkz. [PSTN ve SMS çözümünüzü planlayın](../concepts/telephony-sms/plan-solution.md)

## <a name="humans-communicating-with-bots-and-other-services"></a>Botlar ve diğer hizmetlerle iletişim kuran insanlar

Azure Iletişim Hizmetleri, Azure Communication Services veri düzlemine doğrudan erişebilen hizmetlerle, metin ve ses kanalları gibi insandan sisteme iletişimi destekler. Örneğin, bir bot yanıt gelen telefon aramalarından veya Web sohbetine katılımda bulunabilir. Azure Iletişim Hizmetleri, bu senaryoları çağırmak ve sohbet etmek için etkinleştiren istemci kitaplıkları sağlar. Örnek mimari akışı aşağıda bulunabilir:

:::image type="content" source="../media/scenarios/archdiagram-bot.png" alt-text="Iletişim Hizmetleri bot mimarisini gösteren diyagram.":::

## <a name="networking"></a>Ağ

Örneğin, paylaşılan bir karma gerçeklik veya oyun deneyimini senkronize etmek gibi kullanıcılar arasında rastgele veri alışverişi yapmak isteyebilirsiniz. Metin, ses ve video iletişimi için kullanılan gerçek zamanlı veri düzlemi doğrudan iki şekilde kullanılabilir:

- **İstemci kitaplığı çağırma** -bir çağrıda bulunan cihazlar, çağrı kanalı üzerinden veri göndermek ve almak için API 'lere erişim sağlar. Bu, mevcut bir etkileşime veri iletişimleri eklemenin en kolay yoludur.
- **Stun/** açık-Azure iletişim hizmetleri, standartlara uygun bir şekilde hizmet verebilir ve bu hizmetleri kullanabilmenizi sağlar. Bu, standartlaştırılmış temel elemanlar üzerinde yoğun şekilde özelleştirilmiş bir aktarım katmanı oluşturmanıza olanak sağlar. Standart uyumlu istemcinizi yazabilir veya [Wınrtc](https://github.com/microsoft/winrtc)gibi açık kaynak kitaplıklarını kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kullanıcı erişim belirteçleri oluşturma](../quickstarts/access-tokens.md)

Daha fazla bilgi için aşağıdaki makaleleri inceleyin:

- [Kimlik doğrulaması](../concepts/authentication.md) hakkında bilgi edinin
- [PSTN ve SMS çözümleri](../concepts/telephony-sms/plan-solution.md) hakkında bilgi edinin

- [Uygulamanıza sohbet ekleyin](../quickstarts/chat/get-started.md)
- [Uygulamanıza sesli çağrı ekleme](../quickstarts/voice-video-calling/getting-started-with-calling.md)
