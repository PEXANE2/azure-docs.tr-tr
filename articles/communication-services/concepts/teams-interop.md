---
title: Ekipte birlikte çalışabilirlik toplantısı
titleSuffix: An Azure Communication Services concept document
description: Ekip toplantılarına katılarak
author: chpalm
manager: chpalm
services: azure-communication-services
ms.author: chpalm
ms.date: 10/10/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 894f8b17c3c5e9e3147b66854a5809bf82351fb9
ms.sourcegitcommit: 49ea056bbb5957b5443f035d28c1d8f84f5a407b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2021
ms.locfileid: "100012412"
---
# <a name="teams-interoperability"></a>Teams ile birlikte çalışabilirlik

[!INCLUDE [Private Preview Notice](../includes/private-preview-include.md)]

Azure Iletişim Hizmetleri, Microsoft ekipleriyle etkileşime geçen özel toplantı deneyimleri oluşturmak için kullanılabilir. Iletişim Hizmetleri çözümünüzün kullanıcıları ses, video ve ekran paylaşımı üzerinde takımlar katılımcıları ile etkileşime geçebilir.

Bu birlikte çalışabilirlik, kullanıcıları takımlar toplantılarına bağlayan özel Azure uygulamaları oluşturmanıza olanak tanır. Özel uygulamalarınızın kullanıcılarının, bu özelliği denemek için Azure Active Directory kimlikleri veya takımlar lisanslarına sahip olması gerekmez. Bu, çalışanları (ekiplere tanıdık gelebilir) ve harici kullanıcıları (özel bir uygulama deneyimi kullanarak) sorunsuz bir toplantı deneyimiyle birlikte getirmek için idealdir. Bu, aşağıdakilere benzer deneyimler oluşturmanıza olanak sağlar:

1. Çalışanlar bir toplantıyı zamanlamak için takımlar kullanır
2. Özel Iletişim Hizmetleri uygulamanız, Toplantı ayrıntılarına erişmek için Microsoft Graph API 'Lerini kullanır
3. Toplantı ayrıntıları, özel uygulamanız aracılığıyla dış kullanıcılarla paylaşılır
4. Dış kullanıcılar, takım toplantısına katmak için özel uygulamanızı kullanır (istemci Kitaplığı çağıran Iletişim Hizmetleri aracılığıyla)

Bu kullanım örneği için üst düzey mimari şöyle görünür: 

![Takımlar için mimari birlikte çalışma](./media/call-flows/teams-interop.png)

Belirli takımlar, kabarık, birlikte modu ve bir arada bulunan odalar gibi özellikler de yalnızca takımlar kullanıcıları tarafından kullanılabilir. özel uygulamanız, toplantının çekirdek ses, video ve ekran paylaşım özelliklerine erişebilir.

Bir Iletişim Hizmetleri kullanıcısı takımlar toplantısına katıldığında, çağıran istemci kitaplığı aracılığıyla verilen görünen ad takımlar kullanıcıları tarafından gösterilir. Iletişim Hizmetleri kullanıcısı başka şekilde ekiplerde anonim bir kullanıcı gibi ele alınacaktır. Özel uygulamanız, takımlar toplantılarını korumak için Kullanıcı kimlik doğrulamasını ve diğer güvenlik önlemlerini göz önünde bulundurmalıdır. Anonim kullanıcıların toplantılara katılmasına imkan tanıyan güvenlik etkilerine karşı dikkatli olun ve anonim kullanıcılara sunulan özellikleri yapılandırmak için [takımlar güvenlik kılavuzunu](/microsoftteams/teams-security-guide#addressing-threats-to-teams-meetings) kullanın.

İletişim Hizmetleri kullanıcıları, [Toplantı ayarlarında](/microsoftteams/meeting-settings-in-teams)anonim birleşimler etkinleştirildiği sürece zamanlanan takımlar toplantılarına katılabilir.

## <a name="teams-in-government-clouds-gcc"></a>Kamu bulutlarındaki takımlar (GCC)
Azure Iletişim Hizmetleri birlikte çalışabilirliğine, şu anda [Microsoft 365 kamu bulutları (GCC)](/MicrosoftTeams/plan-for-government-gcc) kullanan takımlar dağıtımı yapılmasına izin verilmez. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Arama uygulamanızla bir Teams toplantısına katılma](../quickstarts/voice-video-calling/get-started-teams-interop.md)