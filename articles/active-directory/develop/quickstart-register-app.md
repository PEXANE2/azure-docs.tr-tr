---
title: "Hızlı başlangıç: uygulamayı Microsoft Identity platformu 'nda kaydetme | Mavisi"
description: Bu hızlı başlangıçta, bir uygulamayı Microsoft Identity platformu ile nasıl kaydedeceğinizi öğrenirsiniz.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 09/03/2020
ms.author: marsma
ms.custom: aaddev, identityplatformtop40, contperfq1
ms.reviewer: aragra, lenalepa, sureshja
ms.openlocfilehash: 5f34215d57bd5dae8c9a5e6e8f4630b7ed0c827e
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89436751"
---
# <a name="quickstart-register-an-application-with-the-microsoft-identity-platform"></a>Hızlı başlangıç: Microsoft Identity platformu ile uygulama kaydetme

Bu hızlı başlangıçta, Microsoft kimlik platformunun uygulamanız ve kullanıcıları için kimlik doğrulama ve yetkilendirme hizmetleri sağlayabilmesi adına bir uygulamayı Azure portal kaydedersiniz.

Microsoft Identity platformunun kimlik ve erişim yönetimi (ıAM) gerçekleştirmesini istediğiniz her uygulama için gereklidir. Bir Web veya mobil uygulama gibi bir istemci uygulaması ya da bir istemci uygulamasını yedekleyen bir Web API 'SI olup olmadığı, kaydetme, uygulamanız ile kimlik sağlayıcısı arasında bir güven ilişkisi oluşturur, Microsoft Identity platform.

## <a name="prerequisites"></a>Önkoşullar

