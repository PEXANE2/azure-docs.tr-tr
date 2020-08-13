---
title: "Hızlı başlangıç: bir uygulamayı bir Web API 'sine erişecek şekilde yapılandırma | Mavisi"
titleSuffix: Microsoft identity platform
description: Bu hızlı başlangıçta, Web API 'Lerine erişim için yeniden yönlendirme URI 'Leri, kimlik bilgileri veya izinleri içerecek şekilde Microsoft Identity platformunda kayıtlı bir uygulamayı yapılandırırsınız.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 08/05/2020
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: lenalepa, aragra, sureshja
ms.openlocfilehash: 87c21587567ffe3462e4b702985114ac10454886
ms.sourcegitcommit: a2a7746c858eec0f7e93b50a1758a6278504977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88140811"
---
# <a name="quickstart-configure-a-client-application-to-access-a-web-api"></a>Hızlı başlangıç: bir istemci uygulamasını bir Web API 'sine erişmek için yapılandırma

Bu hızlı başlangıçta, uygulamanız için Web API 'Lerine erişim için yeniden yönlendirme URI 'Leri, kimlik bilgileri veya izinleri eklersiniz. Bir Web veya gizli istemci uygulamasının, kimlik doğrulaması gerektiren bir yetkilendirme verme akışına katılması için güvenli kimlik bilgileri kurması gerekir. Azure portal tarafından desteklenen varsayılan kimlik doğrulaması yöntemi istemci kimliği ve gizli anahtar kullanımıdır. Uygulama, bu işlem sırasında bir erişim belirteci alır.

İstemci, Microsoft Graph API gibi bir kaynak uygulama tarafından kullanıma sunulan bir Web API 'sine erişebilinceye kadar, izin çerçevesi istemcinin istenen izinler için gereken izin iznini almasını sağlar. Varsayılan olarak, tüm uygulamalar Microsoft Graph API 'sinden izin isteyebilir.

## <a name="prerequisites"></a>Ön koşullar

* Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Hızlı başlangıç: bir uygulamayı bir Web API 'si göstermek Için yapılandırma](quickstart-configure-app-expose-web-apis.md).

## <a name="sign-in-to-the-azure-portal-and-select-the-app"></a>Azure portalında oturum açın ve uygulamayı seçin

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Hesabınız birden fazla kiracıya erişim veriyorsa, sağ üst köşedeki hesabınızı seçin. Portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
1. **Azure Active Directory**'yi bulun ve seçin. **Yönet**'in altında **Uygulama kayıtları**nı seçin.
1. Yapılandırmak istediğiniz uygulamayı bulun ve seçin. Uygulamayı seçtikten sonra, uygulamanın **genel bakış** veya ana kayıt sayfasını görürsünüz.

Uygulamanızı Web API 'Lerine erişecek şekilde yapılandırmak için aşağıdaki yordamları kullanın.

## <a name="add-redirect-uris-to-your-application"></a>Uygulamanıza yeniden yönlendirme URI’leri ekleme

Uygulamanıza özel yeniden yönlendirme URI 'leri ve önerilen yeniden yönlendirme URI 'Leri ekleyebilirsiniz. Web ve genel istemci uygulamaları için özel bir yeniden yönlendirme URI 'SI eklemek için:

1. Uygulamaya **genel bakış** sayfasında **kimlik doğrulaması**' nı seçin.
1. **Yeniden yönlendirme URI 'lerini**bulun. **Eski deneyime geç**' i seçmeniz gerekebilir.
1. Oluşturmakta olduğunuz uygulamanın türünü seçin: **Web** veya **Public Client/native (Mobile & Desktop)**.
1. Uygulamanız için Yeniden Yönlendirme URI’sini girin.

   * Web uygulamaları için, uygulamanızın temel URL'sini girin. Örneğin `http://localhost:31544` yerel makinenizde çalışan bir web uygulamasının URL'si olabilir. Kullanıcılar, bir web istemci uygulamasında oturum açmak için bu URL'yi kullanır.
   * Genel uygulamalar için, Azure AD'nin belirteç yanıtlarını döndürmek üzere kullandığı URI'yi girin. Uygulamanıza özgü bir değer girin, örneğin: `https://MyFirstApp` .
1. **Kaydet**’i seçin.

Ortak istemciler için önerilen yeniden yönlendirme URI 'lerinden seçim yapmak için şu adımları izleyin:

1. Uygulamaya **genel bakış** sayfasında **kimlik doğrulaması**' nı seçin.
1. **Ortak istemciler Için önerilen yeniden yönlendirme URI 'lerini bulun (mobil, masaüstü)**. **Eski deneyime geç**' i seçmeniz gerekebilir.
1. Uygulamanız için bir veya daha fazla yeniden yönlendirme URI 'si seçin. Ayrıca, özel bir yeniden yönlendirme URI 'SI de girebilirsiniz. Ne kullandığınızdan emin değilseniz, kitaplık belgelerine bakın.
1. **Kaydet**’i seçin.

