---
title: 'Quickstart: Uygulamaları Microsoft kimlik platformuyla kaydetme | Azure'
description: Bu hızlı başlangıçta, microsoft kimlik platformuna nasıl bir uygulama ekleyeceğinizi ve kaydedilen öğrenebilirsiniz.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 03/12/2020
ms.author: ryanwi
ms.custom: aaddev, identityplatformtop40
ms.reviewer: aragra, lenalepa, sureshja
ms.openlocfilehash: 79983678d13b810a521a00ba2c1978de92a5029f
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81309518"
---
# <a name="quickstart-register-an-application-with-the-microsoft-identity-platform"></a>Quickstart: Microsoft kimlik platformuile bir uygulama kaydetme

Bu hızlı başlangıçta, Azure portalındaki **Uygulama kayıtları** deneyimini kullanarak bir uygulamayı kaydedebilirsiniz. 

Uygulamanız, Azure Active Directory kiracısı ile kaydederek Microsoft kimlik platformuyla entegre edilmiştir. Kurumsal geliştiriciler ve hizmet olarak yazılım (SaaS) sağlayıcıları, Microsoft kimlik platformuyla entegre edilebilen ticari bulut hizmetleri veya iş yeri uygulamaları geliştirebilir. Tümleştirme, bu tür hizmetler için güvenli oturum açma ve yetkilendirme sağlar.

## <a name="prerequisites"></a>Ön koşullar

* Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* Azure [AD kiracı.](quickstart-create-new-tenant.md)

## <a name="register-a-new-application-using-the-azure-portal"></a>Yeni bir uygulamayı Azure portalını kullanarak kaydetme

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Hesabınız birden fazla kiracıya erişim sağlıyorsa, sağ üst köşedeki hesabınızı seçin. Portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
1. **Azure Active Directory**'yi bulun ve seçin. **Yönet**'in altında **Uygulama kayıtları**nı seçin.
1. **Yeni kayıt**seçin.
1. **Bir uygulamayı kaydedin,** kullanıcılara görüntülemek için anlamlı bir uygulama adı girin.
1. Uygulamayı kimlerin kullanabileceğini aşağıdaki gibi belirtin:

    | Desteklenen hesap türleri | Açıklama |
    |-------------------------|-------------|
    | **Yalnızca bu kuruluş dizinindeki hesaplar** | Bir iş kolu uygulaması (LOB) oluşturuyorsanız bu seçeneği belirtin. Uygulamayı bir dizine kaydetmiyorsanız, bu seçenek kullanılamaz.<br><br>Bu seçenek, yalnızca Azure AD tek kiracılı hesaba eşlenir.<br><br>Uygulamayı bir dizinin dışına kaydettirmediğiniz sürece bu seçenek varsayılandır. Uygulamanın bir dizin dışında kaydedildiği durumlarda, varsayılan seçenek Azure çok kiracılı ve kişisel Microsoft kişisel hesaplarıdır. |
    | **Herhangi bir kuruluş dizinindeki hesaplar** | Tüm iş ve eğitim müşterilerini hedeflemek istiyorsanız bu seçeneği belirleyin.<br><br>Bu seçenek, bir yalnızca Azure AD çok kiracılı hesaba eşlenir.<br><br>Uygulamayı Yalnızca tek kiracılı Azure AD olarak kaydettiyseniz, uygulamayı Azure AD çok kiracılı olarak güncelleyebilir ve **Kimlik Doğrulama** sayfası aracılığıyla tek kiracıya geri dönebilirsiniz. |
    | **Herhangi bir kuruluş dizinindeki hesaplar ve kişisel Microsoft hesapları** | En geniş müşteri kümesini hedeflemek için bu seçeneği belirleyin.<br><br>Bu seçenek Azure AD çok kiracılı ve kişisel Microsoft hesaplarına eşlenir.<br><br>Uygulamayı Azure AD çok kiracılı ve kişisel Microsoft hesapları olarak kaydettiyseniz, kullanıcı arabirimi'nde bu ayarı değiştiremezsiniz. Bunun yerine, desteklenen hesap türlerini değiştirmek için uygulama bildirimi düzenleyicisini kullanmanız gerekir. |

1. **Redirect URI (isteğe bağlı)** altında, oluşturmakta olduğunuz uygulama türünü seçin: **Web** veya **Public client (mobil & masaüstü)**. Ardından, başvurunuz için yeniden yönlendirme URI'sini veya yanıt URL'sini girin.

    * Web uygulamaları için, uygulamanızın temel URL'sini girin. Örneğin `https://localhost:31544` yerel makinenizde çalışan bir web uygulamasının URL'si olabilir. Kullanıcılar, bir web istemci uygulamasında oturum açmak için bu URL'yi kullanır.
    * Genel istemci uygulamaları için, Azure AD'nin belirteç yanıtlarını döndürmek üzere kullandığı URI'yi girin. Uygulamanıza özgü bir değer girin, örn. `myapp://auth`.

    Web uygulamaları veya yerel uygulamalar için örnekler için, [Microsoft kimlik platformunda](https://docs.microsoft.com/azure/active-directory/develop)hızlı başlangıçlar bakın.

1. Bittiğinde **Kaydet**’i seçin.

    ![Azure portalına yeni bir uygulama kaydetmek için ekranı gösterir](./media/quickstart-add-azure-ad-app-preview/new-app-registration.png)

Azure AD, uygulamanız için benzersiz bir uygulama veya istemci kimliği atar. Portal, uygulamanızın **Genel Bakış** sayfasını açar. Uygulamanıza özellikler eklemek için marka, sertifika ve sırlar, API izinleri ve daha fazlası dahil olmak üzere diğer yapılandırma seçeneklerini seçebilirsiniz.

![Yeni kaydedilmiş bir uygulamaya genel bakış sayfası örneği](./media/quickstart-add-azure-ad-app-preview/new-app-overview-page-expanded.png)

## <a name="next-steps"></a>Sonraki adımlar

* Web API'lerine erişmek için [Bkz. Quickstart: Web API'lerine erişmek için istemci uygulamasını yapılandırma](quickstart-configure-app-access-web-apis.md)

* İzinler hakkında bilgi edinmek için [Microsoft kimlik platformu bitiş noktasında İzinler ve onay günü'ne](v2-permissions-and-consent.md)bakın.

* Web API'lerini ortaya çıkarmak için [Bkz. Quickstart: Web API'lerini ortaya çıkarmak için bir uygulamayı yapılandırın.](quickstart-configure-app-expose-web-apis.md)

* Desteklenen hesapları yönetmek için [Bkz. Hızlı Başlangıç: Bir uygulama tarafından desteklenen hesapları değiştirin.](quickstart-modify-supported-accounts.md)

* Bir uygulama oluşturmak ve işlevsellik eklemek için [Microsoft kimlik platformunda](https://docs.microsoft.com/azure/active-directory/develop)hızlı başlangıçlar'a bakın.

* Kayıtlı uygulamayı temsil eden iki Azure AD nesnesi ve aralarındaki ilişki hakkında daha fazla bilgi edinmek için bkz. [Uygulama nesneleri ve hizmet sorumlusu nesneleri](app-objects-and-service-principals.md).

* Uygulama geliştirirken kullanmanız gereken marka yönergeleri hakkında daha fazla bilgi edinmek [için uygulamalar için Marka oluşturma yönergelerine](howto-add-branding-in-azure-ad-apps.md)bakın.
