---
title: 'Hızlı başlangıç: Microsoft Identity platform ile uygulamaları kaydetme | Mavisi'
description: Bu hızlı başlangıçta, bir uygulamayı Microsoft Identity platformu ile nasıl ekleyeceğinizi ve kaydedeceğinizi öğreneceksiniz.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 03/09/2020
ms.author: ryanwi
ms.custom: aaddev, identityplatformtop40
ms.reviewer: aragra, lenalepa, sureshja
ms.openlocfilehash: dc719064166be917d868c7b7fee6b126b4099840
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79240887"
---
# <a name="quickstart-register-an-application-with-the-microsoft-identity-platform"></a>Hızlı başlangıç: Microsoft Identity platformu ile uygulama kaydetme

Bu hızlı başlangıçta, Azure portal **uygulama kayıtları** deneyimini kullanarak bir uygulamayı kaydedersiniz. Uygulamanız Microsoft Identity platformu ile tümleşiktir. Kurumsal geliştiriciler ve hizmet olarak yazılım (SaaS) sağlayıcıları, Microsoft Identity platform ile tümleştirilebilen ticari bulut hizmetleri veya iş kolu uygulamaları geliştirebilir. Tümleştirme, bu hizmetler için güvenli oturum açma ve yetkilendirme sağlar.

## <a name="prerequisites"></a>Önkoşullar

* Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

## <a name="register-a-new-application-using-the-azure-portal"></a>Yeni bir uygulamayı Azure portalını kullanarak kaydetme

1. Bir iş veya okul hesabı ya da kişisel Microsoft hesabınızı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Hesabınız birden fazla kiracıya erişim veriyorsa, sağ üst köşedeki hesabınızı seçin. Portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
1. Arama yapın ve **Azure Active Directory**seçin. **Yönet**altında **uygulama kayıtları**' yi seçin.
1. **Yeni kayıt**seçeneğini belirleyin.
1. **Bir uygulamayı kaydet**' de kullanıcılara görüntülenecek anlamlı bir uygulama adı girin.
1. Uygulamayı şu şekilde kullanabilecek kişileri belirtin:

    | Desteklenen hesap türleri | Açıklama |
    |-------------------------|-------------|
    | **Yalnızca bu kuruluş dizinindeki hesaplar** | Bir iş kolu uygulaması (LOB) oluşturuyorsanız bu seçeneği belirtin. Uygulamayı bir dizine kaydetaçmadıysanız, bu seçenek kullanılamaz.<br><br>Bu seçenek, yalnızca Azure AD tek kiracılı hesaba eşlenir.<br><br>Uygulamayı bir dizinin dışına kaydetmiyorsanız Bu seçenek varsayılandır. Uygulamanın bir dizin dışında kaydedildiği durumlarda, varsayılan seçenek Azure çok kiracılı ve kişisel Microsoft kişisel hesaplarıdır. |
    | **Herhangi bir kuruluş dizinindeki hesaplar** | Tüm iş ve eğitim müşterilerini hedeflemek istiyorsanız bu seçeneği belirleyin.<br><br>Bu seçenek, bir yalnızca Azure AD çok kiracılı hesaba eşlenir.<br><br>Uygulamayı Azure AD 'ye yalnızca tek kiracı olarak kaydettirdiğiniz takdirde, **kimlik doğrulama** sayfası aracılığıyla Azure AD çok kiracılı ve tek kiracıya geri yükleyebilirsiniz. |
    | **Herhangi bir kuruluş dizinindeki hesaplar ve kişisel Microsoft hesapları** | En geniş müşteri kümesini hedeflemek için bu seçeneği belirleyin.<br><br>Bu seçenek Azure AD çok kiracılı ve kişisel Microsoft hesaplarına eşlenir.<br><br>Uygulamayı Azure AD çok kiracılı ve kişisel Microsoft hesapları olarak kaydettiniz, Kullanıcı arabiriminde bu ayarı değiştiremezsiniz. Bunun yerine, desteklenen hesap türlerini değiştirmek için uygulama bildirimi düzenleyicisini kullanmanız gerekir. |

1. **Yeniden yönlendirme URI 'si (isteğe bağlı)** altında, oluşturmakta olduğunuz uygulamanın türünü seçin: **Web** veya **Public Client (mobil & Masaüstü)** . Ardından, uygulamanız için yeniden yönlendirme URI 'sini veya yanıt URL 'sini girin.

    * Web uygulamaları için, uygulamanızın temel URL'sini girin. Örneğin `https://localhost:31544` yerel makinenizde çalışan bir web uygulamasının URL'si olabilir. Kullanıcılar, bir web istemci uygulamasında oturum açmak için bu URL'yi kullanır.
    * Genel istemci uygulamaları için, Azure AD'nin belirteç yanıtlarını döndürmek üzere kullandığı URI'yi girin. Uygulamanıza özgü bir değer girin, örn. `myapp://auth`.

    Web uygulamaları veya yerel uygulamalar için örnekler için bkz. [Microsoft Identity platform](https://docs.microsoft.com/azure/active-directory/develop)'daki hızlı başlangıçlara bakın.

1. Bittiğinde **Kaydet**’i seçin.

    ![Azure portal yeni bir uygulamanın kaydedileceği ekranı gösterir](./media/quickstart-add-azure-ad-app-preview/new-app-registration.png)

Azure AD, uygulamanıza benzersiz bir uygulama veya istemci KIMLIĞI atar. Portal, uygulamanızın **genel bakış** sayfasını açar. Uygulamanıza özellik eklemek için, marka, sertifikalar ve gizlilikler, API izinleri ve daha fazlasını içeren diğer yapılandırma seçeneklerini belirleyebilirsiniz.

![Yeni kayıtlı bir uygulamaya genel bakış sayfası örneği](./media/quickstart-add-azure-ad-app-preview/new-app-overview-page-expanded.png)

## <a name="next-steps"></a>Sonraki adımlar

Web API 'Lerine nasıl erişebileceğinizi öğrenmek için sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Hızlı başlangıç: Web API 'Lerine erişmek için bir istemci uygulaması yapılandırma](quickstart-configure-app-access-web-apis.md)

* İzinler hakkında bilgi edinmek için bkz. [Microsoft Identity platform uç noktasındaki izinler ve onay](v2-permissions-and-consent.md).

* Web API 'Lerini göstermek için bkz. [hızlı başlangıç: Web API 'leri göstermek için uygulama yapılandırma](quickstart-configure-app-expose-web-apis.md).

* Desteklenen hesapları yönetmek için bkz. [hızlı başlangıç: bir uygulama tarafından desteklenen hesapları değiştirme](quickstart-modify-supported-accounts.md).

* Bir uygulama oluşturmak ve işlevsellik eklemek için bkz. [Microsoft Identity platform](https://docs.microsoft.com/azure/active-directory/develop)'daki hızlı başlangıçlara bakın.

* Kayıtlı uygulamayı temsil eden iki Azure AD nesnesi ve aralarındaki ilişki hakkında daha fazla bilgi edinmek için bkz. [Uygulama nesneleri ve hizmet sorumlusu nesneleri](app-objects-and-service-principals.md).

* Uygulama geliştirirken kullanmanız gereken marka yönergeleri hakkında daha fazla bilgi edinmek için bkz. [uygulamalar Için marka yönergeleri](howto-add-branding-in-azure-ad-apps.md).