Belirli kısıtlamalar URI 'Leri yeniden yönlendirme için geçerlidir. Daha fazla bilgi için bkz. [yeniden YÖNLENDIRME URI/yanıt URL kısıtlamaları ve sınırlamaları](./reply-url.md).

> [!NOTE]
> Hedeflemek istediğiniz platforma veya cihaza göre uygulamanızın ayarlarını yapılandırabileceğiniz yeni **kimlik doğrulama** ayarları deneyimini deneyin.
>
> Bu görünümü görmek için **kimlik doğrulama** sayfasından **Yeni deneyimi deneyin** ' i seçin.
>
> ![Platform yapılandırma görünümünü görmek için "yeni deneyimi deneyin" seçeneğine tıklayın](./media/quickstart-update-azure-ad-app-preview/authentication-try-new-experience-cropped.png)
>
> Bu sizi [Yeni **Platform konfigürasyonları** sayfasına](#configure-platform-settings-for-your-application)götürür.

### <a name="configure-advanced-settings-for-your-application"></a>Uygulamanızın gelişmiş ayarlarını yapılandırma

Kayıt yaptığınız uygulamaya bağlı olarak, yapılandırmanız gerekebilecek bazı ek ayarlar vardır, örneğin:

* **Oturum kapatma URL 'si**.
* Tek sayfalı uygulamalar için, **örtük izni** etkinleştirebilir ve yetkilendirme uç noktasının vermesini istediğiniz belirteçleri seçebilirsiniz.
* **Varsayılan istemci türü** bölümünde tümleşik Windows kimlik doğrulaması, cihaz kodu akışı veya Kullanıcı adı/parola kullanarak belirteçleri elde eden masaüstü uygulamaları için, **uygulamayı ortak istemci olarak değerlendir** ayarını **Evet**olarak ayarlayın.
* Microsoft hesabı hizmetiyle tümleştirilecek canlı SDK 'yı kullanan eski uygulamalar için, **LIVE SDK desteği**' ni yapılandırın. Yeni uygulamalara bu ayara gerek yoktur.
* **Varsayılan istemci türü**.
* **Desteklenen hesap türleri**.

### <a name="modify-supported-account-types"></a>Desteklenen hesap türlerini değiştirme

**Desteklenen hesap türleri** , uygulamayı KULLANABILECEK veya API 'ye erişebilecek kişileri belirtir.

Uygulamayı kaydettiğinizde desteklenen hesap türlerini yapılandırdıysanız, bu ayarı yalnızca şu durumlarda uygulama bildirimi düzenleyicisini kullanarak değiştirebilirsiniz:

* Hesap türlerini **Azureadmyorg** veya **Azureadmultipleorgs** , **Azureadandpersonmicrosoftaccount**veya diğer bir şekilde değiştirin ya da
* Hesap türlerini **Azureadmyorg** 'Den **Azureadmultipleorgs**olarak veya başka bir şekilde değiştirirsiniz.

Mevcut bir uygulama kaydının desteklenen hesap türlerini değiştirmek için `signInAudience` anahtarı güncelleştirin. Daha fazla bilgi için bkz. [uygulama bildirimini yapılandırma](reference-app-manifest.md#configure-the-app-manifest).

## <a name="configure-platform-settings-for-your-application"></a>Uygulamanız için platform ayarlarını yapılandırma

![Platforma veya cihaza göre uygulamanızın ayarlarını yapılandırın](./media/quickstart-update-azure-ad-app-preview/authentication-new-platform-configurations.png)

Uygulama ayarlarını platforma veya cihaza göre yapılandırmak için şunları hedefliyorsanız:

1. **Platform konfigürasyonları** sayfasında **Platform Ekle** ' yi seçin ve kullanılabilir seçeneklerden birini belirleyin.

   ![Platformları Yapılandır sayfasını gösterir](./media/quickstart-update-azure-ad-app-preview/authentication-platform-configurations-configure-platforms.png)

1. Seçtiğiniz platforma bağlı olarak ayarlar bilgilerini girin.

   | Platform                | Yapılandırma ayarları            |
   |-------------------------|-----------------------------------|
   | **Web**              | Uygulamanız için **yeniden yönlendirme URI 'si** girin. |
   | **iOS/macOS**              | Info. plist veya Build Settings içinde XCode içinde bulabileceğiniz uygulama **paket kimliğini**girin. Paket KIMLIĞINI eklemek, otomatik olarak uygulama için bir yeniden yönlendirme URI 'SI oluşturur. |
   | **Android**          | AndroidManifest.xml dosyasında bulabileceğiniz uygulama **paketi adını**sağlayın.<br/>**İmza karmasını**oluşturun ve girin. İmza karmasını eklemek, otomatik olarak uygulama için bir yeniden yönlendirme URI 'SI oluşturur.  |
   | **Mobil ve Masaüstü uygulamaları**  | İsteğe bağlı. Masaüstü ve cihazlar için uygulamalar oluşturuyorsanız önerilen **Önerilen yeniden yönlendirme URI** 'lerinden birini seçin.<br/>İsteğe bağlı. Azure AD 'nin kullanıcıları kimlik doğrulama isteklerine yanıt olarak yeniden yönlendireceği konum olarak kullanılan **özel bir yeniden yönlendirme URI 'si**girin. Örneğin, etkileşim istediğiniz .NET Core uygulamaları için kullanın `http://localhost` . |

   > [!NOTE]
   > Active Directory Federasyon Hizmetleri (AD FS) (AD FS) ve Azure AD B2C bir bağlantı noktası numarası da belirtmeniz gerekir.  Örneğin: `http://localhost:1234`.

   > [!IMPORTANT]
   > En son Microsoft kimlik doğrulama kitaplığı 'nı (MSAL) kullanmayan veya bir aracı kullanmayan mobil uygulamalarda, bu uygulamalar için yeniden yönlendirme URI 'Lerini **Masaüstü + cihazlarda**yapılandırmanız gerekir.

Seçtiğiniz platforma bağlı olarak, yapılandırabileceğiniz ek ayarlar olabilir. **Web** Apps için şunları yapabilirsiniz:

* Daha fazla yeniden yönlendirme URI 'si ekleyin
* Kimlik doğrulama uç noktası tarafından verilmek istediğiniz belirteçleri seçmek için **örtülü izni** yapılandırın:

  * Tek sayfalı uygulamalar için, hem **erişim belirteçlerini** hem de **kimlik belirteçlerini** seçin
  * Web Apps için **Kimlik belirteçleri** seçin

## <a name="add-credentials-to-your-web-application"></a>Web uygulamanıza kimlik bilgileri ekleme

Web uygulamanıza bir kimlik bilgisi eklemek için, bir sertifika ekleyin ya da bir istemci gizli anahtarı oluşturun. Sertifika eklemek için:

1. Uygulamaya **genel bakış** sayfasında, **Sertifikalar & gizlilikler** bölümünü seçin.
1. **Sertifikayı karşıya yükle**’yi seçin.
1. Yüklemek istediğiniz dosyayı seçin. Şu dosya türlerinden biri olmalıdır: .cer, .pem, .crt.
1. **Add (Ekle)** seçeneğini belirleyin.

Bir istemci parolası eklemek için:

1. Uygulamaya **genel bakış** sayfasında, **Sertifikalar & gizlilikler** bölümünü seçin.
1. **Yeni istemci gizli dizisi**’ni seçin.
1. İstemci gizli diziniz için bir açıklama ekleyin.
1. Bir süre seçin.
1. **Add (Ekle)** seçeneğini belirleyin.

> [!NOTE]
> Yapılandırmada yaptığınız değişiklikleri kaydettikten sonra en sağdaki sütunda istemci gizli dizi değeri gösterilir. İstemci uygulamanızın kodunda kullanmak üzere **değeri kopyalamayı unutmayın**. Bu sayfadan ayrıldıktan sonra bu değere erişemezsiniz.

## <a name="add-permissions-to-access-web-apis"></a>Web API’lerine erişim izinleri ekleme

[Graph API oturum açma ve kullanıcı profilini Oku izni](/graph/permissions-reference#user-permissions) varsayılan olarak seçilidir. Her bir Web API 'SI için [iki tür izin](developer-glossary.md#permissions) arasından seçim yapabilirsiniz:

* **Uygulama izinleri**. İstemci uygulamanızın, Kullanıcı bağlamı olmadan doğrudan kendi kendine Web API 'sine erişmesi gerekir. Bu izin türü yönetici onayı gerektirir. Bu izin masaüstü ve mobil istemci uygulamaları için kullanılamaz.
* **Temsilci izinleri**. İstemci uygulamanızın, oturum açmış kullanıcı olarak Web API 'sine erişmesi gerekir, ancak erişim, seçilen izinle sınırlı olarak. Yönetici onayı gerektirmediği sürece bu izin türü bir kullanıcı tarafından verilebilir.

  > [!NOTE]
  > Bir uygulamaya temsilcili izin eklenmesi kiracı içindeki kullanıcılara otomatik olarak onay vermez. Yöneticinin tüm kullanıcıların adına onay vermemesi durumunda kullanıcıların yine çalışma zamanında eklenen temsilcili izinler için el ile onay vermesi gerekir.

İstemcinizden kaynak API 'Lerine erişim izinleri eklemek için:

1. Uygulamaya **genel bakış** sayfasında, **API izinleri**' ni seçin.
1. **Yapılandırılan izinler**altında **izin Ekle**' yi seçin.
1. Varsayılan olarak, görünüm **Microsoft API’leri** arasından seçim yapmanızı sağlar. İlgilendiğiniz API’leri seçin:

    * **Microsoft API 'leri**. Microsoft Graph gibi Microsoft API 'Leri için izinleri seçmenizi sağlar.
    * **Kuruluşumun kullandığı API 'ler**. Kuruluşunuzun sunduğu API 'Ler veya kuruluşunuzun tümleştirildiği API 'ler için izinleri seçmenizi sağlar.
    * **API 'Lerim**. Sergilemiş olan API 'Ler için izinleri seçmenizi sağlar.

1. API’leri seçtikten sonra, **API İzinleri İsteme** sayfasını görürsünüz. API hem temsilci hem de uygulama izinlerini kullanıma sunuyorsa, uygulamanız için gereken izin türünü seçin.
1. İşiniz bittiğinde, **İzinleri ekle** seçeneğini belirleyin.

**API izinleri** sayfasına dönersiniz. İzinler kaydedildi ve tabloya eklendi.

## <a name="understanding-api-permissions-and-admin-consent-ui"></a>API izinlerini ve yönetici onayı Kullanıcı arabirimini anlama

### <a name="configured-permissions"></a>Yapılandırılan izinler

Bu bölümde, uygulama nesnesi üzerinde açıkça yapılandırılmış izinler gösterilmektedir. Bu izinler, uygulamanın gerekli kaynak erişim listesinin bir parçasıdır. Bu tabloya izinleri ekleyebilir veya kaldırabilirsiniz. Yönetici olarak, bir API 'nin izinleri veya ayrı izinler kümesi için yönetici onayı de verebilir veya iptal edebilirsiniz.

### <a name="other-permissions-granted"></a>Diğer izinler verildi

Uygulamanız bir kiracıda kayıtlıysa, **kiracı için verilen diğer izinler**başlıklı ek bir bölüm görebilirsiniz. Bu bölümde, kiracı için uygulama nesnesi üzerinde açıkça yapılandırılmamış izinler gösterilmektedir. Bu izinler dinamik olarak istendi ve yapıldı. Bu bölüm yalnızca en az bir tane geçerli izin varsa görüntülenir.

Bu bölümde görüntülenen bir API 'nin izinleri veya ayrı izinler kümesi, **yapılandırılan izinler** bölümüne eklenebilir. Yönetici olarak, bu bölümdeki tek tek API 'Ler veya izinler için yönetici onayını de iptal edebilirsiniz.

### <a name="admin-consent-button"></a>Yönetici onay düğmesi

Uygulamanız bir kiracıda kayıtlıysa, **kiracı için yönetici onayı verme** düğmesine görürsünüz. Yönetici değilseniz veya uygulama için hiçbir izin yapılandırılmamışsa devre dışı bırakılır.
Bu düğme, yöneticinin uygulama için yapılandırılan izinlere yönetici onayı vermesini sağlar. Yönetici onay düğmesine tıkladığınızda, tüm yapılandırılmış izinleri gösteren bir onay istemiyle birlikte yeni bir pencere açılır.

> [!NOTE]
> Uygulama için yapılandırılan izinlerle izin isteminde görüntülenen izinler arasında bir gecikme vardır. İzin isteminde yapılandırılan tüm izinleri görmüyorsanız, kapatın ve yeniden başlatın.

Verilmiş ancak yapılandırılmamış izinleriniz varsa, yönetici onay düğmesi bu izinleri idare etmek isteyip istemediğinizi sorar. Bunları, yapılandırılmış izinlere ekleyebilir veya kaldırabilirsiniz.

Onay istemi, **kabul etme** veya **iptal**etme seçeneğini sağlar. Yönetici izni vermek için **kabul et** ' i seçin. **İptal**' i seçerseniz, yönetici izni verilmez. Bir hata iletisi, onay reddedildiğini belirtir.

> [!NOTE]
> Onay isteminde **kabul et** ' i seçerek yönetici onayı verme arasında bir gecikme vardır ve yönetici onayınızın durumu portalda yansıtılmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Hangi hesap türlerinin uygulamanıza erişebileceğini nasıl yapılandıracağınızı öğrenmek için serideki bir sonraki hızlı başlangıca ilerleyin. Örneğin, erişimi yalnızca kuruluşunuzdaki kullanıcılarla (tek kiracı) sınırlamak veya diğer Azure AD kiracılarındaki kullanıcılara (çok kiracılı) ve kişisel Microsoft hesaplarıyla (MSA) izin vermek isteyebilirsiniz.

> [!div class="nextstepaction"]
> [Bir uygulama tarafından desteklenen hesapları değiştirme](quickstart-modify-supported-accounts.md)