---
title: PıM-Azure AD 'de Azure Kaynak rollerinin erişim incelemesini tamamlama | Microsoft Docs
description: Azure Active Directory Privileged Identity Management Azure Kaynak rolleri 'nin erişim incelemesini nasıl tamamlayacağınızı öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1601c9fc750df1f8e18bdaa072ede1b5ee164329
ms.sourcegitcommit: 24f31287b6a526e23ff5b5469113522d1ccd4467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84743822"
---
# <a name="complete-an-access-review-of-azure-resource-roles-in-privileged-identity-management"></a>Privileged Identity Management Azure Kaynak rolleri için erişim gözden geçirmesini doldurun

Ayrıcalıklı rol yöneticileri, [bir erişim gözden geçirmesi](pim-resource-roles-start-access-review.md)başlattıktan sonra ayrıcalıklı erişimi gözden geçirebilir. Azure Active Directory (Azure AD) içindeki Privileged Identity Management (PıM) otomatik olarak, kullanıcıların erişimini gözden geçirmesini isteyen bir e-posta gönderir. Bir Kullanıcı bir e-posta almazsa, bunlara [erişim incelemesi gerçekleştirme](pim-resource-roles-perform-access-review.md)yönergelerini gönderebilirsiniz.

Erişim gözden geçirme süresi bittikten sonra veya tüm kullanıcılar kendi kendini gözden geçirmeyi tamamladıktan sonra, gözden geçirmeyi yönetmek ve sonuçları görmek için bu makaledeki adımları izleyin.

## <a name="manage-access-reviews"></a>Erişim incelemelerini yönetme

1. [Azure Portal](https://portal.azure.com/)gidin. Panoda **Azure kaynakları** hizmetini seçin.

2. Kaynağınızı seçin.

3. Panonun **erişim İncelemeleri** bölümünü seçin.

    ![Azure kaynakları-erişim gözden geçirmeleri rol, sahip, başlangıç tarihi, bitiş tarihi ve durumu gösteren bir liste](media/pim-resource-roles-complete-access-review/rbac-access-review-home-list.png)

4. Yönetmek istediğiniz erişim gözden geçirmeyi seçin.

Erişim gözden geçirmesi için ayrıntı sayfasında, gözden geçirmeyi yönetmeye yönelik çeşitli seçenekler vardır. Seçenekler şunlardır:

![İncelemeyi yönetme seçenekleri-durdurma, sıfırlama, uygulama, silme](media/pim-resource-roles-complete-access-review/rbac-access-review-menu.png)

### <a name="stop"></a>Durdur

Tüm erişim incelemelerinin bitiş tarihi vardır. Erken bitmek için **Durdur** ' u seçin. Gözden geçirmeyi durdurduktan sonra, bu süre boyunca gözden geçirmesini tamamlamış olan tüm kullanıcılar bu işlemi bitiremeyecektir. Bir gözden geçirmeyi durdurduktan sonra yeniden çalıştıramazsınız.

### <a name="reset"></a>Sıfırla

Bir erişim incelemesini, üzerinde yapılan tüm kararları kaldırmak için sıfırlayabilirsiniz. Erişim gözden geçirmesini sıfırladıktan sonra, tüm kullanıcılar yeniden gözden geçirilmedi olarak işaretlenir.

### <a name="apply"></a>Uygula

Erişim incelemesi tamamlandıktan sonra, gözden geçirmeyi sonucunu uygulamak için **Uygula** ' yı seçin. Gözden geçirme sırasında bir kullanıcının erişimi reddedildiyse, bu adım rol atamasını kaldırır.  

### <a name="delete"></a>Sil

Daha fazla gözden geçirme ile ilgilenmiyorsanız, silin. **Sil** /Kaldır ' ı seçin Privileged Identity Management hizmetten gözden geçirmeyi kaldırın.

## <a name="results"></a>Sonuçlar

**Sonuçlar** sayfasında inceleme sonuçlarının listesini görüntüleyin ve indirin.

![Kullanıcıları, sonucu, nedeni, gözden geçiren, tarafından uygulanan ve sonucu olan sonuçlar sayfası](media/pim-resource-roles-complete-access-review/rbac-access-review-results.png)

## <a name="reviewers"></a>Gözden Geçirenler

Mevcut erişim gözden geçirenlere gözden geçirenler görüntüleyin ve ekleyin. Gözden geçirenlere İncelemeleri tamamlamayı hatırlatın.

![Gözden geçirenler sayfası listeleme adı ve Kullanıcı asıl adı](media/pim-resource-roles-complete-access-review/rbac-access-review-reviewers.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management Azure Kaynak rolleri için erişim gözden geçirmesi başlatma](pim-resource-roles-start-access-review.md)
- [Privileged Identity Management 'de Azure Kaynak rollerimin erişim incelemesini gerçekleştirin](pim-resource-roles-perform-access-review.md)
