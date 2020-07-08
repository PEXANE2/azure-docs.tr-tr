---
title: ADAL kullanarak iOS 'ta uygulamalar arası SSO 'yu etkinleştirme | Microsoft Docs
description: Uygulamalarınızdaki çoklu oturum açmayı etkinleştirmek için ADAL SDK 'nın özelliklerini kullanma.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.workload: identity
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: how-to
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: brandwe
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: 08b018082c753b9524cb12a72d637fe5458d9114
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85383708"
---
# <a name="how-to-enable-cross-app-sso-on-ios-using-adal"></a>Nasıl yapılır: ADAL kullanarak iOS 'ta uygulamalar arası SSO 'yu etkinleştirme

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Çoklu oturum açma (SSO), kullanıcıların kimlik bilgilerini yalnızca bir kez girmelerini ve bu kimlik bilgilerinin diğer uygulamaların kullanabileceği platformlar arasında (Microsoft hesapları veya Microsoft 365 bir iş hesabı gibi) otomatik olarak çalışmasını sağlar.

Microsoft 'un SDK 'larıyla birlikte kimlik platformu, kendi uygulama paketinizde veya aracı özelliği ve Authenticator uygulamalarıyla tüm cihaz genelinde SSO 'yu etkinleştirmeyi kolaylaştırır.

Bu nasıl yapılır bölümünde, müşterilerinize SSO sağlamak için uygulamanızın içinde SDK 'Yı nasıl yapılandıracağınızı öğreneceksiniz.

Bu nasıl yapılır:

* Azure Active Directory (Azure Active Directory)
* Azure Active Directory B2C
* Azure Active Directory B2B
* Koşullu erişim Azure Active Directory

## <a name="prerequisites"></a>Ön koşullar

Bu şekilde nasıl yapılacağını bildiğiniz varsayılır:

* Azure AD için eski portalı kullanarak uygulamanızı sağlayın. Daha fazla bilgi için bkz. [uygulamayı kaydetme](../develop/quickstart-register-app.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)
* Uygulamanızı [Azure AD IOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc)ile tümleştirin.

## <a name="single-sign-on-concepts"></a>Çoklu oturum açma kavramları

### <a name="identity-brokers"></a>Kimlik aracıları

Microsoft, farklı satıcılardan uygulamalar arasında kimlik bilgilerinin köprülemesini ve kimlik bilgilerinin doğrulanması için tek bir güvenli yer gerektiren gelişmiş özellikler için tüm mobil platformlar için uygulamalar sağlar. Bunlar **aracılar**olarak adlandırılır.

İOS ve Android 'de aracılar, müşterilerin çalışanlara ait cihazların bazılarını veya tümünü yöneten bir şirket tarafından bağımsız olarak yüklemiş veya cihaza itilbileceği indirilebilir uygulamalar aracılığıyla sağlanır. Aracılar, güvenlik yönetimini yalnızca bazı uygulamalar için veya BT Yöneticisi yapılandırmasına bağlı olarak tüm cihaz için destekler. Bu işlevsellik, Windows 'ta, teknik olarak Web kimlik doğrulama Aracısı olarak bilinen, işletim sisteminde yerleşik bir hesap Seçicisi tarafından sağlanır.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>Mobil cihazlarda oturum açmak için desenler

Cihazlardaki kimlik bilgilerine erişim iki temel deseni izler:

* Aracı olmayan yardımlı oturum açma işlemleri
* Aracı yardımlı oturum açma işlemleri

#### <a name="non-broker-assisted-logins"></a>Aracı olmayan yardımlı oturum açma işlemleri

Aracı olmayan yardımlı oturum açmalar, uygulamayla satır içi olarak gerçekleşen ve bu uygulama için cihazdaki yerel depolamayı kullanan oturum açma deneyimidir. Bu depolama, uygulamalar arasında paylaşılabilir, ancak kimlik bilgileri bu kimlik bilgisini kullanan uygulama veya uygulama paketine sıkı bir şekilde bağlanır. Uygulamanın kendisi içinde bir Kullanıcı adı ve parola girdiğinizde birçok mobil uygulamada bu sorunla karşılaştık.

Bu oturumlar aşağıdaki avantajlara sahiptir:

* Kullanıcı deneyimi tamamen uygulamanın içinde bulunur.
* Kimlik bilgileri, uygulama paketinize tek bir oturum açma deneyimi sunan aynı sertifika tarafından imzalanmış uygulamalar arasında paylaşılabilir.
* Oturum açma deneyiminin etrafında denetim, oturum açmadan önce ve sonra uygulamaya sunulur.

