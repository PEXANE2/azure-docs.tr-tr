---
title: Azure Active Directory B2C'de kullanıcı akışları | Microsoft Dokümanlar
description: Azure Active Directory B2C'nin genişletilebilir ilke çerçevesi ve çeşitli kullanıcı akışlarının nasıl oluşturulması hakkında daha fazla bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c11bc48742c398d2048a236c7d00af044971f845
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78185631"
---
# <a name="user-flows-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de kullanıcı akışları

Azure Active Directory B2C'nin (Azure AD B2C) genişletilebilir ilke çerçevesi, hizmetin temel gücüdür. İlkeler, kaydolma, kaydolma veya profil düzenleme gibi kimlik deneyimlerini tam olarak açıklar. Azure AD B2C portalı, en yaygın kimlik görevlerini ayarlamanıza yardımcı olmak için **kullanıcı akışları**adı verilen önceden tanımlanmış, yapılandırılabilir ilkeler içerir.

## <a name="what-are-user-flows"></a>Kullanıcı akışları nedir?

Kullanıcı akışı, aşağıdaki ayarları yapılandırarak uygulamalarınızdaki davranışları denetlemenize olanak tanır:

- Facebook veya yerel hesaplar gibi sosyal hesaplar gibi oturum açma için kullanılan hesap türleri
- İlk ad, posta kodu ve ayakkabı boyutu gibi tüketiciden toplanacak nitelikler
- Azure Multi-Factor Authentication
- Kullanıcı arabiriminin özelleştirmesi
- Uygulamanın bir belirteçte talep olarak aldığı bilgiler

Kiracınızda farklı türde birçok kullanıcı akışı oluşturabilir ve bunları gerektiğinde uygulamalarınızda kullanabilirsiniz. Kullanıcı akışları uygulamalar arasında yeniden kullanılabilir. Bu esneklik, kodunuzda en az veya hiç değişiklik olmadan kimlik deneyimlerini tanımlamanızı ve değiştirmenizi sağlar. Uygulamanız, kullanıcı akış parametresi içeren standart bir HTTP kimlik doğrulama isteği kullanarak kullanıcı akışını tetikler. Yanıt olarak özelleştirilmiş bir [belirteç](tokens-overview.md) alınır.

Aşağıdaki örnekler, kullanılacak kullanıcı akışını belirten "p" sorgu dize parametresini gösterir:

```
https://contosob2c.b2clogin.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up user flow
```

```
https://contosob2c.b2clogin.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in user flow
```

## <a name="user-flow-versions"></a>Kullanıcı akışı sürümleri

Azure portalında, [kullanıcı akışlarının](user-flow-versions.md) yeni sürümleri her zaman eklenmektedir. Azure AD B2C ile başladığınızda, test edilmiş kullanıcı akışlarının kullanmanız önerilir. Yeni bir kullanıcı akışı oluşturduğunuzda, **Önerilen** sekmeden gereksinim duyduğunuz kullanıcı akışını seçersiniz.

Aşağıdaki kullanıcı akışları şu anda önerilir:

- **Kaydolun ve oturum açın** - Tek bir yapılandırmayla hem kaydolma hem de oturum açma deneyimlerini işler. Kullanıcılar içeriğe bağlı olarak doğru yolda yönlendirilir. Bu kullanıcı **akışını, kaydolma** kullanıcı akışı veya **oturum açma** kullanıcı akışı üzerinde kullanmanız önerilir.
- **Profil düzenleme** - Kullanıcıların profil bilgilerini düzenlemelerini sağlar.
- **Parola sıfırlama** - Kullanıcıların parolalarını sıfırlayıp sıfırlayamayacağını ve nasıl sıfırlayabildiğini yapılandırmanızı sağlar.

## <a name="linking-user-flows"></a>Kullanıcı akışlarını bağlama

Yerel hesaplarla **kaydolma veya kaydolma** kullanıcı akışı, deneyimin ilk sayfasında ki **Bir Şifreyi Unuttum?** Bu bağlantıyı tıklattığınızda otomatik olarak bir parola sıfırlama kullanıcı akışını tetiklemez.

Bunun yerine, `AADB2C90118` hata kodu uygulamanıza döndürülür. Uygulamanızın, parolayı sıfırlayan belirli bir kullanıcı akışını çalıştırarak bu hata kodunu işlemesi gerekir. Bir örnek görmek için, kullanıcı akışlarının bağlantısını gösteren basit bir [ASP.NET örneğine](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI) göz atın.

## <a name="email-address-storage"></a>E-posta adresi depolama

Kullanıcı akışının bir parçası olarak bir e-posta adresi gerekebilir. Kullanıcı bir sosyal kimlik sağlayıcısıyla kimlik doğrulaması yaparsa, e-posta adresi **otherMails** özelliğinde depolanır. Yerel bir hesap bir kullanıcı adına dayanıyorsa, e-posta adresi güçlü bir kimlik doğrulama ayrıntısı özelliğinde depolanır. Yerel bir hesap bir e-posta adresine dayanıyorsa, e-posta adresi **signInNames** özelliğinde depolanır.

E-posta adresinin bu durumların hiçbirinde doğrulanaması garanti değildir. Kiracı yönetici, yerel hesapların temel ilkelerinde e-posta doğrulamasını devre dışı kılabilir. E-posta adresi doğrulaması etkinleştirilmiş olsa bile, adresler bir sosyal kimlik sağlayıcısından geliyorsa ve değiştirilmese doğrulanmaz.

Microsoft Graph API'si aracılığıyla yalnızca **otherMail'ler** ve **signInNames** özellikleri ortaya çıkarır. Güçlü kimlik doğrulama ayrıntısı özelliğindeki e-posta adresi kullanılamıyor.

## <a name="next-steps"></a>Sonraki adımlar

Önerilen kullanıcı akışlarını oluşturmak için, Öğretici' deki yönergeleri [izleyin: Bir kullanıcı akışı oluşturun.](tutorial-create-user-flows.md)
