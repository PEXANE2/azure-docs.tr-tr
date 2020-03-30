---
title: PIM' deki Azure kaynak rollerinin erişim incelemesini bitiş - Azure AD | Microsoft Dokümanlar
description: Azure Etkin Dizini'nde Azure kaynak rollerinin erişim incelemesini nasıl tamamlayabilirsiniz Ayrıcalıklı Kimlik Yönetimi öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9e45249245aaab97070b7e774d4b6bab6827bdc9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74021991"
---
# <a name="complete-an-access-review-of-azure-resource-roles-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'ndeki Azure kaynak rollerinin erişim incelemesini tamamlayın

Ayrıcalıklı rol [yöneticileri, bir erişim incelemesibaşlattıktan](pim-resource-roles-start-access-review.md)sonra ayrıcalıklı erişimi gözden geçirebilirsiniz. Azure Etkin Dizini'ndeki (Azure AD) Ayrıcalıklı Kimlik Yönetimi (PIM), kullanıcıların erişimlerini gözden geçirmelerini sağlayan bir e-postayı otomatik olarak gönderir. Bir kullanıcı bir e-posta almazsa, onlara [erişim incelemesinin nasıl gerçekleştirilebileceğine](pim-resource-roles-perform-access-review.md)yönelik talimatları gönderebilirsiniz.

Erişim inceleme süresi bittikten sonra veya tüm kullanıcılar kendi öz incelemelerini tamamladıktan sonra, incelemeyi yönetmek ve sonuçları görmek için bu makaledeki adımları izleyin.

## <a name="manage-access-reviews"></a>Erişim yorumlarını yönetme

1. [Azure portalına](https://portal.azure.com/)gidin. Panoda **Azure kaynakları** hizmetini seçin.

2. Kaynağınızı seçin.

3. Panodaki **Access değerlendirmeleri** bölümünü seçin.

    ![Azure kaynakları - Rolü, sahibini, başlangıç tarihini, bitiş tarihini ve durumu gösteren incelemeler listesine eriş](media/pim-resource-roles-complete-access-review/rbac-access-review-home-list.png)

4. Yönetmek istediğiniz erişim incelemesini seçin.

Erişim incelemesi için ayrıntı sayfasında, bu incelemeyi yönetmek için bir dizi seçenek vardır. Seçenekler şunlardır:

![İnceleme yi yönetme seçenekleri - Durdurma, Sıfırlama, Uygula, Sil](media/pim-resource-roles-complete-access-review/rbac-access-review-menu.png)

### <a name="stop"></a>Durdur

Tüm erişim yorumlarının bir bitiş tarihi var. Erken bitirmek için **Dur'u** seçin. Bu zamana kadar incelemelerini bitirmemiş olan kullanıcılar, incelemeyi durdurduktan sonra incelemeyi tamamlayamaz. Durdurulduktan sonra incelemeyi yeniden başlatamazsınız.

### <a name="reset"></a>Sıfırla

Üzerinde yapılan tüm kararları kaldırmak için bir erişim incelemesini sıfırlayabilirsiniz. Bir erişim incelemesini sıyrıkladıktan sonra, tüm kullanıcılar yeniden gözden geçirilmeyecek olarak işaretlenir.

### <a name="apply"></a>Uygula

Erişim incelemesi tamamlandıktan sonra, incelemenin sonucunu uygulamak için **Uygula'yı** seçin. Gözden geçirmede bir kullanıcının erişimi reddedildiyse, bu adım rol atamasını kaldırır.  

### <a name="delete"></a>Sil

Artık incelemeyle ilgilenmiyorsanız, silin. **Silme** yo'yu ayrıcalıklı kimlik yönetimi hizmetinden kaldır'ı seçin.

## <a name="results"></a>Sonuçlar

**Sonuçlar** sayfasında, inceleme sonuçlarınızın listesini görüntüleyin ve indirin.

![Sonuç sayfası kullanıcıları listeleyen, sonuç, neden, gözden geçiren, uygulayan ve sonuç uygulayan](media/pim-resource-roles-complete-access-review/rbac-access-review-results.png)

## <a name="reviewers"></a>Gözden Geçirenler

Varolan erişim incelemenize gözden geçirenleri görüntüleyin ve ekleyin. Gözden geçirenlere yorumlarını tamamlamalarını hatırlatın.

![Gözden geçirenler sayfa listeleme adı ve kullanıcı ana adı](media/pim-resource-roles-complete-access-review/rbac-access-review-reviewers.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rolleri için bir erişim incelemesi başlatma](pim-resource-roles-start-access-review.md)
- [Ayrıcalıklı Kimlik Yönetimi'ndeki Azure kaynak rollerimin erişim incelemesini gerçekleştirin](pim-resource-roles-perform-access-review.md)