Bu oturum açmalar aşağıdaki dezavantajları vardır:

* Kullanıcılar, Microsoft kimliği kullanan tüm uygulamalarda tek oturum açma deneyiminden yalnızca uygulamanızın yapılandırıldığı Microsoft kimliklerine sahip olamaz.
* Uygulamanız koşullu erişim gibi daha gelişmiş iş özellikleriyle kullanılamaz veya Intune ürün paketini kullanın.
* Uygulamanız iş kullanıcıları için sertifika tabanlı kimlik doğrulamasını destekleyemez.

SDK 'ların, SSO 'yu etkinleştirmek için uygulamalarınızın paylaşılan depolamayla nasıl çalıştığı hakkında bir temsili aşağıda verilmiştir:

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| ADAL SDK  |  |  ADAL SDK  |  |  ADAL SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a>Aracı yardımlı oturum açma işlemleri

Aracı yardımlı oturum açmalar, Aracı uygulamasında gerçekleşen oturum açma deneyimlerinden ve kimlik platformunu uygulayan cihazdaki tüm uygulamalarda kimlik bilgilerini paylaşmak için aracının depolama ve güvenliğini kullanır. Bu, uygulamalarınızın içindeki kullanıcıları imzalamak için aracıya bağlı olduğu anlamına gelir. İOS ve Android 'de, Bu aracılar, müşterilerin cihazı bağımsız olarak yükleyen veya cihaza Kullanıcı tarafından yönetilen bir şirket tarafından gönderilen indirilebilir uygulamalar aracılığıyla sağlanır. Bu tür bir uygulama örneği, iOS üzerinde Microsoft Authenticator uygulamasıdır. Windows 'da Bu işlevsellik, Web kimlik doğrulama Aracısı olarak teknik olarak bilinen, işletim sisteminde yerleşik olarak bulunan bir hesap Seçicisi tarafından sağlanır.

Deneyim platforma göre farklılık gösterir ve bazen doğru yönetilmiyorsa kullanıcılara karışıklığa neden olabilir. Facebook uygulamanız yüklüyse ve başka bir uygulamadan Facebook Connect kullanıyorsanız, bu düzene en çok alışkın olursunuz. Kimlik platformu aynı kalıbı kullanır.

İOS için bu, uygulamanızın arka plana gönderildiği bir "geçiş" animasyonuna yol açar, çünkü Microsoft Authenticator uygulamalar kullanıcının oturum açmak istedikleri hesabı seçmesini sağlamak için bir ön plana gelir.

Android ve Windows için hesap Seçicisi, uygulamanızın üst kısmında görüntülenir ve bu, kullanıcıya daha az karışıklığa neden olur.

#### <a name="how-the-broker-gets-invoked"></a>Aracının nasıl çağrıldığında

Cihaza Microsoft Authenticator uygulaması gibi uyumlu bir aracı yüklüyse, Kullanıcı kimlik platformundan herhangi bir hesabı kullanarak oturum açmasını gösterdiğinde SDK 'lar sizin için otomatik olarak aracı çağırma işini yapılır. Bu hesap, bir kişisel Microsoft hesabı, bir iş veya okul hesabı ya da Azure 'da B2C ve B2B ürünlerimizi kullanarak barındırmanıza ve barındırmanıza izin verdiğiniz bir hesaptır.

#### <a name="how-we-ensure-the-application-is-valid"></a>Uygulamanın geçerli olduğundan emin olduğumuz

Aracıyı çağıran bir uygulamanın kimliğinin, aracı yardımlı oturum açma işlemleri için sağlamamız gereken güvenlik açısından önemli olmasına dikkat edin. Ne iOS ne de Android, yalnızca belirli bir uygulama için geçerli olan benzersiz tanımlayıcılar uygular, bu nedenle kötü amaçlı uygulamalar yasal uygulamanın tanımlayıcısını "taklit edebilir" ve yasal uygulama için gereken belirteçleri alabilir. Çalışma zamanında doğru uygulamayla her zaman iletişim kurduğumuz için, geliştiriciden Microsoft ile uygulama kaydederken özel bir redirectURI sağlamasını isteyeceğiz. Geliştiricilerin bu yeniden yönlendirme URI 'sini oluşturması gereken ayrıntılar aşağıda ayrıntılarıyla açıklanmıştır. Bu özel redirectURI, uygulamanın paket KIMLIĞINI içerir ve Apple App Store tarafından uygulamaya benzersiz olacak şekilde tasarlanmıştır. Bir uygulama aracıya çağrı yaparken, Aracı iOS işletim sistemini aracıyı çağıran paket KIMLIĞI ile sağlamasını ister. Aracı kimlik sistemimize yapılan çağrıda Bu paket KIMLIĞINI Microsoft 'a sağlar. Uygulamanın paket KIMLIĞI, kayıt sırasında geliştirici tarafından bize girilen paket KIMLIĞIYLE eşleşmiyorsa, uygulamanın istediği kaynak için belirteçlere erişimi reddedecağız. Bu denetim, yalnızca geliştirici tarafından kaydedilen uygulamanın belirteçleri almasını sağlar.

