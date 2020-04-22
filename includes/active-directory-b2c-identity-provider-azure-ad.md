---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 04/07/2020
ms.author: mimart
ms.openlocfilehash: 31a6d116ae1afce9afdd3786747490980963d823
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678062"
---
## <a name="register-an-azure-ad-app"></a>Azure AD uygulamasını kaydetme

Belirli bir Azure AD kuruluşundan kullanıcılar için oturum açmayı etkinleştirmek için, bir uygulamayı kuruluş Azure AD kiracısına kaydetmeniz gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Kuruluş azure AD kiracınızı içeren dizini kullandığınızdan emin olun (örneğin, contoso.com). Üst menüdeki **Dizin + abonelik filtresini** seçin ve ardından Azure AD kiracınızı içeren dizini seçin.
1. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından **Uygulama kayıtlarını**arayın ve seçin.
1. **Yeni kayıt**seçin.
1. Başvurunuz için bir **Ad** girin. Örneğin, `Azure AD B2C App`.
1. **Bu kuruluş dizinindeki Hesapların** varsayılan seçimini yalnızca bu uygulama için kabul edin.
1. Yeniden **Yönlendirme**URI'si için **Web**değerini kabul edin ve Azure `your-B2C-tenant-name` AD B2C kiracınızın adıyla değiştirilen tüm küçük harflere aşağıdaki URL'yi girin.

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Örneğin, `https://fabrikam.b2clogin.com/fabrikam.onmicrosoft.com/oauth2/authresp`.

1. **Kaydol**’u seçin. Uygulama **(istemci) kimliğini** daha sonraki bir adımda kullanmak üzere kaydedin.
1. **Sertifikalar & sırları**seçin ve ardından **Yeni istemci sırrını**seçin.
1. Gizli için **Açıklama** girin, bir son kullanma tarihi seçin ve sonra **Ekle'yi**seçin. Daha sonraki bir adımda kullanılmak üzere sırrın **Değerini** kaydedin.

### <a name="configuring-optional-claims"></a>İsteğe bağlı talepleri yapılandırma

Azure AD'den `family_name` ve `given_name` talepleri almak istiyorsanız, Uygulamanız için isteğe bağlı talepleri Azure portalı kullanıcı arabirimi veya uygulama bildiriminde yapılandırabilirsiniz. Daha fazla bilgi için Azure [AD uygulamanızda isteğe bağlı talepler nasıl sağlayabileceğinize](/active-directory/develop/active-directory-optional-claims.md)bakın.

1. [Azure Portal](https://portal.azure.com) oturum açın. **Azure Active Directory**'yi bulun ve seçin.
1. **Yönet** bölümünden **Uygulama kayıtlarını**seçin.
1. Listede isteğe bağlı talepleri yapılandırmak istediğiniz uygulamayı seçin.
1. **Yönet** bölümünden **Token yapılandırmasını**seçin.
1. **İsteğe bağlı talep ekle'yi**seçin.
1. **Token türü için** **kimlik**seçin.
1. Eklenecek isteğe bağlı `family_name` talepleri `given_name`seçin ve .
1. **Ekle**'ye tıklayın.