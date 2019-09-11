---
title: PıM-Azure Active Directory 'de Azure Kaynak rolleri 'ne yönelik erişim incelemesini tamamlar | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içindeki Azure Kaynak rollerinin erişim incelemesini tamamlamayı öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/02/2018
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3b9563a4032011b999bf867fc782ba4cbb9c3fac
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804248"
---
# <a name="complete-an-access-review-of-azure-resource-roles-in-pim"></a>PıM 'de Azure Kaynak rolleri 'ne yönelik erişim incelemesini tamamlar
Ayrıcalıklı rol yöneticileri, [erişim incelemesi](pim-resource-roles-start-access-review.md)başlatıldıktan sonra ayrıcalıklı erişimi gözden geçirebilir. Azure Active Directory (Azure AD) Privileged Identity Management (PıM), kullanıcıların erişimini gözden geçirmesini isteyen bir e-posta otomatik olarak gönderir. Bir Kullanıcı bir e-posta almazsa, bunlara [erişim incelemesi gerçekleştirme](pim-resource-roles-perform-access-review.md)yönergelerini gönderebilirsiniz.

Erişim gözden geçirme süresi bittikten sonra veya tüm kullanıcılar kendi kendini gözden geçirmeyi tamamladıktan sonra, gözden geçirmeyi yönetmek ve sonuçları görmek için bu makaledeki adımları izleyin.

## <a name="manage-access-reviews"></a>Erişim incelemelerini yönetme
1. [Azure Portal](https://portal.azure.com/) gidin. Ardından, panoda **Azure kaynakları** uygulamasını seçin.

2. Kaynağınızı seçin.

3. Panonun **erişim İncelemeleri** bölümünü seçin.

    ![Azure kaynakları-erişim gözden geçirmeleri rol, sahip, başlangıç tarihi, bitiş tarihi ve durumu gösteren bir liste](media/pim-resource-roles-complete-access-review/rbac-access-review-home-list.png)

4. Yönetmek istediğiniz erişim gözden geçirmeyi seçin.

Erişim incelemesinin ayrıntı dikey penceresinde, gözden geçirmeyi yönetmeye yönelik çeşitli seçenekler vardır. Seçenekler şunlardır:

![İncelemeyi yönetme seçenekleri-durdurma, sıfırlama, uygulama, silme](media/pim-resource-roles-complete-access-review/rbac-access-review-menu.png)

### <a name="stop"></a>Durdur
Tüm erişim incelemelerinin bitiş tarihi vardır, ancak daha önce bitirmek için **Durdur** düğmesini kullanabilirsiniz. Gözden geçirmeyi durdurduktan sonra bu süre içinde gözden geçirmesini tamamlamayan tüm kullanıcılar bu süreyi bitiremeyecektir. Bir gözden geçirmeyi durdurduktan sonra yeniden çalıştıramazsınız.

### <a name="reset"></a>Sıfırla
Bir erişim incelemesini, üzerinde yapılan tüm kararları kaldırmak için sıfırlayabilirsiniz. Erişim gözden geçirmesini sıfırladıktan sonra, tüm kullanıcılar geri gözden geçirilmedi olarak işaretlenir. 

### <a name="apply"></a>Uygula
Erişim incelemesi tamamlandıktan sonra, gözden geçirme sonucunu uygulamak için **Uygula** düğmesini kullanın. Gözden geçirme sırasında bir kullanıcının erişimi reddedildiyse, bu adım rol atamasını kaldırır.  

### <a name="delete"></a>Sil
Daha fazla gözden geçirme ile ilgilenmiyorsanız, silin. **Sil** DÜĞMESI, PIM uygulamasından gözden geçirmeyi kaldırır.

## <a name="results"></a>Sonuçlar
**Sonuçlar** sayfasında inceleme sonuçlarının listesini görüntüleyin ve indirin. 

![Kullanıcıları, sonucu, nedeni, gözden geçiren, tarafından uygulanan ve sonucu olan sonuçlar sayfası](media/pim-resource-roles-complete-access-review/rbac-access-review-results.png)

## <a name="reviewers"></a>Gözden Geçirenler
Mevcut erişim gözden geçirenlere gözden geçirenler görüntüleyin ve ekleyin. Gözden geçirenlere İncelemeleri tamamlamayı hatırlatın.

![Gözden geçirenler sayfası listeleme adı ve Kullanıcı asıl adı](media/pim-resource-roles-complete-access-review/rbac-access-review-reviewers.png)

## <a name="next-steps"></a>Sonraki adımlar

- [PIM hizmetinde Azure kaynak rolleri için erişim gözden geçirmesi başlatma](pim-resource-roles-start-access-review.md)
- [PIM hizmetinde Azure kaynak rollerimin erişim gözden geçirmesini gerçekleştirme](pim-resource-roles-perform-access-review.md)
