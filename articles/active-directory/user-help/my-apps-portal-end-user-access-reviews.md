---
title: Kuruluşunuzun uygulama & gruplarına erişimini yönetme-Azure AD
description: Uygulamalarım portalından Kuruluşunuzun uygulamalarına ve gruplarına yönelik güvenlik erişimini yönetmek için bir erişim incelemesi gerçekleştirmeyi öğrenin.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 3/21/2019
ms.author: lizross
ms.reviewer: kasimpso
ms.custom: user-help, seo-update-azuread-jan
ms.openlocfilehash: e739024686bdac497b9b7dd450c5ed46e3cf9a63
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76705029"
---
# <a name="perform-an-access-review-from-the-my-apps-portal"></a>Uygulamalarım portalından bir erişim incelemesi gerçekleştirme

İş veya okul hesabınızı Web tabanlı **My Apps** portalından kullanarak kuruluşunuzun bulut tabanlı uygulamalarının çoğunu görüntüleyip başlatabilir, profil ve hesap bilgilerinizin bazılarını güncelleştirebilir, **gruplarınızın** bilgilerini görebilir ve uygulamalarınız ve gruplarınız için **erişim gözden geçirmeleri** gerçekleştirebilirsiniz. **Uygulamalarım** portalına erişiminiz yoksa, Izin Için yardım masasına başvurmanız gerekir.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-my-apps-portal.md)]

>[!Important]
>Bu içerik kullanıcılara yöneliktir. Yöneticiyseniz, [uygulama yönetimi belgelerinde](https://docs.microsoft.com/azure/active-directory/manage-apps)bulut tabanlı uygulamalarınızın nasıl ayarlanacağı ve yönetileceği hakkında daha fazla bilgi edinebilirsiniz.

## <a name="manage-access-reviews"></a>Erişim incelemelerini yönetme

Yöneticiniz kendi erişim incelemelerinizi gerçekleştirme izni verdiyse, kendi **uygulamalarım** veya uygulamalar erişim gözden geçirmeleri sayfasındaki **erişim İncelemeleri** kutucuğundan gruplarınızı veya uygulamalarınızı yönetebilirsiniz.

>[!Note]
>**Erişim gözden geçirmeleri** kutucuğunu görmüyorsanız, erişim gözden geçirmeleri gerçekleştirme izniniz yok ya da Onayınızı bekleyen herhangi bir gözden geçirme onayınız yok demektir. Kutucuğa erişiminizin olması gerektiğini düşünüyorsanız yardım için yardım masasına başvurun.

### <a name="to-perform-your-access-reviews"></a>Erişim incelemelerinizi gerçekleştirmek için

1. İş veya okul hesabınızda oturum açın.

2. Web tarayıcınızı açın ve https://myapps.microsoft.com gidin veya kuruluşunuz tarafından sunulan bağlantıyı kullanın. Örneğin, kuruluşunuz için https://myapps.microsoft.com/contoso.com gibi özelleştirilmiş bir sayfaya yönlendirilmeyebilirsiniz.

    Kuruluşunuzun sahip olduğu ve kullanabileceğiniz bulut tabanlı uygulamaların tümünü gösteren **uygulamalar** sayfası görüntülenir.

    ![Uygulamalarım portalındaki uygulamalar sayfası](media/my-apps-portal/my-apps-portal-apps-page-access-review-tile.png)

3. Onayınızı bekleyen erişim gözden geçirmeleri listesini görmek için **erişim İncelemeleri** kutucuğunu seçin.

    ![Kuruluş için bekleyen erişim gözden geçirmeleri içeren erişim İncelemeleri sayfası](media/my-apps-portal/my-apps-portal-access-reviews-page.png)

4. Erişim incelemenizi başlatmak için **gözden geçirmeyi** Başlat ' ı seçin.

5. Erişiminizi gözden geçirin ve hâlâ gerekli olup olmadığını saptayın.

    ![İnceleme ayrıntılarını gösteren erişim gözden geçirme sayfası](media/my-apps-portal/my-apps-portal-perform-access-reviews-page.png)

    >[!Note]
    >Yöneticiyseniz ve kuruluşunuzun gruplara ve uygulamalara erişimini incelemeye izin verildiyse, farklı bir sayfa görürsünüz. Kuruluşunuz için grupları veya uygulamaları gözden geçirme hakkında daha fazla bilgi için bkz. [Azure AD erişim gözden geçirmeleriyle gruplara veya uygulamalara erişimi gözden geçirme](https://docs.microsoft.com/azure/active-directory/governance/perform-access-review).

6. Erişiminizi **kaldırmak için, erişiminizi korumak Için** **Evet** ' i seçin.

    **Evet**' i seçerseniz, **neden** kutusunda bir gerekçe belirtmeniz gerekebilir.

    ![Örnek metinle ilgili neden kutusunu gösteren erişim gözden geçirme sayfası](media/my-apps-portal/my-apps-portal-perform-access-reviews-reason-box.png)

7. Seçin **gönderme**.

    Erişim incelemeniz tamamlanmıştır ve **My Apps** portalına dönersiniz.

    >[!Note]
    >Erişiminizi, erişim gözden geçirme döneminizin bitene kadar istediğiniz zaman değiştirebilirsiniz. Bir uygulamaya veya gruba erişiminizi kaldırırsanız, bu, hemen kaldırılmaz. Kaldırma işlemi, erişim gözden geçirme süresi sona erdiğinde veya bir yönetici incelemeyi kapattığında gerçekleşir.

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulamalarım portalındaki uygulamalara erişin ve bunları kullanın](my-apps-portal-end-user-access.md).

- [Profil bilgilerinizi değiştirin](my-apps-portal-end-user-update-profile.md).

- [Gruplarla ilgili bilgilerinizi görüntüleyin ve güncelleştirin](my-apps-portal-end-user-groups.md).
