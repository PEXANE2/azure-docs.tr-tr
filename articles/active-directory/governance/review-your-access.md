---
title: Erişim incelemeleriyle & uygulamalara erişiminizi gözden geçirme-Azure AD
description: Azure Active Directory erişim gözden geçirmeleriyle gruplara veya uygulamalara kendi erişiminizi incelemeyi öğrenin.
services: active-directory
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/21/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d75d7ffe28fa126ee5e359cba19e66e3c0f36e2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75422396"
---
# <a name="review-access-for-yourself-to-groups-or-applications-in-azure-ad-access-reviews"></a>Azure AD erişim incelemeleriyle ilgili grupları veya uygulamaları kendiniz için erişimi gözden geçirin

Azure Active Directory (Azure AD), kuruluşların Azure AD erişim gözden geçirmeleri adlı bir özellik ile Azure AD ve diğer Microsoft çevrimiçi hizmetlerindeki gruplara veya uygulamalara erişimi nasıl yöneteceğini basitleştirir.

Bu makalede, bir grup veya uygulamaya kendi erişiminizi nasıl gözden geçibileceğinizi açıklar.

## <a name="open-the-access-review"></a>Erişim gözden geçirmesini açın

Erişim gözden geçirmesi gerçekleştirmeye yönelik ilk adım, erişim incelemesini bulup açmak.

1. Microsoft 'un, erişimi incelemenizi isteyen bir e-posta bulun. Bir gruba erişiminizi gözden geçirmek için örnek bir e-posta aşağıda verilmiştir.

    ![Bir gruba erişiminizi gözden geçirmek için Microsoft 'tan örnek e-posta](./media/review-your-access/access-review-email.png)

1. Erişim gözden geçirmesini açmak için **erişimi gözden geçir** bağlantısına tıklayın.

E-postanız yoksa, bekleyen erişim incelemelerinizi aşağıdaki adımları izleyerek bulabilirsiniz.

1. Konumundaki [https://myapps.microsoft.com](https://myapps.microsoft.com)uygulamaps portalında oturum açın.

    ![Uygulamaps portalı izinleriniz olan uygulamaları listeleme](./media/review-your-access/myapps-access-panel.png)

1. Sayfanın sağ üst köşesinde yer alan ve adınızla varsayılan kuruluşunuzun gösterildiği kullanıcı simgesine tıklayın. Listede birden fazla kuruluş varsa erişim gözden geçirmesi isteğinde bulunan kuruluşu seçin.

1. Sayfanın sağ tarafında, bekleyen erişim incelemelerinin listesini görmek için **erişim İncelemeleri** kutucuğuna tıklayın.

    Kutucuk yoksa ilgili kuruluş için bekleyen erişim gözden geçirmesi yoktur ve herhangi bir işlem yapmanız gerekmez.

    ![Uygulamalarınız ve gruplarınız için bekleyen erişim İncelemeleri listesi](./media/review-your-access/access-reviews-list.png)

1. Gerçekleştirmek istediğiniz erişim incelemesi için **gözden geçirmeyi Başlat** bağlantısına tıklayın.

## <a name="perform-the-access-review"></a>Erişim gözden geçirmesini gerçekleştir

Erişim gözden geçirmesini açtıktan sonra erişiminizi görebilirsiniz.

1. Erişiminizi gözden geçirin ve hâlâ erişime ihtiyacınız olup olmadığına karar verin.

    İstek başkalarının erişimini gözden geçirmektir, sayfa farklı görünür. Daha fazla bilgi için bkz. [gruplara veya uygulamalara erişimi gözden geçirme](perform-access-review.md).

    ![Hala bir gruba erişmeniz gerekip gerekmediğini soran açık erişim gözden geçirme](./media/review-your-access/perform-access-review.png)

1. Erişiminizi korumak için **Evet** ' e tıklayın veya erişiminizi kaldırmak için **Hayır** ' a tıklayın.

1. **Evet**' e tıklarsanız, **neden** kutusunda bir gerekçe belirtmeniz gerekebilir.

    ![Bir gruba erişmeniz gerekip gerekmediğini soran tamamlanmış erişim incelemesi](./media/review-your-access/perform-access-review-submit.png)

1. **Gönder**' e tıklayın.

    Seçiminiz gönderilir ve Uygıps portalına dönersiniz.

    Yanıtınızı değiştirmek istiyorsanız, erişim İncelemeleri sayfasını yeniden açın ve yanıtınızı güncelleştirin. Erişim incelemesi sonlanana kadar yanıtınızı dilediğiniz zaman değiştirebilirsiniz.

    > [!NOTE]
    > Artık erişime ihtiyaç duymadığını belirttiyseniz, hemen kaldırılmamıştır. İnceleme sona erdiğinde veya bir yönetici incelemeyi durdurduğu zaman kaldırılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Grupların veya uygulamaların erişim incelemesini tamamlar](complete-access-review.md)
