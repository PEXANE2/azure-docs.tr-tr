---
title: Erişim incelemelerinde gruplara & uygulamalara erişimi gözden geçirin - Azure AD
description: Azure Active Directory erişim incelemelerinde grup üyelerinin erişimini veya uygulama erişimini nasıl inceleyiş olarak inceleyiniz öğrenin.
services: active-directory
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 03/22/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: ee4125e82dd5176f01de294011e22a1d66005094
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80128461"
---
# <a name="review-access-to-groups-and-applications-in-azure-ad-access-reviews"></a>Azure AD erişim incelemelerinde gruplara ve uygulamalara erişimi gözden geçirin

Azure Etkin Dizin (Azure AD), kuruluşların Azure AD ve diğer Microsoft Çevrimiçi Hizmetleri'ndeki gruplara ve uygulamalara erişimi Azure AD erişim incelemeleri adı verilen bir özellik ile nasıl yönettiğini kolaylaştırır.

Bu makalede, atanmış bir gözden geçirenin bir grubun üyeleri veya bir uygulamaya erişimi olan kullanıcılar için nasıl bir erişim incelemesi gerçekleştirdiği açıklanmaktadır.

## <a name="open-the-access-review"></a>Erişim incelemesini açma

Erişim incelemesi gerçekleştirmek için ilk adım, erişim incelemesini bulmak ve açmaktır.

1. Microsoft'tan erişimi gözden geçirmenizi isteyen bir e-posta arayın. Burada bir grup için erişimi gözden geçirmek için örnek bir e-posta.

    ![Bir gruba erişimi incelemek için Microsoft'tan örnek e-posta](./media/perform-access-review/access-review-email.png)

1. Erişim **incelemesini** açmak için incelemeyi başlat bağlantısını tıklatın.

E-postanız yoksa, bekleyen erişim yorumlarınızı aşağıdaki adımları izleyerek bulabilirsiniz.

1. adresindeki [https://myapps.microsoft.com](https://myapps.microsoft.com)MyApps portalında oturum açın.

    ![MyApps portal listeleme uygulamaları size izinleri var](./media/perform-access-review/myapps-access-panel.png)

1. Sayfanın sağ üst köşesinde yer alan ve adınızla varsayılan kuruluşunuzun gösterildiği kullanıcı simgesine tıklayın. Listede birden fazla kuruluş varsa erişim gözden geçirmesi isteğinde bulunan kuruluşu seçin.

1. Bekleyen erişim incelemeleri listesini görmek için **Access incelemeleri** döşemesini tıklatın.

    Kutucuk yoksa ilgili kuruluş için bekleyen erişim gözden geçirmesi yoktur ve herhangi bir işlem yapmanız gerekmez.

    ![Uygulamalar ve gruplar için bekleyen erişim incelemeleri listesi](./media/perform-access-review/access-reviews-list.png)

1. Gerçekleştirmek istediğiniz erişim incelemesi için **Incelemeyi Başlat** bağlantısını tıklatın.

## <a name="perform-the-access-review"></a>Erişim incelemesini gerçekleştirme

Erişim incelemesini açtıktan sonra, gözden geçirilmesi gereken kullanıcıların adlarını görürsünüz.

İstek kendi erişiminizi gözden geçirmekse, sayfa farklı görünür. Daha fazla bilgi için [gruplara veya uygulamalara kendiniz erişimi gözden geçir'e](review-your-access.md)bakın.

![İncelenmesi gereken kullanıcıları listeleyen açık erişim incelemesi](./media/perform-access-review/perform-access-review.png)

Erişimi onaylamanın veya reddetmenin iki yolu vardır:

- Bir veya daha fazla kullanıcının erişimini onaylayabilir veya reddedebilir veya
- En kolay ve en hızlı yol olan sistem önerilerini kabul edebilirsiniz.

### <a name="approve-or-deny-access-for-one-or-more-users"></a>Bir veya daha fazla kullanıcının erişimini onaylama veya reddetme

1. Sürekli erişimlerini onaylayıp onaylamamaya veya reddetmeye karar vermek için kullanıcıların listesini gözden geçirin.

1. Tek bir kullanıcının erişimini onaylamak veya reddetmek için, yapılacak eylemi belirtmek için bir pencere açmak için satırı tıklatın. Birden çok kullanıcının erişimini onaylamak veya reddetmek için, kullanıcıların yanına onay işaretleri ekleyin ve ardından yapılacak eylemi belirtmek için bir pencere açmak için **X kullanıcı(lar) seçeneğini** gözden geçir düğmesini tıklatın.

1. Onayla veya **Reddet'i** tıklatın. **Deny** Emin değilseniz, **Bilmiyorum'a**tıklayabilirsiniz. Bunu yapmak, kullanıcının erişimini sürdürmesi ile sonuçlanır, ancak seçim denetim günlüklerine yansıtılır.

    ![Onaylama, Reddetme ve Bilme seçeneklerini içeren eylem penceresi](./media/perform-access-review/approve-deny.png)

1. Gerekirse, **Neden** kutusuna bir neden girin.

    Erişim incelemesinin yöneticisi, sürekli erişimi veya grup üyeliğini onaylamak için bir neden sağlamanızı gerektirebilir.

1. Yapılacak eylemi belirttikten sonra **Kaydet'i**tıklatın.

    Yanıtınızı değiştirmek istiyorsanız, satırı seçin ve yanıtı güncelleştirin. Örneğin, önceden reddedilen bir kullanıcıyı onaylayabilir veya önceden onaylanmış bir kullanıcıyı reddedebilirsiniz. Erişim incelemesi sona erene kadar yanıtınızı istediğiniz zaman değiştirebilirsiniz.

    Birden çok gözden geçiren varsa, gönderilen son yanıt kaydedilir. Bir yöneticinin iki gözden geçiren -Gamze ve Bob- belirlediği bir örnek düşünün. Alice önce erişim incelemesini açar ve erişimi onaylar. İnceleme sona ermeden önce, Bob erişim incelemesini açar ve erişimi reddeder. Son inkar yanıtı ne kaydedilir.

    > [!NOTE]
    > Bir kullanıcının erişimi engellenirse, bunlar hemen kaldırılmaz. Gözden geçirme sona erdiğinde veya yönetici incelemeyi durdurduğunda kaldırılırlar.

### <a name="approve-or-deny-access-based-on-recommendations"></a>Önerilere dayalı erişimi onaylama veya reddetme

Erişim yorumlarını sizin için daha kolay ve hızlı hale getirmek için, tek bir tıklamayla kabul edebileceğiniz öneriler de sayılabiliriz. Öneriler, kullanıcının oturum açma etkinliğine göre oluşturulur.

1. Sayfanın altındaki mavi çubukta önerileri **kabul et'i**tıklatın.

    ![Önerileri Kabul et düğmesini gösteren açık erişim gözden geçirme listesi](./media/perform-access-review/accept-recommendations.png)

    Önerilen eylemlerin bir özetini görürsünüz.

    ![Önerilen eylemlerin bir özetini görüntüleyen pencere](./media/perform-access-review/accept-recommendations-summary.png)

1. Önerileri kabul etmek için **Tamam'ı** tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

- [Grupların veya uygulamaların erişim gözden geçirmesini tamamlama](complete-access-review.md)
