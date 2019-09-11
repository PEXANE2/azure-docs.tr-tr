---
title: PıM-Azure Active Directory 'de Azure Kaynak rollerine erişimi gözden geçirme | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içindeki Azure Kaynak rollerinin erişimini incelemeyi öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 03/30/2018
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 22c0ce1a5eee4b8d4cc40c47dadd4bcdc74d03ba
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804093"
---
# <a name="review-access-to-azure-resource-roles-in-pim"></a>PıM 'de Azure Kaynak rollerine erişimi gözden geçirme
Azure Active Directory (Azure AD) Privileged Identity Management (PıM), kuruluşların Azure 'daki kaynaklara ayrıcalıklı erişimi nasıl yöneteceğini basitleştirir. 

Bir Yönetim rolüne atandıysanız, kuruluşunuzun ayrıcalıklı rol yöneticisi, işiniz için hala bu rolün gerekli olduğunu düzenli olarak doğrulamanızı isteyebilir. Bir bağlantı içeren bir e-posta alabilir veya [Azure Portal](https://portal.azure.com)doğrudan da gidebilirsiniz. Atanan rollerinizin kendi kendine gözden geçirilmesini gerçekleştirmek için bu makaledeki adımları izleyin.

Erişim gözden geçirmeleriyle ilgilenen ayrıcalıklı bir rol yöneticisiyseniz, [erişim incelemesi başlatma hakkında](pim-resource-roles-start-access-review.md)daha fazla ayrıntı alın.

## <a name="add-the-privileged-identity-management-application"></a>Privileged Identity Management uygulamasını ekleme
İncelemenizi gerçekleştirmek için [Azure portal](https://portal.azure.com/) Azure Active Directory (Azure AD) PIM uygulamasını kullanabilirsiniz. Portalınızdaki uygulamaya sahip değilseniz, başlamak için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure portal sağ üst köşesinde Kullanıcı adınızı seçin ve ardından çalıştırdığınız dizini seçin.
3. **Tüm hizmetler**' i seçin ve *Azure AD Privileged Identity Management*aramak için **filtre** kutusunu kullanın.
4. **Panoya sabitle**' yi işaretleyin ve ardından **Oluştur**' u seçin. PıM uygulaması açılır.

## <a name="approve-or-deny-access"></a>Erişimi onayla veya Reddet
Erişimi onayladığınızda veya reddetmeniz durumunda, gözden geçireni bu rolü hâlâ kullanıp kullanmayacağınızı söylemiş olursunuz. Rol üzerinde kalmak istiyorsanız **Onayla** ' yı seçin veya artık erişime Ihtiyacınız yoksa **Reddet** ' i seçin. Durumunuz yalnızca gözden geçiren sonuçları uygularken değişir.

Erişim gözden geçirmesini bulmak ve gerçekleştirmek için şu adımları izleyin:
1. Azure AD PıM uygulamasına gidin.
2. **Erişimi gözden geçir** dikey penceresini seçin.

   ![Gözden geçirme erişimi dikey penceresi seçili olan PıM uygulamasının ekran görüntüsü](media/pim-resource-roles-perform-access-review/rbac-access-review-complete.png)

3. Gerçekleştirmek istediğiniz gözden geçirmeyi seçin. 
4. **Onayla** veya **Reddet**' i seçin. **Bir neden belirtin kutusunda**kararınız için bir neden eklemeniz gerekebilir.

   ![Ayrıntıları gözden geçirme sayfasının ekran görüntüsü](media/pim-resource-roles-perform-access-review/rbac-access-review-choice.png)

## <a name="next-steps"></a>Sonraki adımlar

- [PıM 'de Azure AD rollerimin erişim incelemesini gerçekleştirme](pim-how-to-perform-security-review.md)
