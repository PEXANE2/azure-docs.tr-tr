---
title: Azure İletişim Hizmetleri nedir?
description: Azure Iletişim hizmetlerinin gerçek zamanlı iletişimlerle zengin kullanıcı deneyimleri geliştirmenize nasıl yardımcı olduğunu öğrenin.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 40dacdf243505447e9831239ef1252602a3e0f99
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "103491699"
---
# <a name="what-is-azure-communication-services"></a>Azure İletişim Hizmetleri nedir?

[!INCLUDE [Public Preview Notice](./includes/public-preview-include.md)]

> [!IMPORTANT]
> Azure Iletişim Hizmetleri 'ni kullanarak oluşturduğunuz uygulamalar Microsoft ekipleriyle iletişim kurabilir. Daha fazla bilgi edinmek için [takımlar birlikte çalışma](./quickstarts/voice-video-calling/get-started-teams-interop.md) belgelerimizi ziyaret edin.


Azure Iletişim Hizmetleri, uygulamalarınıza kolay bir şekilde gerçek zamanlı multimedya sesi, video ve telefon üzerinden iletişim özellikleri eklemenizi sağlar. Iletişim Hizmetleri istemci kitaplıkları, iletişim çözümlerinizi sohbet ve SMS işlevselliği eklemenize de olanak tanır.

<br>

