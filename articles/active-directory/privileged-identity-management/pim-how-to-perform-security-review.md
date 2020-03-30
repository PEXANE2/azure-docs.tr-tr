---
title: PIM'deki Azure REKLAM rollerine erişimi gözden geçirin - Azure AD | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) Azure Etkin Dizin rollerine erişimi nasıl inceleyiş olarak inceleyebilirsiniz.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 10/22/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 76eccb5d62b68865b7a117312be62753f203e2cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73847101"
---
# <a name="review-access-to-azure-ad-roles-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde Azure REKLAM rollerine erişimi gözden geçirin

Ayrıcalıklı Kimlik Yönetimi (PIM), kuruluşların Azure Etkin Dizini (AD) ve Office 365 veya Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetlerindeki kaynaklara ayrıcalıklı erişimi nasıl yönetebildiğini kolaylaştırır. Atanan rollerinizi başarılı bir şekilde gözden geçirmek için bu makaledeki adımları izleyin.

İdari bir role atandıysanız, kuruluşunuzun ayrıcalıklı rol yöneticisi işiniz için bu role hala ihtiyacınız olduğunu düzenli olarak onaylamanızı isteyebilir. Bağlantı içeren bir e-posta alabilirsiniz veya doğrudan [Azure portalına](https://portal.azure.com) gidip başlayabilirsiniz.

Erişim değerlendirmeleri ile ilgilenen ayrıcalıklı bir rol yöneticisi veya global yöneticiyseniz, [erişim incelemesini başlatma hakkında](pim-how-to-start-security-review.md)daha fazla bilgi edinin.

## <a name="add-a-pim-dashboard-tile"></a>PIM pano döşemesi ekleme

Azure portalınızda Azure AD Ayrıcalıklı Kimlik Yönetimi hizmetini panonuza sabitletmiyorsanız, başlamak için aşağıdaki adımları izleyin.

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Azure portalının sağ üst köşesindeki kullanıcı adınızı seçin ve işletim işleceğiniz yeri seçin.
3. **Tüm hizmetler** seçeneğini belirleyin ve **Azure AD Privileged Identity Management** araması yapmak için Filtre metin kutusunu kullanın.
4. **Panoya sabitle**'yi işaretleyin ve ardından **Oluştur**’a tıklayın. Privileged Identity Management uygulaması açılır.

## <a name="approve-or-deny-access"></a>Erişimi onaylama veya reddetme

Erişimi onayladığınızda veya reddettiğinde, sadece gözden geçirene bu rolü hala kullanıp kullanmadığınızı söylersiniz. Rolde kalmak istiyorsanız **Onayla'yı** veya artık erişime ihtiyacınız yoksa **Reddet'i** seçin. İncelemeci sonuçları uygulayana kadar durumunuz hemen değişmez.
Erişim incelemesini bulmak ve tamamlamak için aşağıdaki adımları izleyin:

1. Ayrıcalıklı Kimlik Yönetimi hizmetinde, **ayrıcalıklı erişimi gözden geçir'i**seçin. Bekleyen erişim incelemeniz varsa, bunlar Azure AD **Access incelemeleri** sayfasında görünür.
2. Tamamlamak istediğiniz incelemeyi seçin.
3. İncelemeyi oluşturmadığınız sürece, incelemedeki tek kullanıcı siz görünürüz. Adınızın yanındaki onay işaretini seçin.
4. Onayla veya **Reddet'i** seçin. **Deny** Kararınızın nedenini **bir neden** metin kutusuna eklemeniz gerekebilir.  
5. Azure **REKLAM rollerini gözden geçir'i** kapatın.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar

- [PIM hizmetinde Azure kaynak rollerimin erişim gözden geçirmesini gerçekleştirme](pim-resource-roles-perform-access-review.md)
