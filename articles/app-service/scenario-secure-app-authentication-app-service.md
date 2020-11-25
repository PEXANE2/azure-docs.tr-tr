---
title: Öğretici-Azure App Service bir Web uygulamasına kimlik doğrulaması ekleme | Mavisi
description: Bu öğreticide, Azure App Service üzerinde çalışan bir Web uygulaması için kimlik doğrulama ve yetkilendirmeyi nasıl etkinleştireceğinizi öğreneceksiniz. Web uygulamasına erişimi kuruluşunuzdaki kullanıcılarla sınırlayın.
services: active-directory, app-service-web
author: rwike77
manager: CelesteDG
ms.service: app-service-web
ms.topic: tutorial
ms.workload: identity
ms.date: 11/09/2020
ms.author: ryanwi
ms.reviewer: stsoneff
ms.openlocfilehash: 297db67b80f9e5ae04e3c10f370ece9b5fdedee3
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "96020458"
---
# <a name="tutorial-add-authentication-to-your-web-app-running-on-azure-app-service"></a>Öğretici: Azure App Service üzerinde çalışan Web uygulamanıza kimlik doğrulaması ekleme

Azure App Service üzerinde çalışan Web uygulamanız için kimlik doğrulamanın nasıl etkinleştirileceğini ve kuruluşunuzdaki kullanıcılara erişimi kısıtlayacağınızı öğrenin.

:::image type="content" source="./media/scenario-secure-app-authentication-app-service/web-app-sign-in.svg" alt-text="Kullanıcı oturum açma gösteren diyagram." border="false":::

App Service, yerleşik kimlik doğrulama ve yetkilendirme desteği sağlar. bu sayede, Web uygulamanızda en düşük veya hiç kod yazmadan kullanıcılara oturum açabilir ve verilere erişebilirsiniz. App Service kimlik doğrulaması/yetkilendirme modülünü kullanmak gerekli değildir, ancak uygulamanız için kimlik doğrulama ve yetkilendirmeyi basitleştirmeye yardımcı olur. Bu makalede, kimlik sağlayıcısı olarak Azure Active Directory (Azure AD) kullanarak Web uygulamanızın App Service kimlik doğrulaması/yetkilendirme modülüyle nasıl güvenliği yapılacağı gösterilmektedir.

Kimlik doğrulama/yetkilendirme modülü etkinleştirilir ve Azure portal ve uygulama ayarları aracılığıyla yapılandırılır. SDK, belirli dil veya uygulama kodundaki değişiklikler gerekli değildir. Azure AD, Microsoft hesabı, Facebook, Google ve Twitter da dahil olmak üzere çeşitli kimlik sağlayıcıları desteklenir. Kimlik doğrulama/yetkilendirme modülü etkinleştirildiğinde, gelen tüm HTTP istekleri uygulama kodu tarafından işlenmeden önce üzerinden geçirilir. Daha fazla bilgi için bkz. [Azure App Service kimlik doğrulaması ve yetkilendirme](overview-authentication-authorization.md).

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
>
> * Web uygulaması için kimlik doğrulamasını yapılandırın.
> * Web uygulamasına erişimi kuruluşunuzdaki kullanıcılarla sınırlayın.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-and-publish-a-web-app-on-app-service"></a>App Service Web uygulaması oluşturma ve yayımlama

Bu öğreticide, App Service dağıtılan bir Web uygulamasının olması gerekir. Mevcut bir Web uygulamasını kullanabilir veya App Service yeni bir Web uygulaması oluşturmak ve yayımlamak için [ASP.NET Core hızlı](quickstart-dotnetcore.md) başlangıcı ' nı takip edebilirsiniz.

Var olan bir Web uygulaması mı yoksa yeni bir tane oluşturun, Web uygulamasının adı ve Web uygulamasının dağıtıldığı kaynak grubunun adını unutmayın. Bu öğreticide bu adlara ihtiyacınız vardır. Bu öğreticide, yordamlarda ve ekran görüntülerinde örnek adlar *Securewebapp* içerir.

## <a name="configure-authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirmeyi yapılandırma

