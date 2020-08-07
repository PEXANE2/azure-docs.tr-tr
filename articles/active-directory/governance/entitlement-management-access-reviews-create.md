---
title: Azure AD Yetkilendirme Yönetimi 'nde erişim paketi için erişim gözden geçirmesi oluşturma
description: Azure Active Directory erişim gözden geçirmeleri (Önizleme) içinde Yetkilendirme Yönetimi erişim paketleri için bir erişim İnceleme ilkesi oluşturmayı öğrenin.
services: active-directory
documentationCenter: ''
author: ajburnle
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 06/18/2020
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 012eefe9140703a62d7bb1074ab763191a0976cb
ms.sourcegitcommit: fbb66a827e67440b9d05049decfb434257e56d2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2020
ms.locfileid: "87798520"
---
# <a name="create-an-access-review-of-an-access-package-in-azure-ad-entitlement-management"></a>Azure AD Yetkilendirme Yönetimi 'nde erişim paketi için erişim gözden geçirmesi oluşturma

Eski erişim riskini azaltmak için, Azure AD yetkilendirme yönetiminde bir erişim paketine etkin atamaları olan kullanıcıların düzenli olarak incelemelerine izin verilmelidir. Yeni bir erişim paketi oluşturduğunuzda veya var olan bir erişim paketini düzenlerken İncelemeleri etkinleştirebilirsiniz. Bu makalede erişim paketlerinin erişim incelemelerinin nasıl etkinleştirileceği açıklanır.

## <a name="prerequisites"></a>Ön koşullar

Erişim paketleri incelemelerini etkinleştirmek için, erişim paketi oluşturma önkoşullarını karşılamanız gerekir:
- Azure AD Premium P2
- Genel yönetici, Kullanıcı Yöneticisi, Katalog sahibi veya erişim paketi Yöneticisi

Daha fazla bilgi için bkz. [Lisans gereksinimleri](entitlement-management-overview.md#license-requirements).


## <a name="create-an-access-review-of-an-access-package"></a>Erişim paketi için erişim gözden geçirmesi oluşturma

[Yeni bir erişim paketi oluştururken](entitlement-management-access-package-create.md) veya [var olan bir erişim paketi ilkesini düzenlediğinizde](entitlement-management-access-package-lifecycle-policy.md) , erişim incelemelerini etkinleştirebilirsiniz. Erişim paketinin erişim incelemelerini etkinleştirmek için şu adımları izleyin:

1. Erişim paketi için **yaşam döngüsü** sekmesini açın ve **erişim gözden geçirmeleri**' na kaydırın.

1. **Erişim Incelemelerini gerektir** ' ın taşınması **Evet**' e geçiş yapar.

    ![Erişim gözden geçirmesini ekleme](./media/entitlement-management-access-reviews/access-reviews-pane.png)

1. Gözden geçirmeler 'in başlaması için **izlenecek**tarihi belirtin.

1. Ardından, **Gözden geçirme sıklığını** **yıllık**, **iki**yıllık, **üç** aylık veya **aylık**olarak ayarlayın.
Bu ayar, erişim incelemelerinin ne sıklıkla gerçekleşeceğini belirler.

1. Her bir yinelenen serinin her incelemesinin gözden geçirenlerden giriş için ne kadar gün açık olacağını tanımlamak için **süreyi** ayarlayın. Örneğin, 1 Ocak 'ta başlayan ve gözden geçirenlerin ayın sonuna kadar olan 30 gün boyunca incelenmesi için açık olan yıllık bir gözden geçirme zamanlayabilirsiniz.

1. Gözden **geçirenler**' in yanında, kullanıcıların kendi erişim incelemesini gerçekleştirmesini Istiyorsanız **kendi kendine gözden geçir** ' i seçin veya bir gözden geçiren belirlemek istiyorsanız **belirli gözden** geçirenler ' i seçin.

    ![Gözden geçirenler Ekle 'yi seçin](./media/entitlement-management-access-reviews/access-reviews-add-reviewer.png)

1. **Belirli gözden geçirenler**' i seçtiyseniz, hangi kullanıcıların erişim gözden geçirmesini yapacağız belirtin:
    1. **Gözden geçirenler Ekle**' yi seçin.
    1. **Gözden geçirenler Seç** bölmesinde, gözden geçiren olmasını istediğiniz kullanıcı (ler) i arayın ve seçin.
    1. Gözden geçiriciyi seçtiğinizde **Seç** düğmesine tıklayın.

    ![Gözden geçirenleri belirtin](./media/entitlement-management-access-reviews/access-reviews-select-reviewer.png)

1. Sayfanın alt kısmındaki bir erişim paketini düzenliyorsanız yeni bir erişim paketi veya **güncelleştirme** oluşturuyorsanız, **gözden geçir + oluştur** ' a tıklayın.

## <a name="view-the-status-of-the-access-review"></a>Erişim gözden geçirmesi durumunu görüntüleme

Başlangıç tarihinden sonra erişim gözden **geçirmeleri** bölümünde bir erişim incelemesi listelenecektir. Erişim gözden geçirmesi durumunu görüntülemek için aşağıdaki adımları izleyin:

1. **Kimlik**Yönetimi ' nde, **erişim paketleri** ' ne tıklayın ve ardından denetlemek istediğiniz erişim gözden geçirme durumunu içeren erişim paketini seçin.   

1. Erişim paketine genel bakış ' a ulaştıktan sonra sol menüdeki **erişim İncelemeleri** ' ne tıklayın.
    
    ![Erişim incelemelerini seçin](./media/entitlement-management-access-reviews/access-review-status-access-package-overview.png)

1. Kendileriyle ilişkili erişim gözden geçirmeleri olan tüm ilkeleri içeren bir liste görüntülenir. Raporunu görmek için gözden geçirmeyi tıklatın.

    ![Erişim gözden geçirmeleri listesi](./media/entitlement-management-access-reviews/access-review-status-select-access-reviews.png)
   
1. Raporu görüntülediğinizde, gözden geçirilen kullanıcı sayısını ve gözden geçiren tarafından gerçekleştirilen eylemleri görüntüler.

    ![İnceleme durumunu görüntüle](./media/entitlement-management-access-reviews/access-review-status.png)
 

## <a name="access-reviews-email-notifications"></a>Erişim gözden geçirmeleri e-posta bildirimleri
Gözden geçirenleri belirtebilir veya kullanıcılar kendi erişimini gözden geçirebilir. Varsayılan olarak, Azure AD gözden geçirenlere veya İnceleme başladıktan kısa bir süre sonra otomatik gözden geçirenlere e-posta gönderir.

E-postada, erişim paketlerine erişimin nasıl incelemesinin yapılacağı hakkında yönergeler de yer alır. Gözden geçirme kullanıcıların erişimini gözden geçirmesine yönelik ise, bunlara erişim paketlerinin kendi kendini gözden geçirmesinin nasıl yapılacağı hakkında yönergeler görüntüler.
  
Konuk kullanıcıları gözden geçirenler olarak atadıysanız ve Azure AD Konuk davetini kabul etmediyse, Azure AD erişim gözden geçirmelerinden e-posta almaz. Önce, e-postaları alabilmesi için önce daveti kabul etmeli ve Azure AD ile bir hesap oluşturmaları gerekir. 

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim paketlerine erişimi gözden geçirme](entitlement-management-access-reviews-review-access.md)
- [Erişim paketlerini kendi kendine gözden geçirme](entitlement-management-access-reviews-self-review.md)
