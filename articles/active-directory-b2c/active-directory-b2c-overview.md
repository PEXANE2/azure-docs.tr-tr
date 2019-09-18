---
title: Azure Active Directory B2C nedir? | Microsoft Docs
description: Azure Active Directory B2C kullanarak uygulamanızdaki kaydolma ve profil yönetimi gibi kimlik deneyimlerini oluşturma ve yönetme hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 02/20/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: ca636079439f811a887d16b627473e7d73930799
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71065703"
---
# <a name="what-is-azure-active-directory-b2c"></a>Azure Active Directory B2C nedir?

Azure Active Directory B2C (Azure AD B2C), işletmeden müşteriye kimlik yönetimi hizmetidir. Bu hizmet, kullanıcıların Web, Masaüstü, mobil veya tek sayfalı uygulamalarla nasıl güvenli bir şekilde etkileşim kuracağını özelleştirmenize ve denetlemenize olanak sağlar. Azure AD B2C kullanarak, kullanıcılar kaydolabilir, oturum açabilir, parolaları sıfırlayabilir ve profilleri düzenleyebilir. Azure AD B2C, OpenID Connect ve OAuth 2,0 protokollerinin bir formunu uygular. Bu protokollerin uygulanmasında önemli anahtar, kaynaklara güvenli erişim sağlamanıza olanak tanıyan güvenlik belirteçleridir ve talepleridir.

*Kullanıcı yolculuğu* , Kullanıcı ve uygulamanızın Azure AD B2C ile nasıl etkileşime gireceğini denetleyen bir ilkeyi belirten bir istedir. Azure AD B2C Kullanıcı yolculukları tanımlamak için kullanabileceğiniz iki yol vardır.

Kimlik uzmanlığına sahip veya olmayan bir uygulama geliştiricisiyseniz, Azure portal kullanarak ortak kimlik Kullanıcı akışları tanımlamanızı tercih edebilirsiniz. Kimlik uzmanı, Sistem Tümleştirici, danışman veya şirket içi kimlik ekibinizde, OpenID Connect akışlarıyla rahat ve kimlik sağlayıcılarını ve talep tabanlı kimlik doğrulamasını anladıysanız, XML tabanlı özel ilkeler seçebilirsiniz.

Kullanıcı yolculuğu tanımlamaya başlamadan önce bir Azure AD B2C kiracı oluşturmanız ve uygulamanızı ve API 'nizi kiracıya kaydetmeniz gerekir. Bu görevleri tamamladıktan sonra, Kullanıcı akışlarını veya özel ilkeleri kullanarak bir Kullanıcı yolculuğu tanımlamayı kullanmaya başlamanızı sağlayabilirsiniz. İsteğe bağlı olarak, kimlik sağlayıcıları ekleyebilir veya değiştirebilir ya da kullanıcının yolculuğun karşılaştığı yolu özelleştirebilirsiniz.

## <a name="protocols-and-tokens"></a>Protokoller ve belirteçler

Azure AD B2C, Kullanıcı yolculukları için [OpenID Connect ve OAuth 2,0 protokollerini](active-directory-b2c-reference-protocols.md) destekler. OpenID Connect'in Azure AD B2C gerçekleştirmesinde uygulamanız kullanıcı yolculuğunu Azure AD B2C'ye kimlik doğrulama istekleri göndererek başlatır.

Azure AD B2C bir isteğin sonucu, [kimlik belirteci veya erişim belirteci](active-directory-b2c-reference-tokens.md)gibi bir güvenlik belirtecidir. Bu güvenlik belirteci, kullanıcının kimliğini tanımlar. Belirteçler, `/token` veya `/authorize` uç nokta gibi Azure AD B2C uç noktalarından alınır. Bu belirteçlerden, bir kimliği doğrulamak ve güvenli kaynaklara erişim sağlamak için kullanılabilecek taleplere erişebilirsiniz.

## <a name="tenants-and-applications"></a>Kiracılar ve uygulamalar

