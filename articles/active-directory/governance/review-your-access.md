---
title: Erişim incelemelerinde gruplara & uygulamalara erişiminizi gözden geçirin - Azure AD
description: Azure Active Directory erişim incelemelerinde gruplara veya uygulamalara kendi erişiminizi nasıl inceleyiş olarak inceleyiş gösterebilirsiniz.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75422396"
---
# <a name="review-access-for-yourself-to-groups-or-applications-in-azure-ad-access-reviews"></a>Azure AD erişim incelemelerinde gruplara veya uygulamalara kendiniz erişimi gözden geçirin

Azure Etkin Dizin (Azure AD), kuruluşların Azure AD ve diğer Microsoft Çevrimiçi Hizmetleri'ndeki gruplara veya uygulamalara erişimi Azure AD erişim incelemeleri adı verilen bir özellik ile nasıl yönettiğini kolaylaştırır.

Bu makalede, bir gruba veya uygulamaya kendi erişiminizi nasıl gözden geçireceğiniz açıklanmaktadır.

## <a name="open-the-access-review"></a>Erişim incelemesini açma

Erişim incelemesi gerçekleştirmek için ilk adım, erişim incelemesini bulmak ve açmaktır.

1. Microsoft'tan erişimi gözden geçirmenizi isteyen bir e-posta arayın. Burada bir gruba erişiminizi gözden geçirmek için örnek bir e-posta verilmiştir.

    ![Bir gruba erişiminizi gözden geçirmek için Microsoft'tan örnek e-posta](./media/review-your-access/access-review-email.png)

1. Erişim incelemesini açmak için **Access'i İncele** bağlantısını tıklatın.

E-postanız yoksa, bekleyen erişim yorumlarınızı aşağıdaki adımları izleyerek bulabilirsiniz.

1. adresindeki [https://myapps.microsoft.com](https://myapps.microsoft.com)MyApps portalında oturum açın.

    ![MyApps portal listeleme uygulamaları size izinleri var](./media/review-your-access/myapps-access-panel.png)

1. Sayfanın sağ üst köşesinde yer alan ve adınızla varsayılan kuruluşunuzun gösterildiği kullanıcı simgesine tıklayın. Listede birden fazla kuruluş varsa erişim gözden geçirmesi isteğinde bulunan kuruluşu seçin.

1. Sayfanın sağ tarafında, bekleyen erişim yorumlarının listesini görmek için **Access incelemeleri** döşemesini tıklatın.

    Kutucuk yoksa ilgili kuruluş için bekleyen erişim gözden geçirmesi yoktur ve herhangi bir işlem yapmanız gerekmez.

    ![Uygulamalarınız ve gruplarınız için bekleyen erişim incelemeleri listesi](./media/review-your-access/access-reviews-list.png)

1. Gerçekleştirmek istediğiniz erişim incelemesi için **Incelemeyi Başlat** bağlantısını tıklatın.

## <a name="perform-the-access-review"></a>Erişim incelemesini gerçekleştirme

Erişim incelemesini açtıktan sonra erişiminizi görebilirsiniz.

1. Erişiminizi gözden geçirin ve hala erişime ihtiyacınız olup olmadığına karar verin.

    İstek başkaları için erişimi gözden geçirmek seise, sayfa farklı görünür. Daha fazla bilgi için [bkz.](perform-access-review.md)

    ![Bir gruba hala erişmeniz gerekip gerekmediğini soran açık erişim incelemesi](./media/review-your-access/perform-access-review.png)

1. Erişiminizi korumak için **Evet'i** tıklatın veya erişiminizi kaldırmak için **Hayır'ı** tıklatın.

1. **Evet'i**tıklattığınızda, **Neden** kutusunda bir yaslama belirtmeniz gerekebilir.

    ![Bir gruba hala erişmeniz gerekip gerekmediğini soran tamamlanmış erişim incelemesi](./media/review-your-access/perform-access-review-submit.png)

1. **Gönder'i**tıklatın.

    Seçiminiz gönderilir ve MyApps portalına geri dönersiniz.

    Yanıtınızı değiştirmek istiyorsanız, erişim incelemeleri sayfasını yeniden açın ve yanıtınızı güncelleyin. Erişim incelemesi sona erene kadar yanıtınızı istediğiniz zaman değiştirebilirsiniz.

    > [!NOTE]
    > Artık erişime ihtiyacınız olmadığını belirttiyseniz, hemen kaldırılmaz. İnceleme sona erdiğinde veya yönetici incelemeyi durdurduğunda kaldırılırsınız.

## <a name="next-steps"></a>Sonraki adımlar

- [Grupların veya uygulamaların erişim gözden geçirmesini tamamlama](complete-access-review.md)
