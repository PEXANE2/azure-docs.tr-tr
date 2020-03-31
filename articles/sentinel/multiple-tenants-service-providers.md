---
title: MSSP hizmet sağlayıcıları için Azure Sentinel'e birden fazla kiracıyla çalışın| Microsoft Dokümanlar
description: MSSP hizmet sağlayıcıları için Azure Sentinel'e birden çok kiracıyla çalışma.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: fdb58686fcdd18a8e2861aab533717dbc91e8893
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476024"
---
# <a name="work-with-multiple-tenants-in-azure-sentinel"></a>Azure Sentinel'de birden çok kiracıyla çalışma 

Yönetilen bir güvenlik hizmeti sağlayıcısıysanız (MSSP) ve müşterilerinizin güvenlik operasyon merkezlerini (SOC) yönetmek için [Azure Deniz Feneri](../lighthouse/overview.md) kullanıyorsanız, müşterilerinizin Azure Sentinel kaynaklarını doğrudan müşterinin kiracısına, kendi Azure kiracınızdan bağlanmadan yönetebilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar
- [Yerleşik Azure Deniz Feneri](../lighthouse/how-to/onboard-customer.md)
- Bunun düzgün çalışması için kiracınızın en az bir abonelikle Azure Sentinel Kaynak Sağlayıcısı'na kayıtlı olması gerekir. Kiracınızda kayıtlı bir Azure Sentinel varsa, başlamaya hazırsınız. Değilse, Azure portalından **Abonelikler'i** seçin ve ardından **Kaynak sağlayıcıları**tarafından izlenir.  Daha sonra, **SOC - Kaynak sağlayıcıları** ekranından, arama ve seçin `Microsoft.OperationalInsights` ve `Microsoft.SecurityInsights` **Kaydol**seçin.
   ![Kaynak sağlayıcılarını denetleyin](media/multiple-tenants-service-providers/check-resource-provider.png)
## <a name="how-to-access-azure-sentinel-from-other-tenants"></a>Azure Sentinel'e diğer kiracılardan nasıl erişilir?
1. **Dizin + abonelik**altında, temsilci dizinlerini ve müşterinizin Azure Sentinel çalışma alanlarının bulunduğu abonelikleri seçin.

   ![Güvenlik olayları oluşturma](media/multiple-tenants-service-providers/directory-subscription.png)

1. Azure Sentinel'i açın. Seçili aboneliklerde tüm çalışma alanlarını görürsünüz ve kendi kiracınızdaki tüm çalışma alanları gibi onlarla sorunsuz bir şekilde çalışabilirsiniz.

> [!NOTE]
> Azure Sentinel'de konektörleri yönetilen bir çalışma alanı içinden dağıtamazsınız. Bir bağlayıcı dağıtmak için, doğrudan bir bağlayıcı dağıtmak istediğiniz kiracı oturum açmanız ve gerekli izinleri orada kimlik doğrulaması gerekir.





## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, birden çok Azure Sentinel kiracısını sorunsuz bir şekilde nasıl yöneteceklerini öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.