Azure AD B2C, *kiracı* kuruluşunuzu temsil eder ve bir Kullanıcı dizinidir. Her Azure AD B2C kiracısı benzersizdir ve diğer Azure AD B2C kiracılarından ayrıdır. Zaten bir Azure Active Directory kiracınız olabilir Azure AD B2C kiracısı farklı bir kiracıya sahip olabilirsiniz. Bir kiracı, uygulamanızı kullanmak için kaydolan kullanıcılar hakkında bilgiler içerir. Buna parolalar, profil verileri ve izinler dahil olabilir. Daha fazla bilgi için bkz [. Öğretici: Azure Active Directory B2C kiracı](tutorial-create-tenant.md)oluşturun.

Uygulamanızı Azure AD B2C kullanacak şekilde yapılandırmadan önce, önce Azure portal kullanarak kiracı 'ya kaydetmeniz gerekir. Kayıt işlemi değerler toplar ve bunları uygulamanıza atar. Bu değerler, uygulamayı benzersiz bir şekilde tanımlayan bir uygulama KIMLIĞI ve yanıtları uygulamaya geri yönlendirmek için kullanılan bir yeniden yönlendirme URI 'SI içerir.

Her uygulamanın etkileşimini benzer bir üst düzey deseni izler:

1. Uygulama, kullanıcıyı bir ilkeyi çalıştırmak için yönlendirir.
2. Kullanıcı, ilkeyi ilke tanımına göre tamamlar.
3. Uygulama bir belirteç alır.
4. Uygulama, bir kaynağa erişmeyi denemek için belirtecini kullanır.
5. Kaynak sunucu, erişimin verilebildiğini doğrulamak için belirteci doğrular.
6. Uygulama belirteci düzenli aralıklarla yeniler.

Bir Web uygulamasını kaydetmek için [öğreticideki adımları uygulayın: Azure AD B2C](tutorial-register-applications.md)kullanarak kaydolmayı ve oturum açmayı etkinleştirmek için bir uygulamayı kaydedin. Ayrıca, [Azure Active Directory B2C kiracınıza bir Web API uygulaması ekleyebilir](add-web-application.md) veya [Azure Active Directory B2C kiracınıza bir yerel istemci uygulaması ekleyebilirsiniz](add-native-application.md).

## <a name="user-journeys"></a>Kullanıcı yolculukları

Kullanıcı yolculuğu içindeki ilke, [Kullanıcı akışı](active-directory-b2c-reference-policies.md) veya [özel bir ilke](active-directory-b2c-overview-custom.md)olarak tanımlanabilir. Kaydolma, oturum açma ve profil düzenlemesi gibi en yaygın kimlik görevlerinin ön tanımlı kullanıcı akışları Azure portal kullanılabilir.

Kullanıcı yolculukları aşağıdaki ayarları yapılandırarak davranışları denetlemenize izin verir:

- Kullanıcının uygulamaya kaydolmak için kullandığı sosyal hesaplar
- Kullanıcıdan toplanan ve ilk ad veya posta kodu gibi veriler
- Multi-factor authentication
- Sayfaların görünümü ve kullanım
- Uygulamaya döndürülen bilgiler

Özel ilkeler, Azure AD B2C kiracınızdaki [kimlik deneyimi çerçevesinin](trustframeworkpolicy.md) davranışını tanımlayan yapılandırma dosyalarıdır. Kimlik deneyimi çerçevesi, çok taraflı güven kuran ve Kullanıcı yolculuğu içindeki adımları tamamlayan temel alınan platformdur.

Özel ilkeler, birçok görevi yapacak şekilde değiştirilebilir. Özel ilke, bir hiyerarşi zincirinde birbirine başvuran bir veya daha fazla XML biçimli dosyadır. Ortak kimlik görevlerini etkinleştirmek için özel ilkeler için bir [başlangıç paketi](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) mevcuttur.

Farklı türlerin özel ilkeleri veya Kullanıcı akışları, Azure AD B2C kiracınızda gerektiği şekilde kullanılır ve uygulamalar arasında yeniden kullanılabilir. Bu esneklik, kodunuzda en az değişiklik yapmadan kullanıcı kimliği deneyimlerini tanımlamanızı ve değiştirmenizi sağlar. İlkeler HTTP kimlik doğrulaması isteklerine özel bir sorgu parametresi eklenerek kullanılır. Kendi özel ilkenizi oluşturmak için, bkz. [Azure Active Directory B2C özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md).

## <a name="identity-providers"></a>Kimlik sağlayıcıları

