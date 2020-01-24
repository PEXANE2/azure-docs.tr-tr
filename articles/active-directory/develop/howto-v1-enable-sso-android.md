---
title: ADAL kullanarak Android 'de uygulamalar arası SSO 'yu etkinleştirme | Microsoft Docs
description: Uygulamalarınızdaki çoklu oturum açmayı etkinleştirmek için ADAL SDK 'nın özelliklerini kullanma.
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: 40710225-05ab-40a3-9aec-8b4e96b6b5e7
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: brandwe, jmprieur
ms.custom: aaddev
ms.openlocfilehash: b2dfcd1711be107fd161d38e5c9df660d35d8332
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76696937"
---
# <a name="how-to-enable-cross-app-sso-on-android-using-adal"></a>Nasıl yapılır: ADAL kullanarak Android 'de uygulamalar arası SSO 'yu etkinleştirme

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Çoklu oturum açma (SSO), kullanıcıların kimlik bilgilerini yalnızca bir kez girmelerini ve bu kimlik bilgilerinin diğer uygulamaların kullanabileceği platformlar arasında (Microsoft hesapları veya Microsoft 365 bir iş hesabı gibi) otomatik olarak çalışmasını sağlar yayımcıyı bağımsız olarak.

Microsoft 'un SDK 'larıyla birlikte kimlik platformu, kendi uygulama paketinizde veya aracı özelliği ve Authenticator uygulamalarıyla tüm cihaz genelinde SSO 'yu etkinleştirmeyi kolaylaştırır.

Bu nasıl yapılır bölümünde, müşterilerinize SSO sağlamak için uygulamanızın içinde SDK 'Yı nasıl yapılandıracağınızı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu şekilde nasıl yapılacağını bildiğiniz varsayılır:

- Azure Active Directory için eski portalı kullanarak uygulamanızı sağlayın (Azure AD). Daha fazla bilgi için bkz. [uygulamayı kaydetme](quickstart-register-app.md)
- Uygulamanızı [Azure AD Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android)tümleştirin.

## <a name="single-sign-on-concepts"></a>Çoklu oturum açma kavramları

### <a name="identity-brokers"></a>Kimlik aracıları

Microsoft, farklı satıcılardan uygulamalar arasında kimlik bilgilerinin köprülemesini ve kimlik bilgilerinin doğrulanması için tek bir güvenli yer gerektiren gelişmiş özellikler için tüm mobil platformlar için uygulamalar sağlar. Bunlar **aracılar**olarak adlandırılır.

İOS ve Android 'de aracılar, müşterilerin çalışanlara ait cihazların bazılarını veya tümünü yöneten bir şirket tarafından bağımsız olarak yüklemiş veya cihaza itilbileceği indirilebilir uygulamalar aracılığıyla sağlanır. Aracılar, güvenlik yönetimini yalnızca bazı uygulamalar için veya BT Yöneticisi yapılandırmasına bağlı olarak tüm cihaz için destekler. Bu işlevsellik, Windows 'ta, teknik olarak Web kimlik doğrulama Aracısı olarak bilinen, işletim sisteminde yerleşik bir hesap Seçicisi tarafından sağlanır.

#### <a name="broker-assisted-login"></a>Aracı yardımlı oturum açma

Aracı yardımlı oturum açmalar, Aracı uygulamasında gerçekleşen oturum açma deneyimlerinden ve kimlik platformunu uygulayan cihazdaki tüm uygulamalarda kimlik bilgilerini paylaşmak için aracının depolama ve güvenliğini kullanır. Uygulamalarınızın etkili olması, içindeki kullanıcıları imzalamak için aracıya bağımlıdır. İOS ve Android 'de, Bu aracılar müşterilerin bağımsız olarak yüklemiş olduğu indirilebilir uygulamalar aracılığıyla veya cihazı Kullanıcı için yöneten bir şirket tarafından cihaza gönderilebilecek şekilde sağlanır. Bu tür bir uygulama örneği, iOS üzerinde Microsoft Authenticator uygulamasıdır. Bu işlevsellik, Windows 'ta, teknik olarak Web kimlik doğrulama Aracısı olarak bilinen, işletim sisteminde yerleşik bir hesap Seçicisi tarafından sağlanır.
Deneyim platforma göre farklılık gösterir ve bazen doğru yönetilmiyorsa kullanıcılara karışıklığa neden olabilir. Facebook uygulamanız yüklüyse ve başka bir uygulamadan Facebook Connect kullanıyorsanız, bu düzene en çok alışkın olursunuz. Kimlik platformu aynı kalıbı kullanır.

Android 'de hesap Seçicisi, uygulamanızın üst kısmında görüntülenir ve bu, kullanıcıya daha az karışıklığa neden olur.

#### <a name="how-the-broker-gets-invoked"></a>Aracının nasıl çağrıldığında

Cihaza Microsoft Authenticator uygulaması gibi uyumlu bir aracı yüklüyse, kimlik SDK 'Ları, bir kullanıcı kimlik platformundan herhangi bir hesabı kullanarak oturum açmak istediklerinde, aracı sizin için çağırma işini otomatik olarak yapılır.

#### <a name="how-microsoft-ensures-the-application-is-valid"></a>Microsoft 'un uygulamanın geçerli olmasını sağlar

