---
title: Azure Active Directory 'de bağlantılı oturum açmayı yapılandırma
description: Azure AD 'de bağlantılı oturum açmayı yapılandırın.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: how-to
ms.workload: identity
ms.date: 07/30/2020
ms.author: kenwith
ms.reviewer: arvinh,luleon
ms.openlocfilehash: 2269a8f7f58d35fee5e2ca30a77af5e8cba83678
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87459343"
---
# <a name="configure-linked-sign-on"></a>Bağlantılı oturum açmayı yapılandırma

Uygulama yönetiminde [hızlı başlangıç serisinde](view-applications-portal.md) , Azure AD 'yi bir uygulama Için kimlik sağlayıcısı (IDP) olarak kullanmayı öğrendiniz. Hızlı başlangıç kılavuzunda SAML tabanlı SSO 'yu ayarlarsınız. Başka bir seçenek **bağlandı**. Bu makale, bağlantılı seçenek hakkında daha fazla ayrıntıya gider.

**Bağlı** seçeneği, bir Kullanıcı kuruluşunuzun [uygulamalarım](https://myapplications.microsoft.com/) veya Office 365 portalından uygulamayı seçtiğinde hedef konumu yapılandırmanıza olanak tanır.

Bağlantı seçeneğinin değerli olduğu bazı yaygın senaryolar şunlardır:
- Active Directory Federasyon Hizmetleri (AD FS) (AD FS) gibi şu anda Federasyon kullanan özel bir Web uygulamasına bir bağlantı ekleyin.
- Belirli SharePoint sayfalarına veya yalnızca kullanıcının erişim panellerinde görünmesini istediğiniz diğer Web sayfalarına derin bağlantılar ekleyin.
- Kimlik doğrulaması gerektirmeyen bir uygulamaya bağlantı ekleyin. 
 
 **Bağlantılı** seçenek, Azure AD kimlik bilgileri aracılığıyla oturum açma işlevleri sağlamaz. Ancak, bazı **Kurumsal uygulama**özelliklerini kullanmaya devam edebilirsiniz. Örneğin, Denetim günlüklerini kullanabilir ve özel bir logo ve uygulama adı ekleyebilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Hızlı bir şekilde hızlandırmak için uygulama yönetiminde [hızlı başlangıç serisini](view-applications-portal.md) gözden geçirebilirsiniz. Çoklu oturum açmayı yapılandırdığınız hızlı başlangıçta **bağlantılı** seçeneği de bulacaksınız. 

**Bağlantılı** seçenek, Azure AD aracılığıyla oturum açma işlevleri sağlamaz. Bu seçenek yalnızca kullanıcıların [uygulamamda](https://myapplications.microsoft.com/) Microsoft 365 veya uygulama başlatıcısında uygulamayı seçerken gönderileceği konumu ayarlar.

> [!IMPORTANT] 
> **Çoklu oturum açma** seçeneğinin **Kurumsal uygulamalarda**bir uygulama için gezinmede olmadığı bazı senaryolar vardır. 
>
> Uygulama **uygulama kayıtları** kullanılarak kaydedilmişse, çoklu oturum açma özelliği varsayılan olarak OIDC OAuth kullanacak şekilde ayarlanır. Bu durumda, **Çoklu oturum açma** seçeneği, **Kurumsal uygulamalar**altındaki gezinmede gösterilmez. Özel uygulamanızı eklemek için **uygulama kayıtları** kullandığınızda, bildirim dosyasındaki seçenekleri yapılandırırsınız. Bildirim dosyası hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory uygulama bildirimi](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest). SSO standartları hakkında daha fazla bilgi edinmek için bkz. [Microsoft Identity platform kullanarak kimlik doğrulama ve yetkilendirme](https://docs.microsoft.com/azure/active-directory/develop/authentication-vs-authorization#authentication-and-authorization-using-microsoft-identity-platform). 
>
> Bir uygulamanın başka bir kiracıda barındırıldığı veya hesabınızın gerekli izinleri (genel yönetici, bulut uygulaması Yöneticisi, uygulama Yöneticisi veya hizmet sorumlusu sahibi) yoksa, **Çoklu oturum açma** 'nın gezinmede eksik olduğu diğer senaryolar. İzinler Ayrıca **Çoklu oturum** açmayı açabiliyor ancak kaydedemeyeceksiniz bir senaryoya neden olabilir. Azure AD Yönetim rolleri hakkında daha fazla bilgi için bkz https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) . (.

### <a name="configure-link"></a>Bağlantıyı Yapılandır

Bir uygulamaya yönelik bir bağlantı ayarlamak için **Çoklu oturum açma** sayfasında **bağlı** ' yı seçin. Sonra bağlantıyı girip **Kaydet**' i seçin. Bu seçeneklerin nerede bulunacağı hakkında bir anımsatıcı mi var? [Hızlı başlangıç serisine](view-applications-portal.md)göz atın.
 
Bir uygulamayı yapılandırdıktan sonra, bu uygulamaya Kullanıcı ve grup atayın. Kullanıcıları atadığınızda, uygulamanın [uygulamamda](https://myapplications.microsoft.com/) ne zaman göründüğünü veya Microsoft 365 uygulama Başlatıcısı ' nı denetleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulamaya Kullanıcı veya Grup atama](methods-for-assigning-users-and-groups.md)
- [Otomatik Kullanıcı hesabı sağlamayı yapılandırma](../app-provisioning/configure-automatic-user-provisioning-portal.md)