**Geliştirici, SDK 'nın aracıyı çağırırsa veya aracı olmayan yardımlı akışı kullanıp kullanmadığını tercih etti.** Ancak geliştirici, aracı yardımlı akışı kullanmayı seçerse, kullanıcının cihaza zaten eklemiş olabileceği ve uygulamaların iş özellikleriyle kullanılmasını önlediği, Microsoft 'un, koşullu erişim, Intune yönetim özellikleri ve sertifika tabanlı kimlik doğrulama gibi müşterileri sağladığı avantajlardan faydalanır.

Bu oturumlar aşağıdaki avantajlara sahiptir:

* Kullanıcı, satıcıya bakılmaksızın tüm uygulamalarında SSO ile karşılaşır.
* Uygulamanız, koşullu erişim gibi daha gelişmiş iş özellikleri kullanabilir veya Intune ürün paketini kullanabilir.
* Uygulamanız, iş kullanıcıları için sertifika tabanlı kimlik doğrulamasını destekleyebilir.
* Uygulamanın kimliği ve Kullanıcı, ek güvenlik algoritmaları ve şifrelemesi ile aracı uygulama tarafından doğrulandığından, daha güvenli oturum açma deneyimi.

Bu oturum açmalar aşağıdaki dezavantajları vardır:

* İOS 'ta, kimlik bilgileri seçildiği sırada kullanıcı uygulamanızın deneyiminden geçirilir.
* Uygulamanızdaki müşterilerinizin oturum açma deneyimini yönetme yeteneğinin kaybı.

SDK 'ların, SSO 'yu etkinleştirmek için aracı uygulamalarla nasıl çalıştığı hakkında bir temsili aşağıda verilmiştir:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
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

## <a name="enabling-cross-app-sso-using-adal"></a>ADAL kullanarak uygulamalar arası SSO 'yu etkinleştirme

Burada, ADAL iOS SDK 'sını kullanarak şunları yapın:

* Uygulama paketiniz için aracı olmayan yardımlı SSO 'yu açın
* Aracı yardımlı SSO desteğini aç

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Aracı olmayan yardımlı SSO için SSO 'yu açma

Uygulamalar arasında aracı olmayan yardımlı SSO için SDK 'lar sizin için SSO 'nun karmaşıklığını çoğunu yönetir. Bu, önbellekte doğru kullanıcıyı bulmayı ve sorgulamanızı sağlamak için oturum açmış kullanıcıların bir listesini sürdürmeyi içerir.

Sahip olduğunuz uygulamalar genelinde SSO 'yu etkinleştirmek için aşağıdakileri yapmanız gerekir:

1. Tüm uygulamalarınızın aynı Istemci KIMLIĞINI veya uygulama KIMLIĞINI kullandığından emin olun.
2. Anahtar zincirlerini paylaşabilmek için tüm uygulamalarınızın Apple 'dan aynı imzalama sertifikasını paylaştığından emin olun.
3. Uygulamalarınızın her biri için aynı Anahtarlık yetkilendirmesini isteyin.
4. Kullanmamızı istediğiniz paylaşılan anahtarlığa ilişkin SDK 'lara anlatın.

#### <a name="using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Uygulama paketinizdeki tüm uygulamalar için aynı Istemci KIMLIĞINI/uygulama KIMLIĞINI kullanma

Kimlik platformunun, uygulamalarınızda belirteçleri paylaşmasına izin verildiğini bilmesi için, uygulamalarınızın her birinin aynı Istemci KIMLIĞINI veya uygulama KIMLIĞINI paylaşması gerekir. Bu, portalda ilk uygulamanızı kaydettiğinizde size sağlanmış olan benzersiz tanıtıcıdır.

