---
title: MSSP hizmet sağlayıcıları için Azure Sentinel 'e birden çok kiracı ile çalışma | Microsoft Docs
description: MSSP hizmet sağlayıcıları için Azure Sentinel 'e birden çok kiracı ile çalışma.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79476024"
---
# <a name="work-with-multiple-tenants-in-azure-sentinel"></a>Azure Sentinel 'de birden çok kiracı ile çalışma 

Yönetilen bir güvenlik hizmeti sağlayıcısıysanız (MSSP) ve müşterilerinizin güvenlik işlemleri merkezlerini (SOC) yönetmek için [Azure Mathouse](../lighthouse/overview.md) kullanıyorsanız, kendi Azure kiracınızdan doğrudan müşterinin kiracısına bağlanmadan müşterilerinizin Azure Sentinel kaynaklarını yönetebileceksiniz. 

## <a name="prerequisites"></a>Ön koşullar
- [Azure ışıklı kullanımı](../lighthouse/how-to/onboard-customer.md)
- Bunun düzgün çalışması için kiracınızın Azure Sentinel kaynak sağlayıcısına en az bir abonelikte kayıtlı olması gerekir. Kiracınızda kayıtlı bir Azure Sentinel varsa, kullanmaya başlamak için hazırlanın. Aksi takdirde, Azure portal **abonelikler** ' i ve ardından **kaynak sağlayıcıları**' nı seçin.  Ardından, **SOC kaynak sağlayıcıları** ekranından, ve ' ı arayıp seçin ve `Microsoft.OperationalInsights` `Microsoft.SecurityInsights` **Kaydet**' i seçin.
   ![Kaynak sağlayıcılarını denetleme](media/multiple-tenants-service-providers/check-resource-provider.png)
## <a name="how-to-access-azure-sentinel-from-other-tenants"></a>Diğer kiracılardan Azure Sentinel 'e erişme
1. **Dizin + abonelik**altında, temsilcili dizinler ve müşterinizin Azure Sentinel çalışma alanlarının bulunduğu abonelikler ' i seçin.

   ![Güvenlik Olayları oluştur](media/multiple-tenants-service-providers/directory-subscription.png)

1. Azure Sentinel 'i açın. Seçili aboneliklerdeki tüm çalışma alanlarını görürsünüz ve kendi kiracınızdaki herhangi bir çalışma alanı gibi bunlarla sorunsuz bir şekilde çalışabilirsiniz.

> [!NOTE]
> Yönetilen bir çalışma alanının içinden Azure Sentinel 'e bağlayıcılar dağıtamazsınız. Bir bağlayıcıyı dağıtmak için, bağlayıcı dağıtmak istediğiniz kiracıda doğrudan oturum açmanız ve gerekli izinlerle ilgili kimlik doğrulaması yapmanız gerekir.





## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, birden çok Azure Sentinel kiracıyı sorunsuz bir şekilde yönetmeyi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.

