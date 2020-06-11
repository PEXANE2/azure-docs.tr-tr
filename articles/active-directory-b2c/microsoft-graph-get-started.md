---
title: Microsoft Graph ile kaynakları yönetme
titleSuffix: Azure AD B2C
description: Gerekli Graph API izinleri verilen bir uygulamayı kaydederek Microsoft Graph Azure AD B2C kaynaklarını yönetmeye hazırlanın.
services: B2C
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/14/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: fd4ed98383d38be8528bafd6ec7d149381386fac
ms.sourcegitcommit: f01c2142af7e90679f4c6b60d03ea16b4abf1b97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84672553"
---
# <a name="manage-azure-ad-b2c-with-microsoft-graph"></a>Microsoft Graph ile Azure AD B2C yönetme

[Microsoft Graph][ms-graph] , müşteri Kullanıcı hesapları ve özel ilkeler dahil olmak üzere Azure AD B2C kiracınızdaki kaynakların birçoğunu yönetmenizi sağlar. [MICROSOFT Graph API][ms-graph-api]'sini çağıran komut dosyaları veya uygulamalar yazarak, şu şekilde kiracı yönetim görevlerini otomatikleştirebilirsiniz:

* Mevcut bir Kullanıcı deposunu Azure AD B2C kiracısına geçirme
* Azure DevOps 'da bir Azure işlem hattı ile özel ilkeler dağıtma ve özel ilke anahtarlarını yönetme
* Kullanıcı kaydını kendi sayfanızda barındırın ve arka planda Azure AD B2C dizininizde Kullanıcı hesapları oluşturun
* Uygulama kaydını otomatikleştirme
* Denetim günlüklerini al

Aşağıdaki bölümler, Azure AD B2C dizininizdeki kaynakların yönetimini otomatikleştirmek için Microsoft Graph API 'sini kullanmaya hazırlanmanıza yardımcı olur.

## <a name="microsoft-graph-api-interaction-modes"></a>Microsoft Graph API etkileşim modları

Azure AD B2C kiracınızdaki kaynakları yönetmek için Microsoft Graph API 'siyle çalışırken kullanabileceğiniz iki iletişim modu vardır:

* **Etkileşimli** -bir kez çalıştır görevleri için, yönetim görevlerini GERÇEKLEŞTIRMEK üzere B2C kiracısında bir yönetici hesabı kullanın. Bu mod, bir yöneticinin Microsoft Graph API 'YI çağırmadan önce kimlik bilgilerini kullanarak oturum açmasını gerektirir.

* **Otomatikleştirilmiş** -zamanlanmış veya sürekli olarak çalıştırma görevleri için, bu yöntem yönetim görevlerini gerçekleştirmek için gereken izinlerle yapılandırdığınız bir hizmet hesabını kullanır. Uygulama *(istemci) kimliğini* ve **OAuth 2,0 istemci kimlik bilgilerini** kullanarak kimlik doğrulaması için kullanılan uygulama ve betiklerinizin bir uygulamayı kaydederek Azure AD B2C ' de "hizmet hesabı" oluşturursunuz. Bu durumda, uygulama, daha önce açıklanan etkileşimli yönteminde olduğu gibi yönetici kullanıcıyı değil Microsoft Graph API 'sini çağırmak için kendisini kendisi olarak çalışır.

Aşağıdaki bölümlerde gösterilen bir uygulama kaydı oluşturarak **Otomatik** etkileşim senaryosunu etkinleştirirsiniz.

OAuth 2,0 istemci kimlik bilgileri verme akışı şu anda Azure AD B2C kimlik doğrulama hizmeti tarafından doğrudan desteklenmese de, Azure AD 'yi kullanarak istemci kimlik bilgileri akışını ve Azure AD B2C kiracınızdaki bir uygulama için Microsoft Identity platform/Token uç noktasını ayarlayabilirsiniz. Azure AD B2C kiracı, Azure AD kurumsal kiracılar ile bazı işlevleri paylaşır.

## <a name="register-management-application"></a>Yönetim uygulamasını Kaydet

