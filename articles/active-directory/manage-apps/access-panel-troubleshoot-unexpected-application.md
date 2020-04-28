---
title: Uygulamalar erişim panelinde nasıl görünür? | Microsoft Docs
description: Uygulamanın erişim panelinde neden görüneceği sorunlarını giderme
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: mimart
ms.reviewr: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8fa8ea75cc7fda05326c802c25a91d025b66b5ce
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "65784406"
---
# <a name="how-applications-appear-on-the-access-panel"></a>Uygulamalar erişim panelinde nasıl görünür?

Erişim paneli, Azure Active Directory (Azure AD) ' de iş veya okul hesabı olan bir kullanıcının Azure AD yöneticisinin erişim izni verdiği bulut tabanlı uygulamaları görüntülemesini ve başlatmasını sağlayan Web tabanlı bir portaldır. Bu uygulamalar, Azure AD portalındaki Kullanıcı adına yapılandırılır. Yönetici, uygulamayı kullanıcıya doğrudan veya kullanıcının erişim panelinde görünen uygulamanın bir parçası olan bir gruba sağlayabilir.

## <a name="general-issues-to-check-first"></a>Önce denetlenecek genel sorunlar

-   Bir uygulama, Kullanıcı üyesi olan bir kullanıcı veya gruptan kaldırılmışsa, uygulamanın kaldırılıp kaldırılmadığını görmek için birkaç dakika sonra kullanıcının erişim paneline yeniden oturum açmayı deneyin.

-   Bir kullanıcı veya gruptan bir lisans kaldırılmışsa, Kullanıcı bu grubun bir üyesi olduğundan, değişikliklerin yapılabilmesi için grubun boyutuna ve karmaşıklığına bağlı olarak bu işlem uzun zaman alabilir. Erişim panelinde oturum açmadan önce ek zamana izin verin.

## <a name="problems-related-to-assigning-applications-to-users"></a>Kullanıcılara uygulama atama ile ilgili sorunlar

Bir Kullanıcı daha önce kendisine atanmış olduğu için erişim panelinde bir uygulama görüyor olabilir. Aşağıda denetlenecek bazı yollar verilmiştir:

-   [Bir kullanıcının uygulamaya atanıp atanmadığını denetleyin](#check-if-a-user-is-assigned-to-the-application)

-   [Kullanıcının uygulamayla ilgili bir lisansın altında olup olmadığını denetleyin](#check-if-a-user-is-under-a-license-related-to-the-application)


### <a name="check-if-a-user-is-assigned-to-the-application"></a>Bir kullanıcının uygulamaya atanıp atanmadığını denetleyin

Bir kullanıcının uygulamaya atanıp atanmadığını denetlemek için şu adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

6. Söz konusu uygulamanın adını **arayın** .

7. **Kullanıcılar ve gruplar**' a tıklayın.

8. Kullanıcının uygulamaya atanıp atanmadığından emin olun.

   * Kullanıcıyı uygulamadan kaldırmak istiyorsanız, kullanıcının satırına tıklayın ve **Sil** **' i** seçin.

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a>Kullanıcının uygulamayla ilgili bir lisansın altında olup olmadığını denetleyin

Bir kullanıcının atanmış lisanslarını denetlemek için şu adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Gezinti menüsünde **Kullanıcılar ve gruplar** ' a tıklayın.

5. **tüm kullanıcılar**' a tıklayın.

6. İlgilendiğiniz kullanıcıyı **arayın** ve seçilecek **satıra tıklayın** .

7. kullanıcının şu anda hangi lisansları atandığını görmek için **lisanslar** ' a tıklayın.

   * Kullanıcı bir Office lisansına atanırsa, bu, birinci taraf Office uygulamalarının kullanıcının erişim panelinde görünmesini sağlar.

## <a name="problems-related-to-assigning-applications-to-groups"></a>Gruplara uygulama atama ile ilgili sorunlar

Bir Kullanıcı, uygulamanın atandığı bir grubun parçası olduklarından, erişim panelinde bir uygulama görüyor olabilir. Aşağıda denetlenecek bazı yollar verilmiştir:

-   [Kullanıcının grup üyeliklerini denetleme](#check-a-users-group-memberships)

-   [Kullanıcının bir lisansa atanan bir grubun üyesi olup olmadığını denetleyin](#check-if-a-user-is-a-member-of-a-group-assigned-to-a-license)

### <a name="check-a-users-group-memberships"></a>Kullanıcının grup üyeliklerini denetleme

Bir grubun üyeliğini denetlemek için şu adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Gezinti menüsünde **Kullanıcılar ve gruplar** ' a tıklayın.

5. **tüm kullanıcılar**' a tıklayın.

6. İlgilendiğiniz kullanıcıyı **arayın** ve seçilecek **satıra tıklayın** .

7. Gruplar ' a tıklayın **.**

8. Kullanıcının uygulamaya atanan bir grubun parçası olup olmadığını denetleyin.

   * Kullanıcıyı gruptan kaldırmak istiyorsanız, grubun **satırına tıklayın ve Sil ' i** seçin.

### <a name="check-if-a-user-is-a-member-of-a-group-assigned-to-a-license"></a>Kullanıcının bir lisansa atanan bir grubun üyesi olup olmadığını denetleyin

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Gezinti menüsünde **Kullanıcılar ve gruplar** ' a tıklayın.

5. **tüm kullanıcılar**' a tıklayın.

6. İlgilendiğiniz kullanıcıyı **arayın** ve seçilecek **satıra tıklayın** .

7. Gruplar ' a tıklayın **.**

8. belirli bir grubun satırına tıklayın.

9. grubun hangi lisansa atandığını görmek için **lisanslar** ' a tıklayın.

   * Grup bir Office lisansına atanırsa, bu, bazı birinci taraf Office uygulamalarının kullanıcının erişim panelinde görünmesini sağlayabilir.


## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Bu sorun giderme adımları sorunu gidermezse

varsa, aşağıdaki bilgilerle bir destek bileti açın:

-   Bağıntı hata KIMLIĞI

-   UPN (Kullanıcı e-posta adresi)

-   Kiracı Kimliği

-   Tarayıcı türü

-   Hata sırasında saat dilimi ve zaman/zaman dilimi

-   Fiddler izlemeleri

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](what-is-application-management.md)
