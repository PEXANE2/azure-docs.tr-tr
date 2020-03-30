---
title: Kuruluşunuzun uygulamalara erişimini & gruplarına yönetme - Azure AD
description: Uygulamam portalından kuruluşunuzun uygulamaları ve grupları için güvenlik erişimini yönetmek için bir erişim incelemesini nasıl gerçekleştireceklerinizi öğrenin.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: curtand
ms.reviewer: kasimpso
ms.custom: user-help, seo-update-azuread-jan
ms.openlocfilehash: c71195b247af6d5046d88d3e6918a660eddf09b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77062396"
---
# <a name="perform-an-access-review-from-the-my-apps-portal"></a>Uygulamalarım portalından erişim incelemesi gerçekleştirin

Uygulamalarınız ve gruplarınız için erişim değerlendirmeleri gerçekleştirmek için web tabanlı **Uygulamalarım** portalındaki iş veya okul hesabınızı kullanabilirsiniz. Erişim incelemeleri, eski erişimi yönetmenize veya erişim gereksinimlerini değiştirmenize ve bunların gözden geçirilmesini ve güncelleştirilmesini sağlamanıza yardımcı olur.

**Uygulamalarım** portalına erişiminiz yoksa, izin almak için Yardım Masanıza başvurun.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-my-apps-portal.md)]

>[!Important]
>Bu içerik **Uygulamalarım** kullanıcıları için tasarlanmıştır. Yöneticiyseniz, [Uygulama Yönetimi Dokümantasyonu'nda](https://docs.microsoft.com/azure/active-directory/manage-apps)bulut tabanlı uygulamalarınızı nasıl ayarlayıp yönetebileceğiniz hakkında daha fazla bilgi bulabilirsiniz.

## <a name="manage-access-reviews"></a>Erişim yorumlarını yönetme

Yöneticiniz size kendi erişim yorumlarınızı gerçekleştirme izni verdiyse, Gruplarınızı veya uygulamalarınızın erişimini **Uygulamalarım** portalı sayfasındaki **Access incelemeleri** döşemesinden yönetebilirsiniz.

>[!Note]
>**Access incelemeleri** döşemesini görmüyorsanız, bu, erişim incelemeleri gerçekleştirmek için izniniz olmadığı veya onayınızı bekleyen yorumlarınız olmadığı anlamına gelir. Döşemeye erişiminiz olması gerektiğini düşünüyorsanız, yardım için Yardım Masanıza başvurun.

## <a name="to-perform-your-access-reviews"></a>Erişim yorumlarınızı gerçekleştirmek için

1. İş veya okul hesabınızda oturum açın.

2. Web tarayıcınızı açın https://myapps.microsoft.comve kuruluşunuz tarafından sağlanan bağlantıyı kullanın. Örneğin, kuruluşunuz için özelleştirilmiş bir sayfaya yönlendirilmiş https://myapps.microsoft.com/contoso.comolabilirsiniz.

    **Uygulamalar** sayfası, kuruluşunuz tarafından sahip olunan ve kullanabileceğiniz tüm bulut tabanlı uygulamaları gösterir.

    ![Uygulamalarım portalındaki uygulamalar sayfası](media/my-apps-portal/my-apps-portal-apps-page-access-review-tile.png)

3. Onayınızı bekleyen erişim yorumlarılistesini görmek için **Access incelemeleri** döşemesini seçin.

    ![Kuruluş için bekleyen erişim yorumlarıyla incelemeler sayfasına erişin](media/my-apps-portal/my-apps-portal-access-reviews-page.png)

4. Erişim **incelemenizi** başlatmak için Gözden Geçir'i Başlat'ı seçin.

5. Erişiminizi gözden geçirin ve hala gerekli olup olmadığını belirleyin.

    ![İnceleme ayrıntılarını gösteren inceleme sayfasına erişin](media/my-apps-portal/my-apps-portal-perform-access-reviews-page.png)

    >[!Note]
    >Yöneticiyseniz ve kuruluşunuzun gruplara ve uygulamalara erişimini gözden geçirmenize izin verilirse, farklı bir sayfa görürsünüz. Kuruluşunuz için grupları veya uygulamaları gözden geçirme hakkında daha fazla bilgi için Azure [AD Erişim İncelemeleri'ndeki gruplara veya uygulamalara erişimi gözden geçir'e](https://docs.microsoft.com/azure/active-directory/governance/perform-access-review)bakın.

6. Erişiminizi korumak için **Evet'i** veya erişiminizi kaldırmak için **Hayır'ı** seçin.

    **Evet'i**seçerseniz, **Neden** kutusunda bir yaslama belirtmeniz gerekebilir.

    ![Örnek metinle Neden kutusunu gösteren inceleme sayfasına erişin](media/my-apps-portal/my-apps-portal-perform-access-reviews-reason-box.png)

7. **Gönder**’i seçin.

    Erişim incelemeniz tamamlandı ve **Uygulamalarım** portalına geri dönüyorsunuz.

    >[!Note]
    >Erişim inceleme döneminiz sona erene kadar istediğiniz zaman erişiminizi değiştirebilirsiniz. Bir uygulamaya veya gruba erişiminizi kaldırırsanız, uygulama hemen kaldırılmaz. Kaldırma, erişim gözden geçirme dönemi sona erdiğinde veya yönetici gözden geçirmeyi kapattığında gerçekleşir.

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulamalarım portalındaki uygulamalara erişin ve bunları kullanın](my-apps-portal-end-user-access.md)
- [Profil bilgilerinizi değiştirme](my-apps-portal-end-user-update-profile.md)
- [Gruplarla ilgili bilgilerinizi görüntüleme ve güncelleme](my-apps-portal-end-user-groups.md)