Komut dosyalarınız ve uygulamalarınızın Azure AD B2C kaynaklarını yönetmek için [MICROSOFT Graph API][ms-graph-api] 'siyle etkileşime girebilmesi için, Azure AD B2C kiracınızda gerekli API izinlerini veren bir uygulama kaydı oluşturmanız gerekir.

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portal, araması yapın ve **Azure AD B2C**seçin.
1. **Uygulama kayıtları**öğesini seçin ve ardından **Yeni kayıt**' ı seçin.
1. Uygulama için bir **ad** girin. Örneğin, *managementapp1*.
1. **Yalnızca bu kuruluş dizininde hesaplar '** ı seçin.
1. **İzinler**altında, *openıd ve offline_access izinleri Için yönetici izni ver* onay kutusunu temizleyin.
1. **Kaydol**’u seçin.
1. Uygulamaya Genel Bakış sayfasında görüntülenen **uygulama (istemci) kimliğini** kaydedin. Bu değeri sonraki bir adımda kullanırsınız.

### <a name="grant-api-access"></a>API erişimi verme

Ardından, Microsoft Graph API 'sine yapılan çağrılar aracılığıyla kiracı kaynaklarını işlemek için kayıtlı uygulamaya izin verin.

[!INCLUDE [active-directory-b2c-permissions-directory](../../includes/active-directory-b2c-permissions-directory.md)]

### <a name="create-client-secret"></a>İstemci parolası oluştur

[!INCLUDE [active-directory-b2c-client-secret](../../includes/active-directory-b2c-client-secret.md)]

Artık Azure AD B2C kiracınızda Kullanıcı *oluşturma*, *okuma*, *güncelleştirme*ve *silme* iznine sahip bir uygulamanız var. *Parola güncelleştirme* izinleri eklemek için sonraki bölüme geçin.

## <a name="enable-user-delete-and-password-update"></a>Kullanıcı silme ve parola güncelleştirmesini etkinleştir

*Dizin verilerini okuma ve yazma* **izni, kullanıcıları** silme veya Kullanıcı hesabı parolalarını güncelleştirme imkanını içermez.

Uygulamanızın veya betiğinizin kullanıcıları silmesi veya parolalarını güncelleştirmesi gerekiyorsa, uygulamanıza *Kullanıcı Yöneticisi* rolünü atayın:

1. [Azure Portal](https://portal.azure.com) oturum açın ve **Dizin + abonelik** filtresini kullanarak Azure AD B2C kiracınıza geçiş yapın.
1. Arama yapın ve **Azure AD B2C**seçin.
1. **Yönet**altında **Roller ve yöneticiler**' i seçin.
1. **Kullanıcı Yöneticisi** rolünü seçin.
1. **Atama Ekle**' yi seçin.
1. Metin **Seç** kutusuna daha önce kaydettiğiniz uygulamanın adını girin, örneğin, *managementapp1*. Arama sonuçlarında göründüğünde uygulamanızı seçin.
1. **Ekle**'yi seçin. İzinlerin tam olarak yayılması birkaç dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
Yönetim uygulamanızı kaydoldığınıza ve gerekli izinleri vermiş olduğunuza göre, uygulama ve hizmetleriniz (örneğin, Azure Pipelines) Microsoft Graph API 'siyle etkileşim kurmak için kimlik bilgilerini ve izinlerini kullanabilir. 

* [Azure AD'den erişim belirteci alma](https://docs.microsoft.com/graph/auth-v2-service#4-get-an-access-token)
* [Microsoft Graph çağırmak için erişim belirtecini kullanın](https://docs.microsoft.com/graph/auth-v2-service#4-get-an-access-token)
* [Microsoft Graph tarafından desteklenen B2C işlemleri](microsoft-graph-operations.md)
* [Microsoft Graph ile Azure AD B2C Kullanıcı hesaplarını yönetme](manage-user-accounts-graph-api.md)
* [Azure AD Raporlama API 'SI ile denetim günlüklerini alın](view-audit-logs.md#get-audit-logs-with-the-azure-ad-reporting-api)

<!-- LINKS -->
[ms-graph]: https://docs.microsoft.com/graph/
[ms-graph-api]: https://docs.microsoft.com/graph/api/overview