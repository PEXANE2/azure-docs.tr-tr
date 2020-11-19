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
ms.openlocfilehash: 028e7f9764cbf75263736471f434e04bc6daca0f
ms.sourcegitcommit: 230d5656b525a2c6a6717525b68a10135c568d67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2020
ms.locfileid: "94886830"
---
# <a name="teams-interoperability"></a>Teams ile birlikte çalışabilirlik

[!INCLUDE [Private Preview Notice](../../includes/private-preview-include.md)]

Azure Iletişim Hizmetleri, Microsoft ekipleriyle etkileşime geçen özel toplantı deneyimleri oluşturmak için kullanılabilir. Iletişim Hizmetleri çözümünüzün kullanıcıları ses, video ve ekran paylaşımı üzerinde takımlar katılımcıları ile etkileşime geçebilir.

Bu birlikte çalışabilirlik, kullanıcıları takımlar toplantılarına bağlayan özel Azure uygulamaları oluşturmanıza olanak tanır. Özel uygulamalarınızın kullanıcılarının, bu özelliği denemek için Azure Active Directory kimlikleri veya takımlar lisanslarına sahip olması gerekmez. Bu, çalışanları (ekiplere tanıdık gelebilir) ve harici kullanıcıları (özel bir uygulama deneyimi kullanarak) sorunsuz bir toplantı deneyimiyle birlikte getirmek için idealdir. Bu, aşağıdakilere benzer deneyimler oluşturmanıza olanak sağlar:

1. Çalışanlar bir toplantıyı zamanlamak için takımlar kullanır
2. Özel Iletişim Hizmetleri uygulamanız, Toplantı ayrıntılarına erişmek için Microsoft Graph API 'Lerini kullanır
3. Toplantı ayrıntıları, özel uygulamanız aracılığıyla dış kullanıcılarla paylaşılır
4. Dış kullanıcılar, takım toplantısına katmak için özel uygulamanızı kullanır (istemci Kitaplığı çağıran Iletişim Hizmetleri aracılığıyla)

Bu kullanım örneği için üst düzey mimari şöyle görünür: 

![Takımlar için mimari birlikte çalışma](..//media/call-flows/teams-interop.png)

Belirli takımlar, kabarık, birlikte modu ve bir arada bulunan odalar gibi özellikler de yalnızca takımlar kullanıcıları tarafından kullanılabilir. özel uygulamanız, toplantının çekirdek ses, video ve ekran paylaşım özelliklerine erişebilir.

Bir Iletişim Hizmetleri kullanıcısı takımlar toplantısına katıldığında, çağıran istemci kitaplığı aracılığıyla verilen görünen ad takımlar kullanıcıları tarafından gösterilir. Iletişim Hizmetleri kullanıcısı başka şekilde ekiplerde anonim bir kullanıcı gibi ele alınacaktır. Özel uygulamanız, takımlar toplantılarını korumak için Kullanıcı kimlik doğrulamasını ve diğer güvenlik önlemlerini göz önünde bulundurmalıdır. Anonim kullanıcıların toplantılara katılmasına imkan tanıyan güvenlik etkilerine karşı dikkatli olun ve anonim kullanıcılara sunulan özellikleri yapılandırmak için [takımlar güvenlik kılavuzunu](/microsoftteams/teams-security-guide#addressing-threats-to-teams-meetings) kullanın.

İletişim Hizmetleri kullanıcıları, [Toplantı ayarlarında](/microsoftteams/meeting-settings-in-teams)anonim birleşimler etkinleştirildiği sürece zamanlanan takımlar toplantılarına katılabilir.



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Arama uygulamanızla bir Teams toplantısına katılma](../../quickstarts/voice-video-calling/get-started-teams-interop.md)