* Etkin aboneliği olan bir Azure hesabı- [ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [Hızlı başlangıç tamamlama: kiracı ayarlama](quickstart-create-new-tenant.md)

## <a name="register-an-application"></a>Bir uygulamayı kaydetme

Uygulamanızı kaydetmek, uygulamanız ve Microsoft Identity platform arasında bir güven ilişkisi kurar. Güven tek yönlü: uygulamanız Microsoft Identity platformu ' na güvendiğinden, farklı bir şekilde değildir.

Uygulama kaydını oluşturmak için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Birden fazla kiracıya erişiminiz varsa, uygulamayı kaydetmek istediğiniz kiracıyı seçmek için üst menüdeki **Dizin + abonelik** filtresini kullanın :::image type="icon" source="./media/quickstart-register-app/portal-01-directory-subscription-filter.png" border="false"::: .
1. **Azure Active Directory**'yi bulun ve seçin.
1. **Yönet**altında **uygulama kayıtları**ve ardından **Yeni kayıt**' ı seçin.
1. Uygulamanız için bir **ad** girin. Uygulamanızın kullanıcıları bu adı görebilir ve daha sonra değiştirebilirsiniz.
1. Bazen *oturum açma hedef kitlesi*olarak adlandırılan, uygulamayı kimlerin kullanacağınızı belirtin.

    | Desteklenen hesap türleri | Açıklama |
    |-------------------------|-------------|
    | **Yalnızca bu kuruluş dizinindeki hesaplar** | *Kiracınızda* yalnızca kullanıcılar (veya konuklar) tarafından kullanılacak bir uygulama oluşturuyorsanız bu seçeneği belirleyin.<br><br>Genellikle, *iş kolu* (LOB) uygulaması olarak adlandırılan bu, Microsoft Identity platformunda **tek kiracılı** bir uygulamadır. |
    | **Herhangi bir kuruluş dizinindeki hesaplar** | *Herhangi bir* Azure AD kiracısındaki kullanıcıların uygulamanızı kullanabilmemizi istiyorsanız bu seçeneği belirleyin. Örneğin, birden çok kuruluşa sağlamak istediğiniz bir hizmet olarak yazılım (SaaS) uygulaması oluşturuyorsanız, bu seçenek uygundur.<br><br>Bu, Microsoft Identity platformunda **çok kiracılı** bir uygulama olarak bilinir. |
    | **Herhangi bir kuruluş dizinindeki hesaplar ve kişisel Microsoft hesapları** | En geniş müşteri kümesini hedeflemek için bu seçeneği belirleyin.<br><br>Bu seçeneği belirlediğinizde, kişisel **Microsoft hesapları** (MSA) ile kullanıcıları destekleyebilen bir **çok kiracılı** uygulamayı kaydediyorsanız. |
    | **Kişisel Microsoft hesapları** | Yalnızca kişisel Microsoft hesapları olan kullanıcılar tarafından kullanılmak üzere bir uygulama oluşturuyorsanız bu seçeneği belirleyin. Kişisel Microsoft hesapları, Skype, Xbox, canlı ve hotmail hesaplarını içerir. |

1. **Yeniden yönlendirme URI 'si (isteğe bağlı)** için hiçbir şey girmeyin, sonraki bölümde bir tane yapılandıracaksınız.
1. İlk uygulama kaydını gerçekleştirmek için **Kaydet** ' i seçin.

    :::image type="content" source="media/quickstart-register-app/portal-02-app-reg-01.png" alt-text="Bir uygulama kaydetme bölmesini gösteren bir Web tarayıcısındaki Azure portal ekran görüntüsü.":::

Kayıt tamamlandığında Azure portal, **uygulamanın (istemci) kimliğini**içeren uygulama kaydının **genel bakış** bölmesini görüntüler. Yalnızca *ISTEMCI kimliği*olarak da adlandırılan bu değer, uygulamanızı Microsoft Identity platformunda benzersiz şekilde tanımlar.

Uygulamanızın kodu veya daha yaygın olarak uygulamanızda kullanılan bir kimlik doğrulama kitaplığı da, kimlik platformundan aldığı güvenlik belirteçlerini doğrulamak için istemci KIMLIĞINI bir boyut olarak kullanır.

:::image type="content" source="media/quickstart-register-app/portal-03-app-reg-02.png" alt-text="Bir uygulama kaydının genel bakış bölmesini gösteren bir Web tarayıcısındaki Azure portal ekran görüntüsü.":::

## <a name="add-a-redirect-uri"></a>Yeniden yönlendirme URI 'SI Ekle

Yeniden yönlendirme URI 'SI, Microsoft Identity platformunun bir kullanıcının istemcisini yeniden yönlendirdiği ve kimlik doğrulamasından sonra güvenlik belirteçleri gönderdiği konumdur.

Bir üretim Web uygulamasında, örneğin, yeniden yönlendirme URI 'SI genellikle uygulamanızın çalıştırıldığı ortak bir uç noktadır `https://contoso.com/auth-response` . Geliştirme sırasında, uygulamanızı yerel olarak çalıştırdığınız uç noktayı (gibi) de eklemek yaygın bir uygulamadır `https://127.0.0.1/auth-response` .

[Platform ayarlarını](#configure-platform-settings)yapılandırarak kayıtlı uygulamalarınız için yeniden yönlendirme URI 'leri ekler ve değiştirirsiniz.

### <a name="configure-platform-settings"></a>Platform ayarlarını yapılandırma

Yeniden yönlendirme URI 'Leri dahil olmak üzere her uygulama türünün ayarları, Azure portal **Platform yapılandırmalarında** yapılandırılır. **Web** ve **tek sayfalı uygulamalar**gibi bazı platformlar, BIR yeniden yönlendirme URI 'sini el ile belirtmenizi gerektirir. Mobil ve Masaüstü gibi diğer platformlarda, diğer ayarlarını yapılandırırken sizin için oluşturulan yeniden yönlendirme URI 'Leri arasından seçim yapabilirsiniz.

Hedeflediğiniz platformu veya cihazı temel alan uygulama ayarlarını yapılandırmak için:

1. Azure portal **uygulama kayıtları** uygulamanızdaki uygulamanızı seçin.
1. **Yönet**altında **kimlik doğrulaması**' nı seçin.
1. **Platform yapılandırması**altında **Platform Ekle**' yi seçin.
1. **Platformları Yapılandır**bölümünde, ayarlarını yapılandırmak için uygulama türü (Platform) kutucuğunu seçin.

    :::image type="content" source="media/quickstart-register-app/portal-04-app-reg-03-platform-config.png" alt-text="Azure portal platform yapılandırma bölmesinin ekran görüntüsü" border="false":::

    | Platform | Yapılandırma ayarları |
    | -------- | ---------------------- |
    | **Web** | Uygulamanız için bir **yeniden yönlendirme URI 'si** girin, Microsoft Identity platformunun bir kullanıcının istemcisini yeniden yönlendirdiği ve kimlik doğrulamasından sonra güvenlik belirteçleri gönderdiği konum.<br/><br/>Sunucuda çalışan standart Web uygulamaları için bu platformu seçin. |
    | **Tek sayfalı uygulama** | Uygulamanız için bir **yeniden yönlendirme URI 'si** girin, Microsoft Identity platformunun bir kullanıcının istemcisini yeniden yönlendirdiği ve kimlik doğrulamasından sonra güvenlik belirteçleri gönderdiği konum.<br/><br/>JavaScript 'te bir istemci tarafı Web uygulaması oluşturuyorsanız veya angular, Vue.js, React.js veya Blazor WebAssembly gibi bir çatı varsa bu platformu seçin. |
    | **iOS/macOS** | *Info. plist* veya Build Settings Içinde Xcode içinde bulunan uygulama **paket kimliğini**girin.<br/><br/>Bir paket KIMLIĞI belirttiğinizde sizin için bir yeniden yönlendirme URI 'SI oluşturulur. |
    | **Android** | *AndroidManifest.xml* dosyasında bulabileceğiniz uygulama **paketi adını**girin ve **imza karmasını**oluşturup girin.<br/><br/>Bu ayarları belirttiğinizde sizin için bir yeniden yönlendirme URI 'SI oluşturulur. |
    | **Mobil ve Masaüstü uygulamaları** | **Önerilen yeniden yönlendirme** URI 'lerinden birini seçin veya özel bir **yeniden yönlendirme URI 'si**belirtin.<br/>Masaüstü uygulamaları için şunları yapmanızı öneririz:<br/>`https://login.microsoftonline.com/common/oauth2/nativeclient`<br/><br/>En son Microsoft kimlik doğrulama kitaplığı 'nı (MSAL) kullanmayan veya bir aracı kullanmayan mobil uygulamalar için bu platformu seçin. Masaüstü uygulamaları için de bu platformu seçin. |
1. Platform yapılandırmasını gerçekleştirmek için **Yapılandır** ' ı seçin.

### <a name="redirect-uri-restrictions"></a>Yeniden yönlendirme URI kısıtlamaları

Uygulama kaydına eklediğiniz yeniden yönlendirme URI 'Leri biçiminde bazı kısıtlamalar vardır. Bu kısıtlamalarla ilgili ayrıntılar için bkz. [yeniden YÖNLENDIRME URI 'si (yanıt URL 'si) kısıtlamaları ve sınırlamaları](reply-url.md).

## <a name="add-credentials"></a>Kimlik bilgileri ekle

Kimlik bilgileri, bir Web API 'sine erişen gizli istemci uygulamaları tarafından kullanılır. Gizli istemci örnekleri Web Apps, diğer Web API 'Leri veya hizmet ve Daemon türünde uygulamalardır. Kimlik bilgileri, uygulamanızın çalışma zamanında Kullanıcı etkileşimi gerektirmeksizin kendisi olarak kimlik doğrulaması yapmasına olanak sağlar.

Gizli istemci uygulaması kaydınız için kimlik bilgileri olarak hem sertifikaları hem de istemci gizli dizilerini (bir dize) ekleyebilirsiniz.

:::image type="content" source="media/quickstart-register-app/portal-05-app-reg-04-credentials.png" alt-text="Uygulama kaydında sertifikalar ve gizlilikler bölmesini gösteren Azure portal ekran görüntüsü":::

### <a name="add-a-certificate"></a>Sertifika ekle

Bazen *ortak anahtar*olarak da bilinen sertifikalar, istemci gizliliğine göre daha yüksek bir güvence düzeyi sağladıkları için önerilen kimlik bilgisi türüdür.

1. Azure portal **uygulama kayıtları** uygulamanızdaki uygulamanızı seçin.
1. **Sertifikalar & parolaları**  >  **karşıya yükleme sertifikası**' nı seçin.
1. Yüklemek istediğiniz dosyayı seçin. Şu dosya türlerinden biri olmalıdır: .cer, .pem, .crt.
1. **Add (Ekle)** seçeneğini belirleyin.

### <a name="add-a-client-secret"></a>İstemci parolası ekleme

*Uygulama parolası*olarak da bilinen istemci parolası, uygulamanızın kimliğini kimlik için bir sertifika yerine kullanabileceği bir dize değeridir. Bu iki kimlik bilgisi türünün kullanılması kolaylaşır ve genellikle geliştirme sırasında kullanılır, ancak bir sertifikadan daha az güvenli olarak değerlendirilir. Üretimde çalışan uygulamalarınızda sertifikalar kullanmanız gerekir.

1. Azure portal **uygulama kayıtları** uygulamanızdaki uygulamanızı seçin.
1. **Sertifikalar & parolaları**seçin  >   **yeni istemci gizli anahtarı**.
1. İstemci gizli diziniz için bir açıklama ekleyin.
1. Bir süre seçin.
1. **Add (Ekle)** seçeneğini belirleyin.
1. **Gizli anahtarı** istemci uygulama kodunuzda kullanım için kaydedin-bu sayfadan ayrıldıktan sonra *hiç bir daha gösterilmez* .

## <a name="next-steps"></a>Sonraki adımlar

İstemci uygulamalarının genellikle bir Web API 'sindeki kaynaklara erişmesi gerekir. İstemci uygulamanızı Microsoft Identity platformu ile korumanın yanı sıra, Web API 'nize kapsamlı ve izin tabanlı erişim yetkisi veren platformu kullanabilirsiniz.

Web API 'niz için başka bir uygulama kaydı oluşturmak ve kapsamlarını göstermek için serideki bir sonraki hızlı başlangıca geçin.

> [!div class="nextstepaction"]
> [Web API 'sini kullanıma sunmak için uygulama yapılandırma](quickstart-configure-app-expose-web-apis.md)
