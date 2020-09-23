---
title: Azure Iletişim hizmetlerinde ses ve video kavramları
titleSuffix: An Azure Communication Services concept document
description: Iletişim Hizmetleri çağrı türleri hakkında bilgi edinin.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 932b9f84e9f17555f364ae504967678ee1d7ce97
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90947915"
---
# <a name="voice-and-video-concepts"></a>Ses ve video kavramları

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

Azure Iletişim hizmetlerini kullanarak ses ve video çağrıları yapabilir, çağrı meta verilerini çözümleyebilir, çağrıları kaydedebilir ve hatta çağrı ağaçları oluşturabilirsiniz. Çağrılarınız, Internet 'e bağlı diğer cihazlara ve eski telefonlar için yapılabilir. Kullanıcılarınızın özel konuşmalardaki veya grup tartışmalarında bir diğerine konuşmasını sağlayan uygulamalar oluşturmak için Iletişim Hizmetleri JavaScript, Android veya iOS istemci kitaplıklarını kullanabilirsiniz. Azure Iletişim Hizmetleri, hizmetlere veya botlara yapılan çağrıları destekler.

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
Odalar, Web sitenize veya yerel uygulamanıza ses, video, ekran paylaşımı, PSTN ve SMS etkileşimlerini kolayca eklemenizi sağlayan bir API ve SDK kümesidir.
Önizleme sırasında, aynı konuşmaya katmak için Grup KIMLIĞINI kullanabilirsiniz. İhtiyaç duyduğunuz kadar çok Grup KIMLIĞI oluşturabilir ve kullanıcıları "Odalar" ile ayırabilirsiniz. İleriye doğru taşımak "Odalar" etrafında daha fazla denetim sağlar 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Çağırma ile çalışmaya başlayın](../../quickstarts/voice-video-calling/getting-started-with-calling.md)

Daha fazla bilgi için aşağıdaki makaleleri inceleyin:
- Genel [çağrı akışları](../call-flows.md) hakkında bilgi edinin
- [PSTN çözümünüzü planlayın](../telephony-sms/plan-solution.md)
- [Çağıran istemci kitaplığı özellikleri](../voice-video-calling/calling-sdk-features.md) hakkında bilgi edinin