> [!VIDEO https://www.youtube.com/embed/apBX7ASurgM]

<br>
<br>

Ses, video, metin ve veri iletişimleri için Iletişim hizmetlerini çeşitli senaryolarda kullanabilirsiniz:

- Tarayıcıdan tarayıcıya, tarayıcıdan uygulamaya ve uygulamadan uygulamaya iletişim
- Botlar veya diğer hizmetlerle etkileşim kuran kullanıcılar
- Ortak anahtarlı telefon ağı üzerinden etkileşim kuran kullanıcılar ve botlar

Karışık senaryolar desteklenir. Örneğin, bir Iletişim Hizmetleri uygulaması kullanıcıların tarayıcılarından ve geleneksel telefon aygıtlarından aynı anda konuşuyor olabilir. İletişim Hizmetleri ayrıca, bot tabanlı etkileşimli sesli yanıt (ıVR) sistemleri oluşturmak için Azure bot hizmeti ile birleştirilebilir.

## <a name="common-scenarios"></a>Genel senaryolar

Azure Iletişim Hizmetleri ile yeni başladıysanız, aşağıdaki kaynaklar başlamak için harika bir yerdir:
<br>

| Kaynak                               |Açıklama                           |
|---                                    |---                                   |
|**[Arama uygulamanızla bir Teams toplantısına katılma](./quickstarts/voice-video-calling/get-started-teams-interop.md)**|Azure Iletişim Hizmetleri, Microsoft ekipleriyle etkileşime geçen özel toplantı deneyimleri oluşturmak için kullanılabilir. Iletişim Hizmetleri çözümünüzün kullanıcıları ses, video, sohbet ve ekran paylaşımı üzerinde takımlar katılımcıları ile etkileşime geçebilir.|
|**[İletişim Hizmetleri kaynağı oluşturma](./quickstarts/create-communication-resource.md)**|İlk Iletişim Hizmetleri kaynağınızı sağlamak için Azure portal veya Iletişim Hizmetleri istemci kitaplığını kullanarak Azure Iletişim hizmetlerini kullanmaya başlayabilirsiniz. Iletişim Hizmetleri kaynak bağlantı dizeniz olduktan sonra, ilk Kullanıcı erişim belirteçlerinizi sağlayabilirsiniz.|
|**[İlk Kullanıcı erişim belirtecinizi oluşturma](./quickstarts/access-tokens.md)**|Kullanıcı erişim belirteçleri, hizmetlerinizin kimlik doğrulaması için Azure Iletişim Hizmetleri kaynağına karşı kullanılır. Bu belirteçler, Iletişim Hizmetleri istemci kitaplığı kullanılarak sağlanır ve yeniden yayımlandı.|
|**[Telefon numarası alın](./quickstarts/telephony-sms/get-phone-number.md)**|Azure Iletişim hizmetlerini kullanarak telefon numaralarını temin edebilir ve serbest bırakabilirsiniz. Bu telefon numaraları, giden çağrıları başlatmak ve SMS iletişim çözümleri oluşturmak için kullanılabilir.|
|**[Uygulamanızdan SMS gönderin](./quickstarts/telephony-sms/send.md)**|Azure Iletişim Hizmetleri SMS istemci kitaplığı, .NET ve JavaScript uygulamalarınızdan SMS mesajları göndermenizi ve almanızı sağlar.|
|**[Ses ve video çağırma ile çalışmaya başlama](./quickstarts/voice-video-calling/getting-started-with-calling.md)**| Azure Iletişim Hizmetleri, çağıran istemci kitaplığını kullanarak uygulamalarınıza ses ve video çağrısı eklemenize olanak tanır. Bu kitaplık WebRTC tarafından desteklenmektedir ve uygulamalarınızda eşler arası, çoklu ortam, gerçek zamanlı iletişim kurmanızı sağlar.|
|**[Sohbetle çalışmaya başlama](./quickstarts/chat/get-started.md)**|Azure Iletişim Hizmetleri sohbeti istemci kitaplığı, gerçek zamanlı sohbeti uygulamalarınıza bütünleştirmek için kullanılabilir.|


## <a name="samples"></a>Örnekler

Aşağıdaki örneklerde, Azure Iletişim Hizmetleri istemci kitaplıklarının uçtan uca kullanımı gösterilmektedir. Bu örnekleri kullanarak kendi Iletişim Hizmetleri çözümlerinizi önyükleyebilirsiniz.
<br>

| Örnek adı                               | Description                           |
|---                                    |---                                   |
|**[Hero örneğini çağıran Grup](./samples/calling-hero-sample.md)**|Iletişim Hizmetleri istemci kitaplıklarının bir grup çağırma deneyimi oluşturmak için nasıl kullanılabileceğini öğrenin.|
|**[Grup sohbeti Hero örneği](./samples/chat-hero-sample.md)**|Bkz. Iletişim Hizmetleri istemci kitaplıklarının grup sohbeti deneyimi oluşturmak için nasıl kullanılabileceğini öğrenin.|


## <a name="platforms-and-client-libraries"></a>Platformlar ve istemci kitaplıkları

Aşağıdaki kaynaklar, Azure Communication Services istemci kitaplıkları hakkında bilgi edinmenize yardımcı olur:

| Kaynak                               | Açıklama                           |
|---                                    |---                                   |
|**[İstemci kitaplıkları ve REST API’leri](./concepts/sdk-options.md)**|Azure Iletişim Hizmetleri Özellikleri, her biri bir istemci kitaplığı tarafından temsil edilen altı alana göre düzenlenmiştir. Gerçek zamanlı iletişim gereksinimlerinize göre hangi istemci kitaplıklarının kullanılacağına karar verebilirsiniz.|
|**[Arama istemci kitaplığına genel bakış](./concepts/voice-video-calling/calling-sdk-features.md)**|İstemci kitaplığına genel bakış ' ı çağıran Iletişim hizmetlerini gözden geçirin.|
|**[Sohbet istemci kitaplığına genel bakış](./concepts/chat/sdk-features.md)**|Iletişim Hizmetleri sohbeti istemci kitaplığına genel bakış ' a bakın.|
|**[SMS istemci kitaplığına genel bakış](./concepts/telephony-sms/sdk-features.md)**|Iletişim Hizmetleri SMS istemci kitaplığına genel bakış ' ı inceleyin.|

## <a name="compare-azure-communication-services"></a>Azure Iletişim hizmetlerini karşılaştırın

Şu anda Iletişim hizmetleriyle doğrudan birlikte çalışabilen, kullanmayı düşünebileceğiniz iki farklı Microsoft iletişim ürünü vardır:

 - [Microsoft Graph bulut Iletişimi API 'leri](/graph/cloud-communications-concept-overview) , kuruluşların M365 lisanslarıyla Azure Active Directory kullanıcılara bağlı iletişim deneyimleri oluşturmasına olanak tanır. Bu, Azure Active Directory bağlı olan veya Microsoft ekiplerinde üretkenlik deneyimlerini uzatmak istediğiniz durumlarda idealdir. Ayrıca [takımlar deneyimi](/microsoftteams/platform/?preserve-view=true&view=msteams-client-js-latest) içinde uygulama ve özelleştirme oluşturmak Için API 'ler vardır.

 - [Azure PlayFab partisi](/gaming/playfab/features/multiplayer/networking/) , oyunlara düşük gecikmeli sohbet ve veri iletişimi eklemeyi basitleştirir. Sohbet ve ağ sistemlerini Iletişim hizmetleriyle oyun oynamak için PlayFab, Xbox 'ta hazırlanmış ve ücretsiz bir seçenektir.


## <a name="next-steps"></a>Sonraki Adımlar

 - [İletişim Hizmetleri kaynağı oluşturma](./quickstarts/create-communication-resource.md)