Aracı yardımlı oturum açmada sunulan güvenlik için, aracının bir uygulamanın kimliğine çağrı yapıldığından emin olunması gerekir. iOS ve Android yalnızca belirli bir uygulama için geçerli olan benzersiz tanımlayıcılar zorlamaz, bu nedenle kötü amaçlı uygulamalar yasal uygulamanın tanımlayıcısını "taklit edebilir" ve yasal uygulama için gereken belirteçleri alabilir. Microsoft 'un her zaman çalışma zamanında doğru uygulamayla iletişim kurmasını sağlamak için, geliştiricinin uygulamasını Microsoft 'a kaydederken özel bir redirectURI sağlaması istenir. **Geliştiricilerin bu yeniden yönlendirme URI 'sini oluşturması gereken ayrıntılar aşağıda ayrıntılarıyla açıklanmıştır.** Bu özel redirectURI, uygulamanın sertifika parmak izini içerir ve Google Play Store tarafından uygulama için benzersiz olacak şekilde tasarlanmıştır. Bir uygulama aracıyı çağırdığında, aracı, Android işletim sistemi tarafından Aracı adı verilen sertifika parmak izini sağlamasını ister. Aracı, kimlik sistemine yapılan çağrıda bu sertifika parmak izini Microsoft 'a sağlar. Uygulamanın sertifika parmak izi, kayıt sırasında geliştirici tarafından bize sunulan sertifika parmak iziyle eşleşmiyorsa, uygulamanın istediği kaynak için belirteç erişimi reddedilir. Bu denetim, yalnızca geliştirici tarafından kaydedilen uygulamanın belirteçleri almasını sağlar.

Aracılı-SSO oturum açmaları aşağıdaki avantajlara sahiptir:

* Kullanıcı, satıcıya bakılmaksızın tüm uygulamalarında SSO ile karşılaşır.
* Uygulamanız, koşullu erişim ve Intune senaryolarını destekleme gibi daha gelişmiş iş özellikleri kullanabilir.
* Uygulamanız, iş kullanıcıları için sertifika tabanlı kimlik doğrulamasını destekleyebilir.
* Uygulamanın kimliği ve Kullanıcı, ek güvenlik algoritmaları ve şifrelemesi ile aracı uygulaması tarafından doğrulandığından, daha güvenli bir oturum açma deneyimi.

SDK 'ların, SSO 'yu etkinleştirmek için aracı uygulamalarla nasıl çalıştığı hakkında bir temsili aşağıda verilmiştir:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
|  ADAL SDK  | |  ADAL SDK  |   |  ADAL SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+

```

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Aracı yardımlı SSO için SSO 'yu açma

Uygulamanın cihazda yüklü olan herhangi bir aracıyı kullanma yeteneği varsayılan olarak kapalıdır. Uygulamanızı aracıda kullanabilmek için bazı ek yapılandırmalar yapmanız ve uygulamanıza bazı kodlar eklemeniz gerekir.

İzlenecek adımlar şunlardır:

1. Uygulama kodunuzun MS SDK 'ya çağrılması için aracı modunu etkinleştirin
2. Yeni bir yeniden yönlendirme URI 'SI oluşturun ve hem uygulama hem de uygulama kaydınız için
3. Android bildiriminde doğru izinleri ayarlama

#### <a name="step-1-enable-broker-mode-in-your-application"></a>1\. Adım: uygulamanızda aracı modunu etkinleştirme

Uygulamanızın aracıyı kullanma özelliği, kimlik doğrulama örneğinizin "Ayarlar" veya ilk kurulumunu oluştururken açıktır. Bunu uygulamanızda yapmak için:

```
AuthenticationSettings.Instance.setUseBroker(true);
```

#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a>2\. Adım: URL şemanızın bulunduğu yeni bir yeniden yönlendirme URI 'SI oluşturma

Doğru uygulamanın, kimlik bilgisi belirteçlerini döndüren doğru uygulamanın aldığından emin olmak için, uygulamanıza Android işletim sisteminin doğrulayabilmesi için geri çağırdığınızdan emin olmanız gerekir. Android işletim sistemi, Google Play deposundaki sertifikanın karmasını kullanır. Sertifikanın bu karması, standart dışı bir uygulama tarafından taklit edilemez. Microsoft, aracı uygulamasının URI 'siyle birlikte, belirteçlerin doğru uygulamaya döndürülmesini sağlar. Uygulamaya, benzersiz bir yeniden yönlendirme URI 'SI gereklidir.

Yeniden yönlendirme URI 'niz doğru biçimde olmalıdır:

`msauth://packagename/Base64UrlencodedSignature`

örn.: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*

[Azure Portal](https://portal.azure.com/)kullanarak, bu yeniden yönlendirme URI 'sini uygulama kaydınız için kaydedebilirsiniz. Azure AD uygulama kaydı hakkında daha fazla bilgi için bkz. [Azure Active Directory tümleştirme](active-directory-how-to-integrate.md).

#### <a name="step-3-set-up-the-correct-permissions-in-your-application"></a>3\. Adım: uygulamanızda doğru izinleri ayarlama

Android 'de aracı uygulaması, uygulamalar arasında kimlik bilgilerini yönetmek için Android IŞLETIM sisteminin accounts Manager özelliğini kullanır. Android 'de aracıyı kullanabilmeniz için uygulama bildiriminizde AccountManager hesaplarını kullanma izinleri olmalıdır. Bu izinler, [burada hesap yöneticisi Için Google belgelerinde](https://developer.android.com/reference/android/accounts/AccountManager.html) ayrıntılı olarak ele alınmıştır.

Özellikle, bu izinler şunlardır:

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a>SSO 'yu yapılandırdınız!

Artık kimlik SDK 'Sı, her ikisi de uygulamalarınızın genelinde kimlik bilgilerini paylaşır ve cihazında varsa aracıyı çağırır.

## <a name="next-steps"></a>Sonraki adımlar

* [Çoklu oturum açma SAML Protokolü](single-sign-on-saml-protocol.md) hakkında bilgi edinin
