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
ms.openlocfilehash: caa79b572d0024b93abd2d32ca99d92cc2a8b4bb
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77582084"
---
# <a name="work-with-multiple-tenants-in-azure-sentinel"></a>Azure Sentinel 'de birden çok kiracı ile çalışma 

Yönetilen bir güvenlik hizmeti sağlayıcısıysanız (MSSP) ve müşterilerinizin güvenlik işlemleri merkezlerini (SOC) yönetmek için [Azure Mathouse](../lighthouse/overview.md) kullanıyorsanız, kendi Azure kiracınızdan doğrudan müşterinin kiracısına bağlanmadan müşterilerinizin Azure Sentinel kaynaklarını yönetebileceksiniz. 

## <a name="prerequisites"></a>Önkoşullar
- [Azure ışıklı kullanımı](../lighthouse/how-to/onboard-customer.md)
- Bunun düzgün çalışması için kiracınızın Azure Sentinel kaynak sağlayıcısına en az bir abonelikte kayıtlı olması gerekir. Kiracınızda kayıtlı bir Azure Sentinel varsa, kullanmaya başlamak için hazırlanın. Aksi takdirde, Azure portal, **abonelikler** ' i ve ardından **kaynak sağlayıcıları** ' nı seçin ve ardından `Microsoft.Security.Insights` arayıp **Kaydet**' i seçin.
   Kaynak sağlayıcılarını denetleme ![](media/multiple-tenants-service-providers/check-resource-provider.png)
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

