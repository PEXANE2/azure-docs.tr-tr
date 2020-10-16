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
ms.openlocfilehash: 5035e511b0f3a105a047738fed1c5cc17e88d086
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92114656"
---
# <a name="teams-interoperability"></a>Takımlar birlikte çalışabilirliği

[!INCLUDE [Public Preview Notice](../../includes/private-preview-include.md)]

Azure Iletişim Hizmetleri, Microsoft ekipleriyle etkileşime geçen özel toplantı deneyimleri oluşturmak için kullanılabilir. Iletişim Hizmetleri çözümünüzün kullanıcıları ses, video ve ekran paylaşımı üzerinde takımlar katılımcıları ile etkileşime geçebilir.

Bu birlikte çalışabilirlik, kullanıcıları takımlar toplantılarına bağlayan özel Azure uygulamaları oluşturmanıza olanak tanır. Özel uygulamalarınızın kullanıcılarının, bu özelliği denemek için Azure Active Directory kimlikleri veya takımlar lisanslarına sahip olması gerekmez. Bu, çalışanları (ekiplere tanıdık gelebilir) ve harici kullanıcıları (özel bir uygulama deneyimi kullanarak) sorunsuz bir toplantı deneyimiyle birlikte getirmek için idealdir. Bu, aşağıdakilere benzer deneyimler oluşturmanıza olanak sağlar:

1. Çalışanlar bir toplantıyı zamanlamak için takımlar kullanır
2. Özel Iletişim Hizmetleri uygulamanız, Toplantı ayrıntılarına erişmek için Microsoft Graph API 'Lerini kullanır
3. Toplantı ayrıntıları, özel uygulamanız aracılığıyla dış kullanıcılarla paylaşılır
4. Dış kullanıcılar, takım toplantısına katmak için özel uygulamanızı kullanır (istemci Kitaplığı çağıran Iletişim Hizmetleri aracılığıyla)

Bu kullanım örneği için üst düzey mimari şöyle görünür: 

![Takımlar için mimari birlikte çalışma](..//media/call-flows/teams-interop.png)

Belirli takımlar, kabarık, birlikte modu ve bir arada bulunan odalar gibi özellikler de yalnızca takımlar kullanıcıları tarafından kullanılabilir. özel uygulamanız, toplantının çekirdek ses, video ve ekran paylaşım özelliklerine erişebilir.

Bir Iletişim Hizmetleri kullanıcısı takımlar toplantısına katıldığında, çağıran istemci kitaplığı aracılığıyla verilen görünen ad takımlar kullanıcıları tarafından gösterilir. Iletişim Hizmetleri kullanıcısı başka şekilde ekiplerde anonim bir kullanıcı gibi ele alınacaktır. Özel uygulamanız, takımlar toplantılarını korumak için Kullanıcı kimlik doğrulamasını ve diğer güvenlik önlemlerini göz önünde bulundurmalıdır. Anonim kullanıcıların toplantılara katılmasına imkan tanıyan güvenlik etkilerine karşı dikkatli olun ve anonim kullanıcılara sunulan özellikleri yapılandırmak için [takımlar güvenlik kılavuzunu](https://docs.microsoft.com/microsoftteams/teams-security-guide#addressing-threats-to-teams-meetings) kullanın.

Iletişim Hizmetleri kullanıcılarının, [Toplantı ayarlarında](https://docs.microsoft.com/microsoftteams/meeting-settings-in-teams)anonim birleşimler etkinleştirildiği sürece zamanlanan takımlar toplantılarına katılabileceğini unutmayın.



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Çağıran uygulamanızı bir takım toplantısına ekleyin](../../quickstarts/voice-video-calling/get-started-teams-interop.md)
