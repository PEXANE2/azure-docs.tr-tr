---
title: Microsoft Graph ile kaynakları yönetme
titleSuffix: Azure AD B2C
description: Gerekli Grafik API izinlerini vermiş bir uygulamayı kaydederek Microsoft Graph ile Azure AD B2C kaynaklarını yönetmeye hazırolun.
services: B2C
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/14/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 32117d4bfcf0c0af94eced095b94ab0c1b6f88af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78184374"
---
# <a name="manage-azure-ad-b2c-with-microsoft-graph"></a>Microsoft Graph ile Azure AD B2C'yi yönetme

[Microsoft Graph,][ms-graph] azure AD B2C kiracınızdaki müşteri kullanıcı hesapları ve özel ilkeler de dahil olmak üzere birçok kaynağı yönetmenize olanak tanır. [Microsoft Graph API'yi][ms-graph-api]arayan komut dosyaları veya uygulamalar yazarak, aşağıdakiler gibi kiracı yönetim görevlerini otomatikleştirebilirsiniz:

* Varolan bir kullanıcı mağazasını Azure AD B2C kiracısına geçirin
* Azure DevOps'lerde Azure Ardışık Bir Azure Ardışık Hattı ile özel ilkeler dağıtma ve özel ilke anahtarlarını yönetme
* Kendi sayfanızda kullanıcı kaydı nı barındırın ve azure AD B2C dizininizde arka planda kullanıcı hesapları oluşturun
* Başvuru kaydını otomatikleştirin
* Denetim günlüklerini edinme

Aşağıdaki bölümler, Azure AD B2C dizininizdeki kaynakların yönetimini otomatikleştirmek için Microsoft Graph API'sını kullanmaya hazırlanmanıza yardımcı olur.

## <a name="microsoft-graph-api-interaction-modes"></a>Microsoft Graph API etkileşim modları

Azure AD B2C kiracınızdaki kaynakları yönetmek için Microsoft Graph API ile çalışırken kullanabileceğiniz iki iletişim modu vardır:

* **Etkileşimli** - Çalıştırılan görevler için uygun, yönetim görevlerini gerçekleştirmek için B2C kiracıbir yönetici hesabı kullanın. Bu mod, Microsoft Graph API'yi aramadan önce bir yöneticinin kimlik bilgilerini kullanarak oturum açmasını gerektirir.

* **Otomatik** - Zamanlanmış veya sürekli çalışan görevler için bu yöntem, yönetim görevlerini gerçekleştirmek için gereken izinlerle yapılandırdığınız bir hizmet hesabı kullanır. *Uygulama (İstemci) Kimliği* ve OAuth 2.0 istemci kimlik bilgileri hibesini kullanarak uygulamalarınızın ve komut dosyalarınızın kimlik doğrulaması için kullandığı bir uygulamayı kaydederek Azure AD B2C'de "hizmet hesabı"nı oluşturursunuz. Bu durumda, uygulama, daha önce açıklanan etkileşimli yöntemde olduğu gibi yönetici kullanıcıyı değil, Microsoft Graph API'yi aramak için kendisi gibi davranır.

Aşağıdaki bölümlerde gösterilen bir uygulama kaydı oluşturarak **Otomatik** etkileşim senaryosunu etkinleştirin.

## <a name="register-management-application"></a>Kayıt yönetimi başvurusu

Komut dosyalarınız ve uygulamalarınız Azure AD B2C kaynaklarını yönetmek için [Microsoft Graph API][ms-graph-api] ile etkileşime geçmeden önce, Azure AD B2C kiracınızda gerekli API izinlerini veren bir uygulama kaydı oluşturmanız gerekir.

[!INCLUDE [active-directory-b2c-appreg-mgmt](../../includes/active-directory-b2c-appreg-mgmt.md)]

### <a name="grant-api-access"></a>API erişimi verme

Ardından, Microsoft Graph API'ye yapılan çağrılar yoluyla kiracı kaynaklarını işlemek için kayıtlı uygulama izinleri tanıyın.

[!INCLUDE [active-directory-b2c-permissions-directory](../../includes/active-directory-b2c-permissions-directory.md)]

### <a name="create-client-secret"></a>İstemci sırrı oluşturma

[!INCLUDE [active-directory-b2c-client-secret](../../includes/active-directory-b2c-client-secret.md)]

Artık Azure AD B2C kiracınızdaki kullanıcıları *oluşturma,* *okuma,* *güncelleme*ve *silme* iznine sahip bir uygulamanız var. *Parola güncelleştirme* izinleri eklemek için sonraki bölüme devam edin.

## <a name="enable-user-delete-and-password-update"></a>Kullanıcı silme ve parola güncelleştirmesini etkinleştirme

*Okuma ve yazma dizin iden izin,* kullanıcıları silme veya kullanıcı hesabı parolalarını güncelleştirme özelliğini içermez. **NOT**

Uygulamanızın veya komut dosyanızın kullanıcıları silmesi veya parolalarını güncellemesi gerekiyorsa, *Uygulamanıza Kullanıcı yöneticisi* rolünü atayın:

1. [Azure portalında](https://portal.azure.com) oturum açın ve Azure AD B2C kiracınıza geçmek için **Dizin + Abonelik** filtresini kullanın.
1. Azure AD **B2C'yi**arayın ve seçin.
1. **Yönet** **altında, Rolleri ve yöneticileri**seçin.
1. Kullanıcı **yöneticisi** rolünü seçin.
1. **Atama ekle'yi**seçin.
1. **Metin** seç kutusuna, daha önce kaydettiğiniz uygulamanın adını girin( örneğin, *yönetimapp1*. Arama sonuçlarında göründüğünde uygulamanızı seçin.
1. **Ekle'yi**seçin. İzinlerin tam olarak yayılması birkaç dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

Artık yönetim başvurunuzu kaydettirdiğiniz ve gerekli izinleri aldığınıza göre, uygulamalarınız ve hizmetleriniz (örneğin, Azure Ardışık Hatları) kimlik bilgilerini ve Microsoft Graph API ile etkileşim de izinlerini kullanabilir.

* [Microsoft Graph tarafından desteklenen B2C işlemleri](microsoft-graph-operations.md)
* [Microsoft Graph ile Azure AD B2C kullanıcı hesaplarını yönetme](manage-user-accounts-graph-api.md)
* [Azure AD raporlama API'si ile denetim günlükleri alın](view-audit-logs.md#get-audit-logs-with-the-azure-ad-reporting-api)

<!-- LINKS -->
[ms-graph]: https://docs.microsoft.com/graph/
[ms-graph-api]: https://docs.microsoft.com/graph/api/overview