Uygulamalarınızda, kullanıcıların farklı kimlik sağlayıcılarıyla oturum açmasını sağlamak isteyebilirsiniz. Kimlik *sağlayıcısı* , uygulamalara kimlik doğrulama hizmetleri sağlarken kimlik bilgilerini oluşturur, korur ve yönetir. Azure portal kullanarak Azure AD B2C tarafından desteklenen kimlik sağlayıcıları ekleyebilirsiniz.

Uygulamanızda genellikle yalnızca bir kimlik sağlayıcısı kullanırsınız, ancak daha fazla ekleme seçeneğiniz vardır. Azure AD B2C kiracınızda bir kimlik sağlayıcısı yapılandırmak için öncelikle kimlik sağlayıcısı geliştirici sitesinde bir uygulama oluşturun ve ardından kimlik sağlayıcısından uygulama tanımlayıcısını veya istemci tanımlayıcısını ve parolayı veya istemci gizli anahtarını kaydedersiniz oluşturduğunuz uygulama. Bu tanımlayıcı ve parola daha sonra uygulamanızı yapılandırmak için kullanılır.

Aşağıdaki makalelerde, Kullanıcı akışlarına ortak kimlik sağlayıcılarının bazılarını ekleme adımları açıklanır:

- [Amazon](active-directory-b2c-setup-amzn-app.md)
- [Facebook](active-directory-b2c-setup-fb-app.md)
- [Microsoft hesabı](active-directory-b2c-setup-msa-app.md)

Aşağıdaki makaleler, bazı ortak kimlik sağlayıcılarını özel ilkelere ekleme adımlarını anlatmaktadır:
- [Amazon](setup-amazon-custom.md)
- [Google](active-directory-b2c-custom-setup-goog-idp.md)
- [Microsoft hesabı](active-directory-b2c-custom-setup-msa-idp.md)

Daha fazla bilgi için bkz [. Öğretici: Azure Active Directory B2C](tutorial-add-identity-providers.md)' deki uygulamalarınıza kimlik sağlayıcıları ekleyin.


## <a name="page-customization"></a>Sayfa özelleştirme

Bir Kullanıcı yolculuğunda müşterilere sunulan HTML ve CSS içeriğinin çoğu denetlenebilir. Sayfa özelleştirmesini kullanarak, herhangi bir özel ilkenin veya Kullanıcı akışının görünümünü özelleştirebilirsiniz. Uygulamanız ve Azure AD B2C arasında marka ve görsellik tutarlılığını bu özelleştirme işlevini kullanarak korursunuz.

Azure AD B2C, kullanıcının tarayıcısında kodu çalıştırır ve çıkış noktaları arası kaynak paylaşımı (CORS) adlı modern bir yaklaşım kullanır. İlk olarak özelleştirilmiş HTML içeriğine sahip ilkede bir URL belirtmeniz gerekir. Azure AD B2C, Kullanıcı arabirimi öğelerini URL 'nizden yüklenen HTML içeriğiyle birleştirir ve ardından sayfayı kullanıcıya görüntüler.

Bir sorgu dizesi kullanarak Azure AD B2C'ye parametre gönderirsiniz. Parametre HTML uç noktanıza iletilerek sayfa içeriği dinamik olarak değiştirilir. Örneğin, kayıt veya oturum açma sayfasındaki arka plan görüntüsünü, Web veya mobil uygulamanızdan geçirdiğiniz bir parametreye göre değiştirirsiniz.

Bir Kullanıcı akışındaki sayfaları özelleştirmek için bkz [. Öğretici: Azure Active Directory B2C](tutorial-customize-ui.md)Kullanıcı deneyimlerinin arayüzünü özelleştirin. Özel bir ilkedeki sayfaları özelleştirmek için bkz. [Azure Active Directory B2C özel bir ilke kullanarak uygulamanızın kullanıcı arabirimini özelleştirme](active-directory-b2c-ui-customization-custom.md) veya [Azure Active Directory B2C özel ilkeleri kullanarak Kullanıcı arabirimini Dinamik içerikle yapılandırma](active-directory-b2c-ui-customization-custom-dynamic.md).

## <a name="developer-resources"></a>Geliştirici Kaynakları

### <a name="client-applications"></a>İstemci uygulamaları

