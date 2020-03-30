---
title: PIM'deki Azure kaynak rollerine erişimi gözden geçirin - Azure AD | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) Azure kaynak rollerinin erişimini nasıl inceleyiş olarak inceleyiş olarak öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7ddb4d0ff1339f1427c5041528cdbe464a345b37
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73847010"
---
# <a name="review-access-to-azure-resource-roles-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rollerine erişimi gözden geçirin

Ayrıcalıklı Kimlik Yönetimi (PIM) erişim incelemeleri, Azure Etkin Dizini'ndeki (Azure AD) ayrıcalıklı rollere erişimin sağlanmasına yardımcı olabilir. Bu makalede, bir Azure REKLAM erişim incelemesinde ayrıcalıklı rol atamalarınızı gözden geçirme adımlarını tamamlayınız.

İdari bir role atandıysanız, bir role olan ihtiyacınızı onaylamak için yöneticiniz tarafından bir erişim incelemesini tamamlamanız gerekebilir. Onay isteği, bağlantı içeren bir e-posta gelebilir veya [Azure portalında](https://portal.azure.com)onaylayabilirsiniz.

Erişim değerlendirmeleri ile ilgilenen ayrıcalıklı bir rol yöneticisiyseniz, [erişim incelemesini başlatma hakkında](pim-resource-roles-start-access-review.md)daha fazla bilgi alın.

## <a name="approve-or-deny-access"></a>Erişimi onaylama veya reddetme

Bu rolü hala kullanıp kullanmadığınıza bağlı olarak erişimi onaylayabilir veya reddedebilirsiniz. Rolde kalmak istiyorsanız **Onayla'yı** veya artık erişime ihtiyacınız yoksa **Reddet'i** seçin. Durumunuz yalnızca gözden geçiren sonuçları uyguladığında değişir.

Erişim incelemesini bulmak ve tamamlamak için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com/)oturum açın.
1. **Azure Etkin Dizini'ni** seçin ve **Ayrıcalıklı Kimlik Yönetimi'ni**açın.
1. **Erişimi İncele'yi**seçin.

   ![Ayrıcalıklı Kimlik Yönetimi uygulamasının ekran görüntüsü, Gözden Geçir erişim bıçağı seçili](media/pim-resource-roles-perform-access-review/rbac-access-review-complete.png)

1. Tamamlamak istediğiniz incelemeyi seçin.
1. Onayla veya **Reddet'i** seçin. **Deny** Bir **neden sağla kutusuna,** gerekirse kararınız için bir iş gerekçesi girin.

   ![Gözden Geçirme ayrıntıları sayfasının ekran görüntüsü](media/pim-resource-roles-perform-access-review/rbac-access-review-choice.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'ndeki Azure REKLAM rollerimin erişim incelemesini gerçekleştirin](pim-how-to-perform-security-review.md)
