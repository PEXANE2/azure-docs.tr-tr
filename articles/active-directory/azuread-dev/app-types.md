---
title: v1.0 uygulama türleri | Azure
description: Azure Active Directory v2.0 bitiş noktası tarafından desteklenen uygulama ve senaryo türlerini açıklar.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.workload: identity
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: saeeda, jmprieur, andret
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: c290cbf36fd53d5afb5fd805cda896fb6879bb4b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154959"
---
# <a name="application-types-in-v10"></a>v1.0'daki uygulama türleri

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Azure Active Directory (Azure AD), endüstri standardı protokoller OAuth 2.0 veya OpenID Connect'e dayalı olarak çeşitli modern uygulama mimarileri için kimlik doğrulamayı destekler.

Aşağıdaki diyagram, senaryoları ve uygulama türlerini ve farklı bileşenlerin nasıl eklenebilirolduğunu gösterir:

![Uygulama Türleri ve senaryolar](./media/authentication-scenarios/application-types-scenarios.png)

Azure AD tarafından desteklenen beş birincil uygulama senaryosu şunlardır:

- **[Tek sayfalı uygulama (SPA)](single-page-application.md)**: Bir kullanıcının Azure AD tarafından güvenli tek sayfalık bir uygulamada oturum açması gerekir.
- **[Web tarayıcısı web uygulaması](web-app.md)**: Bir kullanıcının Azure AD tarafından güvenli bir web uygulamasında oturum açması gerekir.
- **[Web API'ye yerel uygulama](native-app.md)**: Telefonda, tablette veya bilgisayarda çalışan yerel bir uygulamanın, Azure AD tarafından güvenli bir web API'sinden kaynak elde etmek için kullanıcının kimliğini doğrulaması gerekir.
- **[Web uygulamasından web API'ye](web-api.md)**: Bir web uygulamasının Azure AD tarafından güvenli bir web API'sinden kaynak alması gerekir.
- **[Daemon veya sunucu uygulaması web API](service-to-service.md)** için : Bir daemon uygulaması veya hiçbir web kullanıcı arayüzü olmayan bir sunucu uygulaması Azure AD tarafından güvenli bir web API kaynakları almak gerekir.

Her uygulama türü hakkında daha fazla bilgi edinmek ve kodla çalışmaya başlamadan önce üst düzey senaryoları anlamak için bağlantıları izleyin. Ayrıca v1.0 bitiş noktası veya v2.0 bitiş noktası ile çalışan belirli bir uygulama yazarken bilmeniz gereken farklar hakkında bilgi edinebilirsiniz.

> [!NOTE]
> v2.0 bitiş noktası tüm Azure REKLAM senaryolarını ve özelliklerini desteklemez. v2.0 bitiş noktasını kullanıp kullanmadığınızı belirlemek için [v2.0 sınırlamaları](../develop/active-directory-v2-limitations.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)hakkında bilgi edinin.

Burada açıklanan uygulamaları ve senaryoları çeşitli dilleri ve platformları kullanarak geliştirebilirsiniz. Bunların hepsi kod örnekleri kılavuzunda bulunan tam kod örnekleri ile desteklidir: [senaryoya göre v1.0 kod örnekleri](sample-v1-code.md) ve [senaryoya göre v2.0 kod örnekleri.](../develop/sample-v2-code.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) Kod örneklerini doğrudan ilgili [GitHub örnek depolarından](https://github.com/Azure-Samples?q=active-directory)da indirebilirsiniz.

Ayrıca, uygulamanızın uçtan uca bir senaryonun belirli bir parçasına veya kesimine ihtiyacı varsa, çoğu durumda bu işlevsellik bağımsız olarak eklenebilir. Örneğin, web API çağıran yerel bir uygulamanız varsa, web API'sini de çağıran bir web uygulamasını kolayca ekleyebilirsiniz.

## <a name="app-registration"></a>Uygulama kaydı

### <a name="registering-an-app-that-uses-the-azure-ad-v10-endpoint"></a>Azure AD v1.0 bitiş noktasını kullanan bir uygulamayı kaydetme

Azure AD kimlik doğrulamasına dış kaynak sağlayan tüm uygulamaların bir dizinde kaydedilmesi gerekir. Bu adım, Azure AD'ye bulunduğunuz URL, kimlik doğrulamadan sonra yanıt gönderilecek URL, uygulamanızı tanımlamak için URI ve daha fazlası dahil olmak üzere uygulamanız hakkında bilgi verebiçerir. Bu bilgiler birkaç önemli nedenden dolayı gereklidir:

* Azure AD'nin oturum açma veya belirteçleri değiş tokuş ederken uygulamayla iletişim kurması gerekir. Azure AD ile uygulama arasında aktarılan bilgiler şunlardır:
  
  * **Uygulama Kimliği URI** - Bir uygulama için tanımlayıcı. Bu değer, arayan kişinin hangi uygulama için bir belirteç istediğini belirtmek için kimlik doğrulama sırasında Azure AD'ye gönderilir. Ayrıca, bu değer, uygulamanın hedeflenen hedef olduğunu bilmesi için belirtecine dahil edilir.
  * **URL'yi Yanıtla** ve **URI'yi Yeniden Yönlendirme** - Bir web API'sı veya web uygulaması için Yanıtla URL'si, Azure AD'nin kimlik doğrulama yanıtını göndereceği ve kimlik doğrulamanın başarılı olup olmadığını gösteren bir belirteç de dahil olmak üzere konumudur. Yerel bir uygulama için Redirect URI, Azure AD'nin kullanıcı aracısını OAuth 2.0 isteğiyle yeniden yönlendireceği benzersiz bir tanımlayıcıdır.
  * **Uygulama Kimliği** - Uygulama kaydedildiğinde Azure AD tarafından oluşturulan bir uygulamanın kimliği. Bir yetkilendirme kodu veya belirteç istenirken, Uygulama Kimliği ve Anahtarı kimlik doğrulaması sırasında Azure AD'ye gönderilir.
  * **Anahtar** - Web API'sını aramak için Azure AD'ye kimlik doğrulaması yaparken Uygulama Kimliği ile birlikte gönderilen anahtar.
* Azure AD'nin, uygulamanın dizin verilerinize, kuruluşunuzdaki diğer uygulamalara ve benzeri uygulamalara erişmek için gerekli izinlere sahip olduğundan emin olması gerekir.

Ayrıntılar [için, bir uygulamayı](../develop/quickstart-register-app.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)nasıl kaydedin öğrenin.

## <a name="single-tenant-and-multi-tenant-apps"></a>Tek kiracılı ve çok kiracılı uygulamalar

Azure AD ile geliştirilebilen ve tümleştirilebilen iki uygulama kategorisi olduğunu anladığınızda sağlama daha net hale gelir:

* **Tek kiracı uygulaması** - Tek bir kiracı uygulaması tek bir kuruluşta kullanılmak üzere tasarlanmıştır. Bunlar genellikle bir kurumsal geliştirici tarafından yazılmış iş satırı (LoB) uygulamalarıdır. Tek bir kiracı uygulamasının yalnızca tek bir dizinde kullanıcılar tarafından erişilmesi gerekir ve sonuç olarak yalnızca tek bir dizinde sağlanması gerekir. Bu uygulamalar genellikle kuruluştaki bir geliştirici tarafından kaydedilir.
* **Çok kiracılı uygulama** - Çok kiracılı bir uygulama, tek bir kuruluşta değil, birçok kuruluşta kullanılmak üzere tasarlanmıştır. Bunlar genellikle bağımsız bir yazılım satıcısı (ISV) tarafından yazılmış hizmet olarak yazılım (SaaS) uygulamalarıdır. Çok kiracılı uygulamaların kullanılacağı her dizinde sağlanması gerekir ve bu da kullanıcı veya yöneticinin bunları kaydetme iznini gerektirir. Bu onay işlemi, bir uygulama dizine kaydedildiğinde başlar ve Grafik API'sine veya belki de başka bir web API'sine erişim verilir. Farklı bir kuruluştan bir kullanıcı veya yönetici uygulamayı kullanmak için kaydolduğunda, uygulamanın gerektirdiği izinleri görüntüleyen bir iletişim kutusu sunulur. Kullanıcı veya yönetici daha sonra, uygulamaya belirtilen verilere erişim sağlayan ve son olarak uygulamayı dizinine kaydeden uygulamayı onaylayabilir. Daha fazla bilgi için, [Bkz. Onay Çerçevesigenel Bakış.](../develop/consent-framework.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)

### <a name="additional-considerations-when-developing-single-tenant-or-multi-tenant-apps"></a>Tek kiracı veya çok kiracılı uygulamalar geliştirirken dikkat edilmesi gereken ek noktalar

Tek bir kiracı uygulaması yerine çok kiracılı bir uygulama geliştirirken bazı ek hususlar ortaya çıkar. Örneğin, uygulamanızı birden çok dizindeki kullanıcılar tarafından kullanılabilir hale getiriyorsanız, hangi kiracıda olduklarını belirlemek için bir mekanizmaya ihtiyacınız vardır. Tek bir kiracı uygulamasıyalnızca bir kullanıcı için kendi dizininde bakmak gerekirken, çok kiracılı bir uygulamanın Azure AD'deki tüm dizinlerden belirli bir kullanıcıyı tanımlaması gerekir. Bu görevi gerçekleştirmek için Azure AD, kiracıya özgü bir bitiş noktası yerine çok kiracılı uygulamaların oturum açma isteklerini yönlendirebileceği ortak bir kimlik doğrulama bitiş noktası sağlar. Bu bitiş `https://login.microsoftonline.com/common` noktası Azure AD'deki tüm dizinler içindir, `https://login.microsoftonline.com/contoso.onmicrosoft.com`kiracıya özgü bir bitiş noktası ise. Oturum açma, imzalama ve belirteç doğrulaması sırasında birden çok kiracıyı işlemek için gerekli mantığa ihtiyacınız olduğundan, uygulamanızı geliştirirken göz önünde bulundurulması gereken ortak bitiş noktası özellikle önemlidir.

Şu anda tek bir kiracı uygulaması geliştiriyorsanız ancak birçok kuruluş için kullanılabilir hale getirmek istiyorsanız, çok kiracılı olabilir hale getirmek için uygulamada ve Azure AD'deki yapılandırmasında kolayca değişiklik yapabilirsiniz. Ayrıca Azure AD, ister tek bir kiracıda ister çok kiracılı uygulamada kimlik doğrulaması sağlıyor olun, tüm dizinlerdeki tüm belirteçler için aynı imzalama anahtarını kullanır.

Bu belgede listelenen her senaryo, sağlama gereksinimlerini açıklayan bir alt bölüm içerir. Azure AD'de bir uygulama sağlama hakkında daha ayrıntılı bilgi ve tek ve çok kiracılı uygulamalar arasındaki farklar için daha fazla bilgi için [uygulamaları Azure Etkin Dizini ile tümleştirme](../develop/single-and-multi-tenant-apps.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) konusuna bakın. Azure AD'deki yaygın uygulama senaryolarını anlamak için okumaya devam edin.

## <a name="next-steps"></a>Sonraki adımlar

- Diğer Azure AD [kimlik doğrulama temelleri](v1-authentication-scenarios.md) hakkında daha fazla bilgi edinin
