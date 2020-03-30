---
title: Azure AD yetkilendirme yönetiminde bir erişim paketinin erişim incelemesi oluşturma
description: Azure Active Directory erişim incelemelerinde (Önizleme) yetkilendirme yönetimi erişim paketleri için bir erişim gözden geçirme ilkesini nasıl oluşturabileceğinizi öğrenin.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 11/01/2019
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: a862bbb1f574e4adab2f7d8e59a1abe8e5a5fa2a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73608845"
---
# <a name="create-an-access-review-of-an-access-package-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde bir erişim paketinin erişim incelemesi oluşturma

Eski erişim riskini azaltmak için, Azure AD yetkilendirme yönetiminde bir erişim paketine etkin atamaları olan kullanıcıların periyodik incelemelerini etkinleştirmelisiniz. Yeni bir erişim paketi oluşturduğunuzda veya varolan bir erişim paketini edindiğinizde incelemeleri etkinleştirebilirsiniz. Bu makalede, erişim paketlerinin erişim değerlendirmelerini nasıl etkinleştirilir açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Erişim paketlerinin gözden geçirildirilmesini etkinleştirmek için, bir erişim paketi oluşturmak için ön koşulları karşılamanız gerekir:
- Azure AD Premium P2
- Genel yönetici, Kullanıcı yöneticisi, Katalog sahibi veya Access paket yöneticisi

Daha fazla bilgi için Lisans [gereksinimlerine](entitlement-management-overview.md#license-requirements)bakın.


## <a name="create-an-access-review-of-an-access-package"></a>Erişim paketinin erişim incelemesi oluşturma

Yeni bir erişim [paketi oluştururken](entitlement-management-access-package-create.md) veya [varolan bir erişim paketi](entitlement-management-access-package-lifecycle-policy.md) ilkesini düzenlerken erişim gözden lemelerini etkinleştirebilirsiniz. Erişim paketinin erişim değerlendirmelerini etkinleştirmek için aşağıdaki adımları izleyin:

1. Erişim paketi için **Yaşam Döngüsü** sekmesini açın ve **Access Reviews'a**gidin.

1. Erişim **İncelemeleri Için Gerek'i** **Evet'e**taşıyın.

    ![Erişim incelemesiekleme](./media/entitlement-management-access-reviews/access-reviews-pane.png)

1. İncelemelerin **Başlangıç'ta**başlayıp sonra başlayacağı tarihi belirtin.

1. Ardından, **Gözden Geçirme sıklığını** **Yıllık,** **İki yıllık,** **Üç Aylık** veya **Aylık**olarak ayarlayın.
Bu ayar, erişim incelemeleri ne sıklıkta oluşacağını belirler.

1. Yinelenen serinin her incelemesinin gözden geçirenlerin girişine kaç gün açık olacağını tanımlamak için **Süreyi** ayarlayın. Örneğin, 1 Ocak'ta başlayan ve gözden geçirenlerin yanıt vermek için ay sonuna kadar süreleri olması için 30 gün boyunca gözden geçirilebilen yıllık bir gözden geçirme zamanlayabilirsiniz.

1. Gözden **Geçirenlerin**yanında, kullanıcıların kendi erişim incelemelerini gerçekleştirmelerini istiyorsanız **Self-review'ı** seçin veya bir gözden geçiren belirlemek istiyorsanız **Belirli gözden geçiren(ler)'i** seçin.

    ![Gözden Geçirenler Ekle'yi seçin](./media/entitlement-management-access-reviews/access-reviews-add-reviewer.png)

1. **Belirli gözden geçiren(ler)'i**seçtiyseniz, erişim incelemesini hangi kullanıcıların yapacağını belirtin:
    1. **Gözden geçirenler ekle'yi**seçin.
    1. Gözden **geçirenleri Seç** bölmesinde, gözden geçiren olmak istediğiniz kullanıcı(lar)ı arayın ve seçin.
    1. Gözden geçireninizi (ler) seçtiğinizde, **Seç** düğmesini tıklatın.

    ![Gözden geçirenleri belirtin](./media/entitlement-management-access-reviews/access-reviews-select-reviewer.png)

1. Sayfanın alt kısmında yeni bir erişim paketi oluşturuyorsanız **Gözden Geçir + Oluştur'u** veya bir erişim paketini düzenliyorsanız **Güncelleştir'i** tıklatın.

## <a name="view-the-status-of-the-access-review"></a>Erişim incelemesinin durumunu görüntüleme

Başlangıç tarihinden sonra, **Erişim incelemeleri** bölümünde bir erişim incelemesi listelenir. Erişim incelemesinin durumunu görüntülemek için aşağıdaki adımları izleyin:

1. **Kimlik**Yönetimi'nde, **Access paketlerini** tıklatın ve ardından kontrol etmek istediğiniz erişim gözden geçirme durumuna sahip erişim paketini seçin.   

1. Erişim paketine genel bakışa çıktıktan sonra, sol menüdeki **yorumlara eriş'i** tıklatın.
    
    ![Erişim yorumlarını seçin](./media/entitlement-management-access-reviews/access-review-status-access-package-overview.png)

1. Bunlarla ilişkili erişim değerlendirmeleri olan tüm ilkeleri içeren bir liste görüntülenir. Raporunu görmek için gözden geçirmeyi tıklatın.

    ![Erişim incelemeleri listesi](./media/entitlement-management-access-reviews/access-review-status-select-access-reviews.png)
   
1. Raporu görüntülediğinizde, gözden geçirilen kullanıcı sayısını ve gözden geçiren tarafından bu rapor üzerinde gerçekleştirilen eylemleri gösterir.

    ![İnceleme durumunu görüntüleme](./media/entitlement-management-access-reviews/access-review-status.png)
 

## <a name="access-reviews-email-notifications"></a>E-posta bildirimlerini gözden geçirir
Gözden geçirenleri atayabilirsiniz veya kullanıcılar erişimlerini kendileri gözden geçirebilirsiniz. Varsayılan olarak, Azure AD inceleme başladıktan kısa bir süre sonra gözden geçirenlere veya kendi kendini gözden geçirenlere bir e-posta gönderir.

E-posta, erişim paketlerine erişimin nasıl gözden geçirileceklerine ilişkin yönergeler içerir. İnceleme, kullanıcıların erişimlerini gözden geçirmeleri içinse, onlara erişim paketlerinin kendi kendini gözden geçirmesini nasıl gerçekleştireceklerine ilişkin talimatları gösterin.
  
Konuk kullanıcıları gözden geçiren olarak atadıysanız ve Azure AD konuk davetlerini kabul etmedilerse, Azure AD erişim yorumlarından e-posta almazlar. E-postaları alabilmeleri için önce daveti kabul etmeli ve Azure AD'de bir hesap oluşturmaları gerekir. 

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim paketlerinin erişimini gözden geçirme](entitlement-management-access-reviews-review-access.md)
- [Erişim paketlerinin kendi kendine gözden geçirilmesi](entitlement-management-access-reviews-self-review.md)
