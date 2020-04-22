---
title: Self servis uygulama ataması nasıl yapılandırılır | Microsoft Dokümanlar
description: Kullanıcıların kendi uygulamalarını bulmalarını sağlamak için self servis uygulama erişimini etkinleştirin
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
ms.date: 04/20/2020
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 89e09fd63ee6121ac9bf7f3c2be00f0ac22f752f
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81731701"
---
# <a name="how-to-configure-self-service-application-assignment"></a>Self servis uygulama ataması nasıl yapılandırılır?

Kullanıcılarınız Uygulamalarım erişim panelinden uygulamaları kendi kendine keşfedebilmeniz için, kullanıcıların kendi kendine keşfetmesine ve erişim istemesine izin vermek istediğiniz uygulamalara **Self servis uygulama erişimi** sağlamanız gerekir. Bu işlev, [Azure AD Galerisi,](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)Azure AD [Uygulama Proxy'sinden](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy) eklenen veya [kullanıcı veya yönetici onayı](https://docs.microsoft.com/azure/active-directory/develop/application-consent-experience)ile eklenen uygulamalar için kullanılabilir. 

Bu özellik, BT grubu olarak zamandan ve paradan tasarruf etmenizi sağlamak için harika bir yoldur ve Azure Active Directory ile modern uygulama dağıtımının bir parçası olarak önerilir.

Bu özelliği kullanarak şunları yapabilirsiniz:

-   Kullanıcıların BT grubunu rahatsız etmeden [Uygulamalarım erişim panelindeki](https://myapps.microsoft.com/) uygulamaları kendi kendine keşfetmelerine izin verin.

-   Bu kullanıcıları önceden yapılandırılmış bir gruba ekleyin, böylece kimlerin erişim istediğini, erişimi kaldırdığını ve kendilerine atanan rolleri yönetebileceğini görebilirsiniz.

-   İsteğe bağlı olarak, BT grubunun onaylamasıgerekmemesi için bir iş onaylayıcısının uygulama erişim isteklerini onaylamasına izin verin.

-   İsteğe bağlı olarak, bu uygulamaya erişimi onaylayacak en fazla 10 kişiyi yapılandırın.

-   İsteğe bağlı olarak, bir işletme onaylayıcısının, iş onaylayıcısının Uygulama Erişim Paneli'nden, [Application Access Panel](https://myapps.microsoft.com/)bu kullanıcıların uygulamada oturum açmada kullanabileceği parolaları ayarlamasına izin verin.

-   İsteğe bağlı olarak otomatik olarak atanan kullanıcıları doğrudan bir uygulama rolüne atayın.

> [!NOTE]
> Kullanıcıların bir self servis uygulamasına katılmak için istekte bulunmaları ve sahiplerinin istekleri onaylaması veya reddetmesi için Azure Active Directory Premium (P1 veya P2) lisansı gereklidir. Azure Active Directory Premium lisansı olmadan, kullanıcılar self servis uygulamalar ekleyemez.

## <a name="enable-self-service-application-access-to-allow-users-to-find-their-own-applications"></a>Kullanıcıların kendi uygulamalarını bulmalarını sağlamak için self servis uygulama erişimini etkinleştirin

Self servis uygulama erişimi, kullanıcıların uygulamaları kendi kendine keşfetmelerine ve isteğe bağlı olarak iş grubunun bu uygulamalara erişimi onaylamasına izin vermek için harika bir yoldur. Uygulamalarda parola tek oturum açma için, iş grubunun bu kullanıcılara atanan kimlik bilgilerini kendi Uygulamalarım erişim panellerinden yönetmesine de izin verebilirsiniz.

Bir uygulamaya self servis uygulama erişimini etkinleştirmek için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com) Global Administrator olarak oturum açın.

2. **Azure Etkin Dizini'ni**seçin. Soldaki gezinti menüsünde **Kurumsal uygulamaları**seçin.

3. Listeden uygulamayı seçin. Uygulamayı görmüyorsanız, adını arama kutusuna yazmaya başlayın. Veya uygulama türünü, durumunu veya görünürlüğünü seçmek için filtre denetimlerini kullanın ve sonra **Uygula'yı**seçin.

4. Soldaki gezinti menüsünde **Self servis'i**seçin.

5. Bu uygulama için Self servis uygulama erişimini etkinleştirmek için, kullanıcıların bu uygulamaya **Yes.** **erişim isteğinde bulunmalarına izin ver'i açın?**

6. Atanan kullanıcıların yanına **hangi gruba eklenmelidir?** **Select group** Bir grup seçin ve sonra **Seç'i**tıklatın. Bir kullanıcının isteği onaylandığında, bu gruba eklenir. Bu grubun üyeliğini görüntülerken, self servis erişimi aracılığıyla uygulamaya kimlerin erişildiğini görebilirsiniz.
  
    > [!NOTE]
    > Bu ayar, şirket içinde eşitlenen grupları desteklemez.

7. **İsteğe bağlı:** Kullanıcılara erişim izni verilmeden önce işletme onayı istemek **için, bu uygulamaya erişim izni vermeden önce** **Onay**İste'yi ayarlayın?

8. **İsteğe bağlı: Yalnızca parola tek oturum açma kullanan uygulamalarda,** iş onaylayanların onaylı kullanıcılar için bu uygulamaya gönderilen parolaları belirtmesine izin vermek **için, onaylayıcıları bu uygulama için kullanıcı parolalarını ayarlamak için izin verin?** **Yes**

9. **İsteğe bağlı:** Bu uygulamaya erişimi onaylamasına izin verilen iş onaylayıcılarını belirtmek için, bu uygulamaya erişimi **Select approvers** **kimlerin onaylamasına izin verilir?** Ardından **Seç**'e tıklayın.

    >[!NOTE]
    >Gruplar desteklenmez. En fazla 10 ayrı iş onaylayıcı seçebilirsiniz. Birden çok onaylayıcı belirtirseniz, herhangi bir tek onaylayıcı bir erişim isteğini onaylayabilir.

10. **İsteğe bağlı:** **Rolleri ortaya çıkaran uygulamalariçin**, self servis onaylı kullanıcıları bir role atamak için, bu **Select Role**uygulamada kullanıcıların hangi role **atanması gerektiğinin yanında?** Ardından **Seç**'e tıklayın.

11. Bitirmek için bölmenin üst kısmındaki **Kaydet** düğmesini tıklatın.

Self servis uygulama yapılandırmasını tamamladıktan sonra, kullanıcılar [Uygulamalarım erişim paneline](https://myapps.microsoft.com/) gidebilir ve self servis erişimi yle etkinleştirilen uygulamaları bulmak için **self servis uygulamaları ekle** düğmesini tıklatabilir. İş onaylayanlar ayrıca [Uygulamalarım erişim panelinde](https://myapps.microsoft.com/)bir bildirim görürler. Bir kullanıcı onayını gerektiren bir uygulamaya erişim istediğinde, bir e-postayı bunları bildiren bir e-postayı etkinleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Self servis grup yönetimi için Azure Active Directory'yi ayarlama](../users-groups-roles/groups-self-service-management.md)
