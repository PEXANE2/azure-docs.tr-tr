---
title: ADAL kullanarak Android'de çapraz uygulama SSO nasıl etkinleştirilir | Microsoft Dokümanlar
description: Uygulamalarınızda tek oturum açmayı etkinleştirmek için ADAL SDK'nın özelliklerini nasıl kullanacağınız.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.workload: identity
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: brandwe, jmprieur
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: 0b87a9cd0ae29281faad4209f4449d547921835d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154823"
---
# <a name="how-to-enable-cross-app-sso-on-android-using-adal"></a>Nasıl olur: ADAL kullanarak Android'de çapraz uygulama SSO'su etkinleştirin

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Tek oturum açma (SSO), kullanıcıların kimlik bilgilerini yalnızca bir kez girmelerine ve bu kimlik bilgilerinin uygulamalar da ve diğer uygulamaların kullanabileceği platformlar da (Microsoft Hesapları veya Microsoft 365'teki bir iş hesabı gibi) otomatik olarak çalışmasını sağlar. yayıncı önemli.

Microsoft'un kimlik platformu, SDK'larla birlikte, SSO'yu kendi uygulama paketinizde veya aracı özelliği ve Kimlik Doğrulayıcı uygulamalarıyla tüm cihazda etkinleştirmenizi kolaylaştırır.

Bu nasıl yapılandırılırsa, müşterilerinize SSO sağlamak için uygulamanız dahilinde SDK'yı nasıl yapılandırabileceğinizi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılacağını bildiğinizi varsayar:

- Azure Active Directory (Azure AD) için eski portalı kullanarak uygulamanızı sağlama. Daha fazla bilgi için [bkz.](../develop/quickstart-register-app.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)
- Uygulamanızı Azure [AD Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android)ile tümleştirin.

## <a name="single-sign-on-concepts"></a>Tek oturum açma kavramları

### <a name="identity-brokers"></a>Kimlik simsarları

Microsoft, farklı satıcılardan gelen uygulamalar arasında kimlik bilgilerinin birleştirilmesine olanak tanıyan her mobil platform ve kimlik bilgilerini doğrulamak için tek bir güvenli yer gerektiren gelişmiş özellikler için uygulamalar sağlar. Bunlara **broker**denir.

iOS ve Android'de, brokerlar, müşterilerin bağımsız olarak yükleyebileceği veya çalışanları için cihazların bir kısmını veya tamamını yöneten bir şirket tarafından cihaza itildiği indirilebilir uygulamalar aracılığıyla sağlanır. Aracılar, bt yöneticisi yapılandırmasını temel alan bazı uygulamalar veya aygıtın tamamı için güvenliği yönetmeyi destekler. Windows'da bu işlevsellik, teknik olarak Web Kimlik Doğrulama Aracısı olarak bilinen işletim sistemine yerleşik bir hesap seçici tarafından sağlanır.

#### <a name="broker-assisted-login"></a>Broker destekli giriş

Broker destekli oturum açmalar, broker uygulamasında meydana gelen ve aracının depolama ve güvenliğini kullanarak kimlik platformını uygulayan cihazdaki tüm uygulamalarda kimlik bilgilerini paylaşmak için kullanılan oturum açma deneyimleridir. Uygulamalarınızın anlamı, kullanıcıları oturum açacak aracıya güvenecektir. iOS ve Android'de bu brokerlar, müşterilerin bağımsız olarak yükleyebileceği veya cihazı kullanıcıiçin yöneten bir şirket tarafından cihaza itilebildiği indirilebilir uygulamalar aracılığıyla sağlanır. Bu tür uygulamalara örnek olarak iOS'taki Microsoft Authenticator uygulaması örnektir. Windows'da bu işlevsellik, teknik olarak Web Kimlik Doğrulama Aracısı olarak bilinen işletim sistemine yerleşik bir hesap seçici tarafından sağlanır.
Deneyim platforma göre değişir ve doğru yönetilmezse bazen kullanıcılar için rahatsız edici olabilir. Facebook uygulamasını yüklediyseniz ve başka bir uygulamadan Facebook Connect kullanıyorsanız, muhtemelen bu modele aşinasınızdır. Kimlik platformu aynı deseni kullanır.

Android'de, hesap seçici uygulamanızın üstünde görüntülenir ve bu da kullanıcı için daha az rahatsız edicidir.

#### <a name="how-the-broker-gets-invoked"></a>Komisyoncu nasıl çağrılır

Microsoft Authenticator uygulaması gibi aygıta uyumlu bir broker yüklenirse, bir kullanıcı kimlik platformundan herhangi bir hesabı kullanarak oturum açmak istediğini belirttiğinde kimlik SDK'ları aracıyı sizin için çağırmak işini otomatik olarak yapar.

#### <a name="how-microsoft-ensures-the-application-is-valid"></a>Microsoft uygulamanın geçerli olmasını nasıl sağlar?

Broker i arayan bir uygulamanın kimliğinin sağlanması ihtiyacı, broker destekli oturum açmalarda sağlanan güvenlik için çok önemlidir. iOS ve Android yalnızca belirli bir uygulama için geçerli olan benzersiz tanımlayıcıları zorlamaz, bu nedenle kötü amaçlı uygulamalar yasal bir uygulamanın tanımlayıcısını "taklit edebilir" ve yasal uygulama için verilen belirteçleri alabilir. Microsoft'un çalışma zamanında her zaman doğru uygulamayla iletişim kurmasını sağlamak için, geliştiriciden uygulamalarını Microsoft'a kaydederken özel bir redirectURI sağlaması istenir. **Geliştiriciler bu yönlendirme URI zanaat nasıl ayrıntılı olarak aşağıda ele alınmıştır.** Bu özel redirectURI, uygulamanın sertifika parmak izini içerir ve Google Play Store tarafından uygulamaya özgü olması sağlanır. Bir uygulama broker'ı aradığında, broker Android işletim sisteminden broker'ı çağıran sertifika parmak izini sağlamasını ister. Aracı, kimlik sistemine yapılan çağrıda bu sertifikaparmak izini Microsoft'a sağlar. Başvurunun sertifika parmak izi, kayıt sırasında geliştirici tarafından bize verilen sertifika parmak izi ile eşleşmiyorsa, uygulamanın talep ettiği kaynak için belirteçlere erişim reddedilir. Bu denetim, yalnızca geliştirici tarafından kaydedilmiş uygulamanın belirteçleri almasını sağlar.

Aracılı-SSO girişleri aşağıdaki avantajlara sahiptir:

* Kullanıcı, satıcı ne olursa olsun tüm uygulamalarında SSO'yu deneyimler.
* Uygulamanız Koşullu Erişim gibi daha gelişmiş iş özelliklerini kullanabilir ve Intune senaryolarını destekleyebilir.
* Uygulamanız, iş kullanıcıları için sertifika tabanlı kimlik doğrulamasını destekleyebilir.
* Uygulamanın ve kullanıcının kimliği olarak daha güvenli oturum açma deneyimi, ek güvenlik algoritmaları ve şifreleme ile aracı uygulaması tarafından doğrulanır.

SSO'yu etkinleştirmek için SDK'ların broker uygulamalarıyla nasıl çalıştığını aşağıda açıklayabilirsiniz:

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

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Broker destekli SSO için SSO'ya açma

Bir uygulamanın aygıta yüklenen herhangi bir aracıyı kullanabilmesi varsayılan olarak kapatılır. Aracıile uygulamanızı kullanmak için, bazı ek yapılandırma yapmak ve uygulamanıza bazı kod eklemek gerekir.

İzlenen adımlar şunlardır:

1. Uygulama kodunuzdaki MS SDK'ya yapılan çağrıda broker modunu etkinleştirme
2. Uri'yi yeni bir yönlendirme kurun ve bunu hem uygulamanıza hem de uygulama kaydınıza sağlayın
3. Android bildiriminde doğru izinleri ayarlama

#### <a name="step-1-enable-broker-mode-in-your-application"></a>Adım 1: Uygulamanızda broker modunu etkinleştirin

Kimlik Doğrulama örneğinizin "ayarları" veya ilk kurulumunu oluşturduğunuzda uygulamanızın aracıyı kullanma özelliği açıktır. Bunu uygulamanızda yapmak için:

```
AuthenticationSettings.Instance.setUseBroker(true);
```

#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a>Adım 2: URL Şemanızla yeni bir yeniden yönlendirme URI kurun

Doğru uygulamanın iade edilen kimlik bilgilerini aldığından emin olmak için, Android işletim sisteminin doğrulayabildiği bir şekilde başvurunuza geri çağrının yapılmasını sağlamanız gerekir. Android işletim sistemi, Google Play mağazasında sertifikanın karma sını kullanır. Sertifikanın bu karma bir haydut uygulama tarafından taklit edilemez. Microsoft, aracı uygulamanın URI'si ile birlikte belirteçlerin doğru uygulamaya döndürülmesini sağlar. Benzersiz bir yönlendirme URI uygulama üzerinde kayıtlı olması gereklidir.

Yeniden yönlendirme URI uygun biçimde olmalıdır:

`msauth://packagename/Base64UrlencodedSignature`

ör: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*

Bu yeniden yönlendirme URI'yi [Azure portalını](https://portal.azure.com/)kullanarak uygulama kaydınızda kaydedebilirsiniz. Azure AD uygulama kaydı hakkında daha fazla bilgi için azure [etkin dizinle tümleştirme](../develop/active-directory-how-to-integrate.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)'ye bakın.

#### <a name="step-3-set-up-the-correct-permissions-in-your-application"></a>Adım 3: Uygulamanızda doğru izinleri ayarlama

Android'deki broker uygulaması, uygulamalar arasında kimlik bilgilerini yönetmek için Android işletim sistemi'nin Accounts Manager özelliğini kullanır. Android'deki aracıyı kullanabilmek için uygulama manifestonuzun AccountManager hesaplarını kullanma izinlerine sahip olması gerekir. Bu izinler, Hesap Yöneticisi [için Google belgelerinde](https://developer.android.com/reference/android/accounts/AccountManager.html) ayrıntılı olarak tartışılmıştır

Özellikle, bu izinler şunlardır:

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a>SSO'ya göre yapılandırıldınız!

Artık kimlik SDK, hem uygulamalarınızda kimlik bilgilerini otomatik olarak paylaşır hem de aygıtlarında varsa aracıyı çağırır.

## <a name="next-steps"></a>Sonraki adımlar

* Tek [oturum açma SAML protokolü](../develop/single-sign-on-saml-protocol.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) hakkında bilgi edinin