Artık App Service üzerinde çalışan bir Web uygulamanız var. Daha sonra, Web uygulaması için kimlik doğrulama ve yetkilendirmeyi etkinleştirirsiniz. Kimlik sağlayıcısı olarak Azure AD 'yi kullanırsınız. Daha fazla bilgi için bkz. [App Service uygulamanızın Azure AD kimlik doğrulamasını yapılandırma](configure-authentication-provider-aad.md).

[Azure Portal](https://portal.azure.com) menüsünde, **kaynak grupları**' nı seçin veya herhangi bir sayfadan **kaynak grupları** arayın ve seçin.

**Kaynak grupları**' nda, kaynak grubunuzu bulun ve seçin. **Genel bakış** bölümünde uygulamanızın yönetim sayfasını seçin.

:::image type="content" alt-text="Uygulamanızın yönetim sayfasını seçmeyi gösteren ekran görüntüsü." source="./media/scenario-secure-app-authentication-app-service/select-app-service.png":::

Uygulamanızın sol menüsünde **kimlik doğrulama/yetkilendirme**' yi seçin ve ardından **Açık**' ı seçerek App Service kimlik doğrulamasını etkinleştirin.

**İsteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem** menüsünde **Azure Active Directory ile oturum aç**’ı seçin.

**Kimlik doğrulama sağlayıcıları** altında **Azure Active Directory**' yi seçin. **Hızlı**' i seçin ve ardından yeni bir Active Directory uygulaması oluşturmak için varsayılan ayarları kabul edin. **Tamam**’ı seçin.

:::image type="content" alt-text="Express kimlik doğrulamasını gösteren ekran görüntüsü." source="./media/scenario-secure-app-authentication-app-service/configure-authentication.png":::

**Kimlik doğrulama/yetkilendirme** sayfasında **Kaydet**' i seçin.

İletiyle ilgili bildirimi gördüğünüzde `Successfully saved the Auth Settings for <app-name> App` Portal sayfasını yenileyin.

Artık App Service kimlik doğrulaması ve yetkilendirmeyle güvenliği sağlanmış bir uygulamanız var.

## <a name="verify-limited-access-to-the-web-app"></a>Web uygulamasına sınırlı erişimi doğrulama

App Service kimlik doğrulaması/yetkilendirme modülünü etkinleştirdiğinizde, Azure AD kiracınızda bir uygulama kaydı oluşturulmuştur. Uygulama kaydı, Web uygulamanız ile aynı görünen ada sahiptir. Ayarları denetlemek için Portal menüsünden **Azure Active Directory** ' yi seçin ve **uygulama kayıtları**' ı seçin. Oluşturulan uygulama kaydını seçin. Genel bakışta, **Desteklenen hesap türlerinin** **yalnızca Kuruluşum** olarak ayarlandığını doğrulayın.

:::image type="content" alt-text="Erişimin doğrulanması gerektiğini gösteren ekran görüntüsü." source="./media/scenario-secure-app-authentication-app-service/verify-access.png":::

Uygulamanıza erişimin kuruluşunuzdaki kullanıcılarla sınırlı olduğunu doğrulamak için, ınbilito veya Private modunda bir tarayıcı başlatın ve adresine gidin `https://<app-name>.azurewebsites.net` . Kimliği doğrulanmamış kullanıcıların siteye erişimine izin verilmediği doğrulanırken, güvenli bir oturum açma sayfasına yönlendirilmelisiniz. Siteye erişim kazanmak için kuruluşunuzda kullanıcı olarak oturum açın. Ayrıca, yeni bir tarayıcı başlatabilir ve kuruluşun dışındaki kullanıcıların erişimi olmadığından emin olmak için kişisel bir hesap kullanarak oturum açmayı deneyebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticiyi bitirdiyseniz ve artık Web uygulamasına veya ilişkili kaynaklara ihtiyacınız yoksa, [oluşturduğunuz kaynakları temizleyin](scenario-secure-app-clean-up-resources.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
>
> * Web uygulaması için kimlik doğrulamasını yapılandırın.
> * Web uygulamasına erişimi kuruluşunuzdaki kullanıcılarla sınırlayın.

> [!div class="nextstepaction"]
> [App Service, depolamaya erişiyor](scenario-secure-app-access-storage.md)
