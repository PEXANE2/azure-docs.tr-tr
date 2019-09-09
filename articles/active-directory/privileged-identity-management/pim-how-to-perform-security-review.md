---
title: PıM-Azure Active Directory 'de Azure AD rollerine erişimi gözden geçirme | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içinde Azure AD rollerinin erişimini incelemeyi öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 06/21/2018
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3bd548041b086eef8d788a696497163c756fd5b7
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804431"
---
# <a name="review-access-to-azure-ad-roles-in-pim"></a>PıM 'de Azure AD rollerine erişimi gözden geçirme

Azure Active Directory (AD) Privileged Identity Management (PıM), kuruluşların Azure AD 'deki kaynaklara yönelik ayrıcalıklı erişimi ve Office 365 veya Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetler yönetmesini basitleştirir.  

Bir Yönetim rolüne atandıysanız, kuruluşunuzun ayrıcalıklı rol yöneticisi, işiniz için hala bu rolün gerekli olduğunu düzenli olarak doğrulamanızı isteyebilir. Bir bağlantı içeren bir e-posta alabilir veya [Azure Portal](https://portal.azure.com)doğrudan da gidebilirsiniz. Atanan rollerinizin kendi kendine gözden geçirilmesini gerçekleştirmek için bu makaledeki adımları izleyin.

Erişim gözden geçirmeleriyle ilgilenen ayrıcalıklı bir rol yöneticisiyseniz veya küresel yöneticisiyseniz, [bir erişim incelemesi başlatma hakkında](pim-how-to-start-security-review.md)daha fazla bilgi alın.

## <a name="add-the-privileged-identity-management-application"></a>Privileged Identity Management uygulamasını ekleme
İncelemenizi gerçekleştirmek için [Azure portal](https://portal.azure.com/) Azure AD PRIVILEGED IDENTITY Management (PIM) uygulamasını kullanabilirsiniz.  Portalınızdaki Azure AD Privileged Identity Management uygulamasına sahip değilseniz, başlamak için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure portal sağ üst köşesindeki Kullanıcı adınızı seçin ve çalıştıracağınız dizini seçin.
3. **Tüm hizmetler** seçeneğini belirleyin ve **Azure AD Privileged Identity Management** araması yapmak için Filtre metin kutusunu kullanın.
4. **Panoya sabitle**'yi işaretleyin ve ardından **Oluştur**’a tıklayın. Privileged Identity Management uygulaması açılır.

## <a name="approve-or-deny-access"></a>Erişimi onayla veya Reddet
Erişimi onayladığınızda veya reddetmeniz durumunda, gözden geçireni bu rolü hâlâ kullanıp kullanmayacağınızı söylemiş olursunuz. Rol üzerinde kalmak istiyorsanız **Onayla** ' yı seçin veya artık erişime Ihtiyacınız yoksa **Reddet** ' i seçin. Gözden geçiren sonuçları uygulayaana kadar durumunuz hemen değişmez.
Erişim gözden geçirmesini bulmak ve gerçekleştirmek için şu adımları izleyin:

1. PıM uygulamasında **ayrıcalıklı erişimi gözden geçir**' i seçin. Bekleyen erişim gözden geçirmeleri varsa, bunlar Azure AD erişim gözden geçirmeleri dikey penceresinde görünürler.
2. Gerçekleştirmek istediğiniz gözden geçirmeyi seçin.
3. Gözden geçirmeyi oluşturmadığınız sürece, incelemede yalnızca Kullanıcı olarak görüntülenir. Adınızın yanındaki onay işaretini seçin.
4. **Onayla** veya **Reddet**' i seçin. **Bir neden belirtin** metin kutusu içinde kararınız için bir neden eklemeniz gerekebilir.  
5. **Azure AD rollerini gözden geçir** dikey penceresini kapatın.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar

- [PIM hizmetinde Azure kaynak rollerimin erişim gözden geçirmesini gerçekleştirme](pim-resource-roles-perform-access-review.md)
