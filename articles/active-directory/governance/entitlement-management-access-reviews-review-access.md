---
title: Azure AD yetkilendirme yönetiminde bir erişim paketinin erişimini gözden geçirme
description: Azure Active Directory erişim incelemelerinde (Önizleme) yetkilendirme yönetimi erişim paketlerinin erişim incelemesini nasıl tamamlayabileceğinizi öğrenin.
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
ms.openlocfilehash: 99de022b7259b33baab3aa825673a8f85e932bff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78968736"
---
# <a name="review-access-of-an-access-package-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde bir erişim paketinin erişimini gözden geçirme

Azure AD yetkilendirme yönetimi, işletmelerin gruplara, uygulamalara ve SharePoint sitelerine erişimi nasıl yönettiğini basitleştirir. Bu makalede, atanmış bir gözden geçiren olarak bir erişim paketine atanan diğer kullanıcılar için erişim incelemeleri nasıl gerçekleştirilirken açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Kullanıcıların etkin erişim paketi atamalarını gözden geçirmek için, bir erişim incelemesi yapmak için ön koşulları karşılamanız gerekir:
- Azure AD Premium P2
- Genel yönetici
- Atanmış Kullanıcı yöneticisi, Katalog sahibi veya Access paket yöneticisi

Daha fazla bilgi için Lisans [gereksinimlerine](entitlement-management-overview.md#license-requirements)bakın.


## <a name="open-the-access-review"></a>Erişim incelemesini açma

Erişim incelemesini bulmak ve açmak için aşağıdaki adımları kullanın:

1. Microsoft'tan erişimi gözden geçirmenizi isteyen bir e-posta alabilirsiniz. Erişim incelemesini açmak için e-postayı bulun. Erişimi gözden geçirmek için örnek bir e-posta aşağıda verilmiştir:
    
    ![İnceleme gözden geçirenin e-postaadresine erişin](./media/entitlement-management-access-reviews-review-access/review-access-reviewer-email.png)

1. Erişim incelemesini açmak için **Kullanıcı erişimini Gözden Geçir** bağlantısını tıklatın. 

1. E-postanız yoksa, bekleyen erişim yorumlarınızı doğrudan https://myaccess.microsoft.com.  (ABD Hükümeti için, bunun yerine kullanın.) `https://myaccess.microsoft.us`

1. Size atanan bekleyen erişim yorumlarının listesini görmek için sol daki gezinti çubuğundaki **Erişim yorumlarını** tıklatın.
    
    ![Erişimim'de erişim yorumlarını seçin](./media/entitlement-management-access-reviews-review-access/review-access-myaccess-select-access-review.png)

1. Başlamak istediğiniz incelemeyi tıklatın.
    
    ![Erişim incelemesini seçin](./media/entitlement-management-access-reviews-review-access/review-access-select-access-review.png)

## <a name="perform-the-access-review"></a>Erişim incelemesini gerçekleştirme

Erişim incelemesini açtıktan sonra, incelemeniz gereken kullanıcıların adlarını görürsünüz. Erişimi onaylamanın veya reddetmenin iki yolu vardır:
- Bir veya daha fazla kullanıcının erişimini el ile onaylayabilir veya reddedebilirsiniz
- Sistem önerilerini kabul edebilirsiniz

### <a name="manually-approve-or-deny-access-for-one-or-more-users"></a>Bir veya daha fazla kullanıcının erişimini el ile onaylama veya reddetme
1. Kullanıcı listesini gözden geçirin ve hangi kullanıcıların erişime devam etmesi gerektiğini belirleyin.

    ![İncelenecek kullanıcı listesi](./media/entitlement-management-access-reviews-review-access/review-access-list-of-users.png)

1. Erişimi onaylamak veya reddetmek için kullanıcının adının solundaki radyo düğmesini seçin.

1. Kullanıcı adlarının üzerindeki çubukta **Onayla** veya **Reddet'i** seçin.

    ![Kullanıcıyı seçin](./media/entitlement-management-access-reviews-review-access/review-access-select-users.png)

1. Emin değilseniz, **Bilmiyorum** düğmesini tıklatabilirsiniz.

    Bu seçimi yaparsanız, kullanıcı erişimi korur ve bu seçim denetim günlüklerine gider. Günlük, incelemeyi hala tamamladığınız diğer gözden geçirenleri gösterir.

1. Kararınız için bir neden sağlamanız gerekebilir. Bir neden yazın ve **Gönder'i**tıklatın.

    ![Erişimi onaylama veya reddetme](./media/entitlement-management-access-reviews-review-access/review-access-decision-approve.png)

1. İncelemenin bitiminden önce istediğiniz zaman kararınızı değiştirebilirsiniz. Bunu yapmak için, listeden kullanıcıyı seçin ve kararı değiştirin. Örneğin, daha önce reddettiğiniz bir kullanıcının erişimini onaylayabilirsiniz.

Birden çok gözden geçiren varsa, gönderilen son yanıt kaydedilir. Bir yöneticinin iki gözden geçiren -Gamze ve Bob- belirlediği bir örnek düşünün. Alice önce incelemeyi açar ve erişimi onaylar. İnceleme sona ermeden önce, Bob incelemeyi açar ve erişimi reddeder. Bu durumda, son erişim kararı kaydedilir.

>[!NOTE]
>Bir kullanıcının erişimi engellenirse, erişim paketinden hemen kaldırılmaz. İnceleme sona erdiğinde veya yönetici incelemeyi sona erdiğinde kullanıcı erişim paketinden kaldırılır.

### <a name="approve-or-deny-access-using-the-system-generated-recommendations"></a>Sistem tarafından oluşturulan önerileri kullanarak erişimi onaylama veya reddetme

Birden çok kullanıcının erişimini daha hızlı incelemek için, tek bir tıklamayla önerileri kabul eden sistem tarafından oluşturulan önerileri kullanabilirsiniz. Öneriler, kullanıcının oturum açma etkinliğine göre oluşturulur.

1.  Sayfanın üst kısmındaki çubukta **önerileri kabul et'i**tıklatın.
    
    ![Önerileri Kabul Et](./media/entitlement-management-access-reviews-review-access/review-access-use-recommendations.png)
    
    Önerilen eylemlerin bir özetini görürsünüz.

1.  Önerileri kabul etmek için **Gönder'i** tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim paketlerinin kendi kendine gözden geçirilmesi](entitlement-management-access-reviews-self-review.md)