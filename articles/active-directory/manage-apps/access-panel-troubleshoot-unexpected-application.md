---
title: Uygulamalar erişim panelinde nasıl görünür | Microsoft Dokümanlar
description: Access Paneli'nde bir uygulamanın neden göründüğünü giderme
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65784406"
---
# <a name="how-applications-appear-on-the-access-panel"></a>Uygulamalar erişim panelinde nasıl görünür?

Erişim Paneli, Azure Etkin Dizini'nde (Azure AD) çalışan bir kullanıcının Azure AD yöneticisinin kendilerine erişim izni verdiği bulut tabanlı uygulamaları görüntülemesini ve başlatmasını sağlayan web tabanlı bir portaldır. Bu uygulamalar Azure AD portalında kullanıcı adına yapılandırılır. Yönetici, uygulamayı doğrudan kullanıcıya veya kullanıcının kullanıcının Access Paneli'nde görünmesine neden olan bir gruba sağlayabilir.

## <a name="general-issues-to-check-first"></a>Önce kontrol etmek için genel konular

-   Bir uygulama kullanıcının üyesi olduğu bir kullanıcıdan veya gruptan kaldırıldıysa, uygulamanın kaldırılıp kaldırılıp kaldırılıp kaldırılmadı sını görmek için birkaç dakika sonra kullanıcının Access Paneli'nde oturum açma ve çıkma yapmayı deneyin.

-   Bir lisans bir kullanıcıdan veya gruptan kaldırıldıysa, kullanıcı bu kullanıcının üyesiyse, yapılacak değişikliklerin boyutuna ve karmaşıklığına bağlı olarak bu işlem uzun sürebilir. Erişim Paneli'nde oturum açmadan önce ek süre bekleyin.

## <a name="problems-related-to-assigning-applications-to-users"></a>Kullanıcılara uygulama atama ile ilgili sorunlar

Bir kullanıcı, daha önce atanmış olduğundan, Access Paneli'nde bir uygulama görüyor olabilir. Aşağıdakileri denetlemenin bazı yolları şunlardır:

-   [Uygulamaya bir kullanıcıatanıp atanmadı denetleme](#check-if-a-user-is-assigned-to-the-application)

-   [Kullanıcının uygulamayla ilgili bir lisans altında olup olmadığını denetleme](#check-if-a-user-is-under-a-license-related-to-the-application)


### <a name="check-if-a-user-is-assigned-to-the-application"></a>Uygulamaya bir kullanıcıatanıp atanmadı denetleme

Uygulamaya bir kullanıcı atanıp atanmadı kontrol etmek için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

6. Söz konusu uygulamanın adını **arayın.**

7. **Kullanıcılar ve gruplar'ı**tıklatın.

8. Kullanıcınızın uygulamaya atanıp atanmadı olup olmadığını kontrol edin.

   * Kullanıcıyı uygulamadan kaldırmak istiyorsanız, **kullanıcının satırını tıklatın** ve **sil'i**seçin.

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a>Kullanıcının uygulamayla ilgili bir lisans altında olup olmadığını denetleme

Bir kullanıcının atanmış lisanslarını denetlemek için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5. **tüm kullanıcıları**tıklatın.

6. İlgilendiğiniz kullanıcıyı **arayın** ve seçmek için **satırı tıklatın.**

7. kullanıcının şu anda hangi lisansları atadığını görmek için **Lisanslar'ı** tıklatın.

   * Kullanıcı bir Office lisansına atanmışsa, bu, Birinci Taraf Office uygulamalarının kullanıcının Erişim Paneli'nde görünmesini sağlar.

## <a name="problems-related-to-assigning-applications-to-groups"></a>Gruplara uygulama atanması ile ilgili sorunlar

Kullanıcı, uygulamaya atanan bir grubun parçası olduğundan, Access Paneli'nde bir uygulama görüyor olabilir. Aşağıdakileri denetlemenin bazı yolları şunlardır:

-   [Kullanıcının grup üyeliklerini kontrol edin](#check-a-users-group-memberships)

-   [Kullanıcının lisansa atanmış bir grubun üyesi olup olmadığını denetleme](#check-if-a-user-is-a-member-of-a-group-assigned-to-a-license)

### <a name="check-a-users-group-memberships"></a>Kullanıcının grup üyeliklerini kontrol edin

Bir grubun üyeliğini denetlemek için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5. **tüm kullanıcıları**tıklatın.

6. İlgilendiğiniz kullanıcıyı **arayın** ve seçmek için **satırı tıklatın.**

7. **Gruplar'ı tıklatın.**

8. Kullanıcınızın uygulamaya atanan bir Grubun parçası olup olmadığını kontrol edin.

   * Kullanıcıyı gruptan kaldırmak istiyorsanız, grubun **satırını tıklatın** ve sil'i seçin.

### <a name="check-if-a-user-is-a-member-of-a-group-assigned-to-a-license"></a>Kullanıcının lisansa atanmış bir grubun üyesi olup olmadığını denetleme

1. Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5. **tüm kullanıcıları**tıklatın.

6. İlgilendiğiniz kullanıcıyı **arayın** ve seçmek için **satırı tıklatın.**

7. **Gruplar'ı tıklatın.**

8. belirli bir grubun satırını tıklatın.

9. grubun hangi lisansları ona atadığını görmek için **Lisanslar'ı** tıklatın.

   * Grup bir Office lisansına atanmışsa, bu, bazı Birinci Taraf Office uygulamalarının kullanıcının Erişim Paneli'nde görünmesini sağlayabilir.


## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Bu sorun giderme adımları sorunu çözmüyorsa

varsa aşağıdaki bilgileri içeren bir destek bileti açın:

-   Korelasyon hatası kimliği

-   UPN (kullanıcı e-posta adresi)

-   Kiracı Kimliği

-   Tarayıcı türü

-   Hata oluşur sırasında saat dilimi ve saat/zaman dilimi oluşur

-   Kemancı izleri

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](what-is-application-management.md)