[İOS](active-directory-b2c-devquickstarts-ios.md), [Android](active-directory-b2c-devquickstarts-android.md)ve .NET uygulamaları için diğer uygulamalar arasından seçim yapabilirsiniz. Azure AD B2C, kullanıcı kimliklerinizi aynı anda korurken bu eylemleri mümkün.

ASP.NET Web uygulaması geliştiricisiyseniz, [öğreticideki adımları kullanarak, hesapların kimliğini doğrulamak için uygulamanızı ayarlayın: Bir Web uygulamasının Azure AD B2C](active-directory-b2c-tutorials-web-app.md)kullanarak hesaplarla kimlik doğrulaması yapmasını etkinleştirin.

Bir masaüstü uygulama geliştiricisiyseniz, [öğreticideki adımları kullanarak, hesapların kimliğini doğrulamak için uygulamanızı ayarlayın: Bir masaüstü uygulamasının Azure AD B2C](active-directory-b2c-tutorials-desktop-app.md)kullanarak hesaplarla kimlik doğrulaması yapmasını etkinleştirin.

Node. js kullanan tek sayfalı bir uygulama geliştiricisiyseniz, [öğreticideki adımları kullanarak, hesapların kimliğini doğrulamak için uygulamanızı ayarlayın: Tek sayfalı bir uygulamanın Azure AD B2C](active-directory-b2c-tutorials-spa.md)kullanarak hesaplarla kimlik doğrulaması yapmasını etkinleştirin.

### <a name="apis"></a>API'ler
İstemci veya Web uygulamalarınızın API 'Leri çağırması gerekiyorsa, Azure AD B2C bu kaynaklara güvenli erişim ayarlayabilirsiniz.

Bir ASP.NET Web uygulaması geliştiriciyseniz, uygulamanızı [öğreticideki adımları kullanarak korumalı bir API çağrısı olacak şekilde ayarlayın: Azure Active Directory B2C](active-directory-b2c-tutorials-web-api.md)kullanarak BIR ASP.NET Web API 'sine erişim izni verin.

Bir masaüstü uygulama geliştiricisiyseniz, [öğreticideki adımları kullanarak uygulamanızı korumalı bir API çağrısı olacak şekilde ayarlayın: Azure Active Directory B2C](active-directory-b2c-tutorials-desktop-app-webapi.md)kullanarak bir masaüstü uygulamasından Node. js web API 'sine erişim izni verin.

Node. js kullanan tek sayfalı bir uygulama geliştiricisiyseniz, [öğreticideki adımları kullanarak, hesapların kimliğini doğrulamak için uygulamanızı ayarlayın: Azure Active Directory B2C](active-directory-b2c-tutorials-spa-webapi.md)kullanarak tek sayfalı bir uygulamadan ASP.NET Core Web API 'sine erişim izni verin.

### <a name="javascript"></a>JavaScript

Kendi JavaScript istemci tarafı kodunuzu, Azure AD B2C uygulamalarınıza ekleyebilirsiniz. Uygulamanızda JavaScript ayarlamak için, bir [sayfa düzeni](page-layout.md) tanımlar ve Kullanıcı akışlarınızda veya özel ilkeleriniz üzerinde [JavaScript 'i](javascript-samples.md) etkinleştirirsiniz.

### <a name="user-accounts"></a>Kullanıcı hesapları

Birçok ortak Kiracı Yönetimi görevinin programlı olarak gerçekleştirilmesi gerekir. Birincil örnek, Kullanıcı yönetimi olur. Mevcut bir Kullanıcı deposunu Azure AD B2C kiracısına geçirmeniz gerekebilir. Kullanıcı kaydını kendi sayfanızda barındırmak ve arka planda Azure AD B2C dizininizde Kullanıcı hesapları oluşturmak isteyebilirsiniz. Bu tür görevler, Kullanıcı hesapları oluşturma, okuma, güncelleştirme ve silme yeteneği gerektirir. [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md)kullanarak bu görevleri gerçekleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticiye devam ederek uygulamanızın kaydolma ve oturum açma deneyimini yapılandırmaya başlayın.

> [!div class="nextstepaction"]
> [Öğretici: Azure Active Directory B2C kiracı oluşturma](tutorial-create-tenant.md)
