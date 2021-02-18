---
title: Azure Iletişim hizmetlerinde ses ve video kavramları
titleSuffix: An Azure Communication Services concept document
description: Iletişim Hizmetleri çağrı türleri hakkında bilgi edinin.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 09/30/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 98c1ce9367e9bf2aa1a381a772b3cbc73f96b519
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "100650958"
---
# <a name="voice-and-video-concepts"></a>Ses ve görüntü kavramları

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

Azure Iletişim Hizmetleri 'ni kullanarak bir veya daha fazla ses ve video çağrısı yapabilirsiniz. Çağrılarınız, Internet 'e bağlı diğer cihazlara ve eski telefonlar için yapılabilir. Kullanıcılarınızın özel konuşmalardaki veya grup tartışmalarında bir diğerine konuşmasını sağlayan uygulamalar oluşturmak için Iletişim Hizmetleri JavaScript, Android veya iOS istemci kitaplıklarını kullanabilirsiniz. Azure Iletişim Hizmetleri, hizmetlere veya botlara yapılan çağrıları destekler.

## <a name="call-types-in-azure-communication-services"></a>Azure Iletişim hizmetlerinde çağrı türleri

Azure Iletişim hizmetlerinde yapabileceğiniz birden çok tür çağrı vardır. Yaptığınız çağrıların türü, sinyal şemanızı, medya trafiği akışlarınızı ve fiyatlandırma modelinizi belirlemektir.

### <a name="voice-over-ip-voip"></a>IP üzerinden ses (VoIP) 

Uygulamanızın bir kullanıcısı, bir internet veya veri bağlantısı üzerinden uygulamanızın başka bir kullanıcısını çağırdığında, çağrı IP üzerinden (VoIP) ses üzerinden yapılır. Bu durumda, hem sinyal hem de medya internet üzerinden akar.

### <a name="public-switched-telephone-network-pstn"></a>Ortak anahtarlı telefon ağı (PSTN)

Kullanıcılarınızın geleneksel telefon numarası ile her etkileşim kurması durumunda, aramalar PSTN (ortak anahtarlı telefon ağı) sesli arama tarafından kolaylaştırılmış. PSTN çağrıları yapmak ve almak için, Azure Communication Services kaynağınız için telefon özellikleri eklemeniz gerekir. Bu durumda, sinyal ve medya, kullanıcılarınızı bağlamak için IP tabanlı ve PSTN tabanlı teknolojilerin birleşimini kullanır.

### <a name="one-to-one-call"></a>Bire bir çağrı

Azure Iletişim hizmetlerinde bir bire bir çağrı, kullanıcılarınızın istemci kitaplıklarımızdan birini kullanarak başka bir kullanıcıya bağlanmasından oluşur. Çağrı, VoIP veya PSTN olabilir.

### <a name="group-call"></a>Grup çağrısı

Azure Iletişim hizmetlerinde bir grup çağrısı, üç veya daha fazla katılımcı birbirine bağlandığında oluşur. VoIP ve PSTN bağlantılı kullanıcıların herhangi bir birleşimi bir grup çağrısında bulunabilir. Bire bir çağrı, çağrıya daha fazla katılımcı eklenerek bir grup çağrısına dönüştürülebilir. Bu katılımcılardan biri bir bot olabilir.

### <a name="supported-video-standards"></a>Desteklenen video standartları
H. ula 'yi destekliyoruz (MPEG-4) 

### <a name="video-quality"></a>Video kalitesi 
Yerel (iOS, Android) SDK 'larda tam HD 1080p ' i destekliyoruz. Web (JS) SDK 'Sı için standart HD 720p 'yi destekliyoruz. Kalite, kullanılabilir bant genişliğine bağlıdır.  

### <a name="rooms-concept"></a>Oda kavramı
Odalar, Web sitenize veya yerel uygulamanıza ses, video, ekran paylaşımı, PSTN ve SMS etkileşimlerini kolayca eklemenize olanak tanıyan bir API ve SDK kümesidir.
Önizleme sırasında, aynı konuşmaya katmak için Grup KIMLIĞINI kullanabilirsiniz. İhtiyaç duyduğunuz sayıda grup kimliği oluşturabilir ve kullanıcıları "Odalar" ile ayırabilirsiniz. İleriye doğru taşımak "Odalar" etrafında daha fazla denetim sağlar 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Çağırma ile çalışmaya başlayın](../../quickstarts/voice-video-calling/getting-started-with-calling.md)

Daha fazla bilgi için aşağıdaki makaleleri inceleyin:
- Genel [çağrı akışları](../call-flows.md) hakkında bilgi edinin
- [Telefon numarası türleri](../telephony-sms/plan-solution.md)
- [Çağıran istemci kitaplığı özellikleri](../voice-video-calling/calling-sdk-features.md) hakkında bilgi edinin
