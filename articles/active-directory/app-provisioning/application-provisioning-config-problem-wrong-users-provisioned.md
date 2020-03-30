---
title: Yanlış kullanıcı kümesi bir Azure AD Galerisi uygulamasına uygulanır
description: Bir uygulamaya neden beklediğiniz lerden farklı bir kullanıcı kümesinin sağlandığını nasıl öğrenebilirsiniz
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: c94388011605da73666e82011bb8ef56d2af8d30
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77522790"
---
# <a name="wrong-set-of-users-are-being-provisioned-to-an-azure-ad-gallery-application"></a>Azure AD Galerisi uygulamasına yanlış kullanıcı kümesi sağlanıyor

Uygulamaiçin hangi kullanıcıların sağlandığı, öncelikle uygulamanın hangi kullanıcı ve grupların **atandığı** dır.

Azure Etkin Dizini'ndeki bir uygulamaya hangi kullanıcıların ve grupların atandığını denetlemek için aşağıdaki kaynakları kullanın.

## <a name="assign-a-user-directly-as-an-administrator"></a>Kullanıcıyı doğrudan yönetici olarak atama

Bir veya daha fazla kullanıcıyı doğrudan bir uygulamaya atamak için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

   * Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6. Listeden bir kullanıcı atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol daki gezinme menüsünden **Kullanıcılar ve Gruplar'ı** tıklatın.

8. **Atama Ekle** bölmesini açmak **için, Kullanıcılar ve Gruplar** listesinin üst kısmındaki **Ekle** düğmesini tıklatın.

9. **Atama Ekle** bölmesinden Kullanıcılar **ve gruplar** seçicisini tıklatın.

10. Ada veya **e-posta adresi** arama kutusuna atamak istediğiniz kullanıcının **tam adını** veya **e-posta adresini** yazın.

11. Bir **onay kutusunu**ortaya çıkarmak için listedeki **kullanıcının** üzerine taşırın. Kullanıcınızı **Seçili** listeye eklemek için kullanıcının profil fotoğrafının veya logosunun yanındaki onay kutusunu tıklatın.

12. **İsteğe bağlı:** **Birden fazla kullanıcı eklemek**istiyorsanız, **ada veya e-posta adresi** arama kutusuna başka bir tam **ad** veya **e-posta adresi** yazın ve bu kullanıcıyı **Seçili** listeye eklemek için onay kutusunu tıklatın.

13. Kullanıcıları seçmeyi bitirdiğinizde, bunları uygulamaya atanacak kullanıcı ve gruplar listesine eklemek için **Seç** düğmesini tıklatın.

14. **İsteğe bağlı:** Seçtiğiniz kullanıcılara atamak için bir rol seçmek için **Atama Ekle** bölmesinde **Rol** seçiyi seçin.

15. Uygulamayı seçili kullanıcılara atamak için **Atla** düğmesini tıklatın.

Sağlama yapılandırılmış ve bir uygulama için zaten çalışıyorsa, yeni kullanıcılar yaklaşık 10 dakika içinde bir uygulamaya sağlanmalıdır. Ayrıntılar için **Denetim Günlükleri'ni** kontrol edin.

## <a name="assign-a-group-directly-to-an-application-as-an-administrator"></a>Bir grubu doğrudan yönetici olarak bir uygulamaya atama

Bir uygulamaya doğrudan bir veya daha fazla grup atamak için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

   * Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6. Listeden bir kullanıcı atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol daki gezinme menüsünden **Kullanıcılar ve Gruplar'ı** tıklatın.

8. **Atama Ekle** bölmesini açmak **için, Kullanıcılar ve Gruplar** listesinin üst kısmındaki **Ekle** düğmesini tıklatın.

9. **Atama Ekle** bölmesinden Kullanıcılar **ve gruplar** seçicisini tıklatın.

10. **Ada veya e-posta adresi** arama kutusuna atamak istediğiniz grubun **tam grup adını** yazın.

11. Bir onay kutusunu ortaya çıkarmak için listedeki **grubun** üzerine titreyin. **checkbox** Kullanıcınızı **Seçili** listeye eklemek için grubun profil fotoğrafının veya logosunun yanındaki onay kutusunu tıklatın.

12. **İsteğe bağlı:** **Birden fazla grup eklemek**istiyorsanız, **ada veya e-posta adresi** arama kutusuna başka bir tam grup **adı** yazın ve bu grubu **Seçili** listeye eklemek için onay kutusunu tıklatın.

13. Grupları seçmeyi bitirdiğinizde, bunları uygulamaya atanacak kullanıcı ve gruplar listesine eklemek için **Seç** düğmesini tıklatın.

14. **İsteğe bağlı:** Seçtiğiniz gruplara atamak için bir rol seçmek için **Atama Ekle** bölmesinde **Rol** seçiyi seçin' i tıklatın.

15. Uygulamayı seçili gruplara atamak için **Atla** düğmesini tıklatın.

Sağlama yapılandırılmış ve bir uygulama için zaten çalışıyorsa, grup içinde bulunan yeni kullanıcılar yaklaşık 10 dakika içinde bir uygulamaya sağlanmalıdır. Ayrıntılar için **Denetim Günlükleri'ni** kontrol edin.

>[!IMPORTANT]
>Bazı uygulamalar için desteklenirse, üyelere ek olarak grup adı ve grup ayrıntılarının sağlanması. **Sağlama** sekmesinde gösterilen grup nesneleri için **Eşlemeyi** etkinleştirerek veya devre dışı bırakarak bu işlevselliği etkinleştirebilir veya devre dışı bırakabilirsiniz. 
>
>

Sağlama grupları etkinse, "eşleşen kimlik" için uygun bir alanın kullanıldığından emin olmak için öznitelik eşlemelerini gözden geçirin. Bu eşleşen kimlik, görüntü adı veya e-posta takma adı olabilir. Grup ve üyeleri, eşleşen özellik boşsa veya Azure AD'deki bir grup için doldurulmuyorsa sağlanmaz.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile SaaS Uygulamalarına Kullanıcı Sağlama ve Deprovisioning'i otomatikleştirin](user-provisioning.md)