Yeniden yönlendirme URI 'Leri, aynı uygulama KIMLIĞINI kullanıyorsa Microsoft Identity Service 'e farklı uygulamalar tanımlamanızı sağlar. Her uygulamanın, ekleme portalında kayıtlı birden çok yeniden yönlendirme URI 'si olabilir. Paketinizdeki her uygulamanın farklı bir yeniden yönlendirme URI 'SI olacaktır. Bunun nasıl görüneceğine ilişkin bir örnek:

APP1 yeniden yönlendirme URI 'SI:`x-msauth-mytestiosapp://com.myapp.mytestapp`

App2 yeniden yönlendirme URI 'SI:`x-msauth-mytestiosapp://com.myapp.mytestapp2`

App3 yeniden yönlendirme URI 'SI:`x-msauth-mytestiosapp://com.myapp.mytestapp3`

....

Bunlar, aynı istemci KIMLIĞI/uygulama KIMLIĞI altında iç içe yerleştirilmiş ve SDK yapılandırmanızda bize geri döndürülen yeniden yönlendirme URI 'sine bağlı olarak aranır.

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```

Bu yeniden yönlendirme URI 'Lerinin biçimi aşağıda açıklanmıştır. Aracıyı desteklemek istemediğiniz müddetçe herhangi bir yeniden yönlendirme URI 'SI kullanabilirsiniz; Bu durumda yukarıdaki gibi görünmelidir *

#### <a name="create-keychain-sharing-between-applications"></a>Uygulamalar arasında Anahtarlık paylaşımı oluşturma

Anahtarlık paylaşımının etkinleştirilmesi, bu belgenin kapsamı dışındadır ve bu sayede, belge [özellikleri ekleme özelliği](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html)bulunur. Önemli olan özellikler, anahtarlarınızın hangi amaçla çağrdığına karar verirsiniz ve bu özelliği tüm uygulamalarınızda eklemektir.

Yetkilendirmeler doğru şekilde ayarlandığında, proje dizininizde `entitlements.plist` aşağıdaki gibi görünen bir öğe içeren bir dosya görmeniz gerekir:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "https://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>keychain-access-groups</key>
    <array>
        <string>$(AppIdentifierPrefix)com.myapp.mytestapp</string>
        <string>$(AppIdentifierPrefix)com.myapp.mycache</string>
    </array>
</dict>
</plist>
```

Her uygulamanızda Anahtarlık yetkilendirme yetkilerini etkinleştirdikten sonra ve SSO kullanmaya hazırsanız, aşağıdaki ayarı kullanarak aşağıdaki ayarı kullanarak kimlik anahtarlığınıza ilişkin kimlik SDK 'sını söyleyin `ADAuthenticationSettings` :

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [!WARNING]
> Uygulamalarınız genelinde bir anahtarlık paylaştığınızda, herhangi bir uygulama kullanıcıları silebilir veya uygulamanızdaki tüm belirteçleri daha kötü bir şekilde silebilir. Arka plan işlerini yapmak için belirteçlere güvenen uygulamalarınız varsa, bu özellikle felaket. Anahtarlık paylaşımı, kimlik SDK 'Ları aracılığıyla herhangi bir ve tüm kaldırma işlemlerine çok dikkatli olmanız gerektiği anlamına gelir.

İşte bu kadar! SDK artık tüm uygulamalarınızda kimlik bilgilerini paylaşacaktır. Kullanıcı listesi ayrıca uygulama örnekleri arasında paylaşılır.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Aracı yardımlı SSO için SSO 'yu açma

Uygulamanın cihazda yüklü olan herhangi bir aracıyı kullanma yeteneği **Varsayılan olarak**kapalıdır. Uygulamanızı aracıda kullanabilmek için bazı ek yapılandırmalar yapmanız ve uygulamanıza bazı kodlar eklemeniz gerekir.

İzlenecek adımlar şunlardır:

1. Uygulama kodunuzun MS SDK çağrısında aracı modunu etkinleştirin.
2. Yeni bir yeniden yönlendirme URI 'si oluşturun ve hem uygulama hem de uygulama kaydınız için bu URI 'yi sağlayın.
3. URL şeması kaydediliyor.
4. İnfo. plist dosyanıza bir izin ekleyin.

#### <a name="step-1-enable-broker-mode-in-your-application"></a>1. Adım: uygulamanızda aracı modunu etkinleştirme

Uygulamanızın aracıyı kullanma yeteneği, kimlik doğrulama nesnenizin "bağlam" veya ilk kurulumunu oluştururken açıktır. Bunu, kodunuzda kimlik bilgileri türünü ayarlayarak yapabilirsiniz:

```
/*! See the ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
`AD_CREDENTIALS_AUTO`Ayar SDK 'nın aracıya çağrı yapmasına izin verir, `AD_CREDENTIALS_EMBEDDED` SDK 'nın aracıya çağrılmasını engeller.

#### <a name="step-2-registering-a-url-scheme"></a>2. Adım: URL şeması kaydetme

Kimlik platformu, aracıyı çağırmak için URL 'Leri kullanır ve sonra denetimi uygulamanıza geri döndürür. Bu gidiş dönüş için, kimlik platformunun hakkında bilgi sahibi olacağı uygulamanız için kayıtlı bir URL şemasına ihtiyacınız vardır. Bu, daha önce uygulamanıza kaydetmiş olabilecek diğer uygulama şemalarına ek olarak olabilir.

> [!WARNING]
> Aynı URL düzenini kullanan başka bir uygulamanın olasılığını en aza indirmek için, URL düzenini oldukça benzersiz hale getirmeyi öneririz. Apple, App Store 'da kayıtlı URL düzenlerinin benzersizlik düzeyini zorlamaz.

Bunun proje yapılandırmanızda nasıl göründüğünü gösteren bir örnek aşağıda verilmiştir. Bunu ayrıca XCode 'da da yapabilirsiniz:

```
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.myapp.mytestapp</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>x-msauth-mytestiosapp</string>
        </array>
    </dict>
</array>
```

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a>3. Adım: URL şemanızın bulunduğu yeni bir yeniden yönlendirme URI 'SI oluşturma

Her zaman kimlik bilgisi belirteçlerini doğru uygulamaya döndürtiğimiz için, iOS işletim sisteminin doğrulayabilmesi için uygulamanıza geri dönediğimiz bir şekilde emin olunması gerekir. İOS işletim sistemi, Microsoft Broker uygulamalarına bu uygulamayı çağıran uygulamanın paket KIMLIĞINI bildirir. Bu, standart dışı bir uygulama tarafından taklit edilemez. Bu nedenle, belirteçlerin doğru uygulamaya döndürüldüğünden emin olmak için bu, aracı uygulamamız URI 'siyle birlikte faydalanır. Bu benzersiz yeniden yönlendirme URI 'sini uygulamanızda her ikisi de oluşturmanız ve geliştirici portalımızda yeniden yönlendirme URI 'SI olarak ayarlamanız gerekir.

Yeniden yönlendirme URI 'niz doğru biçimde olmalıdır:

`<app-scheme>://<your.bundle.id>`

Ex: *x-msauth-mytesosapp://com.MyApp.mytestapp*

Bu yeniden yönlendirme URI 'sinin [Azure Portal](https://portal.azure.com/)kullanarak uygulama kaydlarınızın belirtilmesi gerekir. Azure AD uygulama kaydı hakkında daha fazla bilgi için bkz. [Azure Active Directory tümleştirme](../develop/active-directory-how-to-integrate.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json).

##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-to-support-certificate-based-authentication"></a>Adım 3a: sertifika tabanlı kimlik doğrulamasını desteklemek için uygulamanıza ve dev portalına yeniden yönlendirme URI 'SI ekleme

Sertifika tabanlı kimlik doğrulamasını desteklemek için ikinci bir "msauth" uygulamanız gerekir ve bu desteği uygulamanızda eklemek istiyorsanız sertifika kimlik doğrulamasını işlemek için [Azure Portal](https://portal.azure.com/) .

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

örn.: *msauth://Code/x-msauth-mytestiosapp%3A%2F%2Fcom.MyApp.mytestapp*

#### <a name="step-4-add-a-configuration-parameter-to-your-app"></a>4. Adım: uygulamanıza bir yapılandırma parametresi ekleme

ADAL, aracının cihaza yüklenip yüklenmediğini denetlemek için – canOpenURL: kullanır. Üzerinde iOS 9 ' da, Apple bir uygulamanın sorgulayabilecekleri düzenleri kilitlemiş. ' Nin Lsapplicationqueriesdüzenleri bölümüne "msauth" eklemeniz gerekir `info.plist file` .

```
    <key>LSApplicationQueriesSchemes</key>
    <array>
        <string>msauth</string>
    </array>

```

### <a name="youve-configured-sso"></a>SSO 'yu yapılandırdınız!

Artık kimlik SDK 'Sı, her ikisi de uygulamalarınızın genelinde kimlik bilgilerini paylaşır ve cihazında varsa aracıyı çağırır.

## <a name="next-steps"></a>Sonraki adımlar

* [Çoklu oturum açma SAML Protokolü](../develop/single-sign-on-saml-protocol.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) hakkında bilgi edinin
