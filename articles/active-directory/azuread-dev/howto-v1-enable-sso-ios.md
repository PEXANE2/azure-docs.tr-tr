---
title: ADAL kullanarak iOS'ta çapraz uygulama SSO nasıl etkinleştirilir | Microsoft Dokümanlar
description: Uygulamalarınızda Tek Oturum Açma özelliğini etkinleştirmek için ADAL SDK'nın özelliklerini nasıl kullanabilirsiniz?
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.workload: identity
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: brandwe
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: 082cbb931c9dae60b39f9ee5323337bf051fb56d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154789"
---
# <a name="how-to-enable-cross-app-sso-on-ios-using-adal"></a>Nasıl? ADAL kullanarak iOS'ta çapraz uygulama SSO'su etkinleştirme

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Tek oturum açma (SSO), kullanıcıların kimlik bilgilerini yalnızca bir kez girmelerine ve bu kimlik bilgilerinin uygulamalar da ve diğer uygulamaların kullanabileceği platformlar da (Microsoft Hesapları veya Microsoft 365'teki bir iş hesabı gibi) otomatik olarak çalışmasını sağlar. yayıncı önemli.

Microsoft'un kimlik platformu, SDK'larla birlikte, SSO'yu kendi uygulama paketinizde veya aracı özelliği ve Kimlik Doğrulayıcı uygulamalarıyla tüm cihazda etkinleştirmenizi kolaylaştırır.

Bu nasıl yapılandırılırsa, müşterilerinize SSO sağlamak için uygulamanız dahilinde SDK'yı nasıl yapılandırabileceğinizi öğreneceksiniz.

Bu nasıl uygulanır:

* Azure Etkin Dizini (Azure Etkin Dizin)
* Azure Active Directory B2C
* Azure Etkin Dizin B2B
* Azure Etkin Dizin Koşullu Erişim

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılacağını bildiğinizi varsayar:

* Azure AD için eski portalı kullanarak uygulamanızı sağlama. Daha fazla bilgi için [bkz.](../develop/quickstart-register-app.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)
* Uygulamanızı Azure [AD iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc)ile tümleştirin.

## <a name="single-sign-on-concepts"></a>Tek oturum açma kavramları

### <a name="identity-brokers"></a>Kimlik simsarları

Microsoft, farklı satıcılardan gelen uygulamalar arasında kimlik bilgilerinin birleştirilmesine olanak tanıyan her mobil platform ve kimlik bilgilerini doğrulamak için tek bir güvenli yer gerektiren gelişmiş özellikler için uygulamalar sağlar. Bunlara **broker**denir.

iOS ve Android'de, brokerlar, müşterilerin bağımsız olarak yükleyebileceği veya çalışanları için cihazların bir kısmını veya tamamını yöneten bir şirket tarafından cihaza itildiği indirilebilir uygulamalar aracılığıyla sağlanır. Aracılar, bt yöneticisi yapılandırmasını temel alan bazı uygulamalar veya aygıtın tamamı için güvenliği yönetmeyi destekler. Windows'da bu işlevsellik, teknik olarak Web Kimlik Doğrulama Aracısı olarak bilinen işletim sistemine yerleşik bir hesap seçici tarafından sağlanır.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>Mobil cihazlarda oturum açma desenleri

Aygıtlarda kimlik bilgilerine erişim iki temel desen izler:

* Aracı olmayan destekli girişler
* Broker destekli girişler

#### <a name="non-broker-assisted-logins"></a>Aracı olmayan destekli girişler

Aracı olmayan destekli oturum açmalar, uygulamayla birlikte olur ve bu uygulama için aygıttaki yerel depolamayı kullanır. Bu depolama, uygulamalar arasında paylaşılabilir, ancak kimlik bilgileri bu kimlik bilgilerini kullanarak uygulamaya veya uygulama paketine sıkı sıkıya bağlıdır. Uygulamanın kendi içinde bir kullanıcı adı ve parola girdiğinizde büyük olasılıkla birçok mobil uygulamada bu deneyim yaşadım.

Bu oturum açmaların aşağıdaki avantajları vardır:

* Kullanıcı deneyimi tamamen uygulama içinde bulunmaktadır.
* Kimlik bilgileri, aynı sertifika tarafından imzalanan uygulamalar arasında paylaşılarak, uygulama paketinize tek bir oturum açma deneyimi sağlar.
* Oturum açma deneyimi nin kontrolü oturum açmadan önce ve sonra uygulamaya sağlanır.

Bu girişlerin aşağıdaki dezavantajları vardır:

* Kullanıcılar, Microsoft kimliği kullanan tüm uygulamalarda, yalnızca uygulamanızın yapılandırdığı Microsoft kimlikleri arasında tek oturum açamazsınız.
* Uygulamanız Koşullu Erişim gibi daha gelişmiş iş özellikleriyle kullanılamaz veya Intune ürün paketini kullanamaz.
* Uygulamanız, iş kullanıcıları için sertifika tabanlı kimlik doğrulamasını destekleemez.

SSO'yu etkinleştirmek için SDK'ların uygulamalarınızın paylaşılan depolama alanıyla nasıl çalıştığının bir gösterimi aşağıda veda edebilirsiniz:

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

#### <a name="broker-assisted-logins"></a>Broker destekli girişler

Broker destekli oturum açmalar, broker uygulamasında meydana gelen ve aracının depolama ve güvenliğini kullanarak kimlik platformını uygulayan cihazdaki tüm uygulamalarda kimlik bilgilerini paylaşmak için kullanılan oturum açma deneyimleridir. Bu, uygulamalarınızın kullanıcıları oturum açmanız için aracıya güvendiği anlamına gelir. iOS ve Android'de bu brokerlar, müşterilerin bağımsız olarak yükleyebileceği veya cihazı kullanıcıiçin yöneten bir şirket tarafından cihaza itildiği indirilebilir uygulamalar aracılığıyla sağlanır. Bu tür uygulamalara örnek olarak iOS'taki Microsoft Authenticator uygulaması örnektir. Windows'da bu işlevsellik, teknik olarak Web Kimlik Doğrulama Aracısı olarak bilinen işletim sistemine yerleşik bir hesap seçici tarafından sağlanır.

Deneyim platforma göre değişir ve doğru yönetilmezse bazen kullanıcılar için rahatsız edici olabilir. Facebook uygulamasını yüklediyseniz ve başka bir uygulamadan Facebook Connect kullanıyorsanız, muhtemelen bu modele aşinasınızdır. Kimlik platformu aynı deseni kullanır.

iOS için bu, Microsoft Authenticator uygulamaları kullanıcının oturum açacak hangi hesapla oturum açacağını seçmesi için ön plana çıkarken uygulamanızın arka plana gönderildiği bir "geçiş" animasyonuna yol açar.

Android ve Windows için hesap seçici uygulamanızın üstünde görüntülenir ve bu da kullanıcı için daha az rahatsız edicidir.

#### <a name="how-the-broker-gets-invoked"></a>Komisyoncu nasıl çağrılır

Microsoft Authenticator uygulaması gibi aygıta uyumlu bir broker yüklenirse, kullanıcı kimlik platformundan herhangi bir hesabı kullanarak oturum açmak istediğini belirttiğinde SDK'lar aracıyı sizin için çağırmak işini otomatik olarak yapar. Bu hesap, B2C ve B2B ürünlerimizi kullanarak azure'da sağladığınız ve barındırdığınız kişisel bir Microsoft Hesabı, bir iş veya okul hesabı veya hesap olabilir.

#### <a name="how-we-ensure-the-application-is-valid"></a>Uygulamanın geçerli olduğundan nasıl emin oluruz?

Broker i çağrıştıran bir uygulamanın kimliğinin güvence ye mesi ihtiyacı, broker destekli girişlerde sağladığımız güvenlik için çok önemlidir. Ne iOS ne de Android yalnızca belirli bir uygulama için geçerli olan benzersiz tanımlayıcıları zorlar, bu nedenle kötü amaçlı uygulamalar yasal bir uygulamanın tanımlayıcısını "taklit edebilir" ve yasal uygulama için verilen belirteçleri alabilir. Çalışma zamanında her zaman doğru uygulamayla iletişim kurduğumuzdan emin olmak için, geliştiriciden uygulamalarını Microsoft'a kaydederken özel bir yönlendirmeuri sağlamasını isteriz. Geliştiriciler bu yönlendirme URI zanaat nasıl ayrıntılı olarak aşağıda ele alınmıştır. Bu özel redirectURI uygulamanın Paket Kimliğini içerir ve Apple App Store tarafından uygulamaya özgü olması sağlanır. Bir uygulama broker'ı aradığında, broker iOS işletim sisteminden aracıyı arayan Paket Kimliği'ni sağlamasını ister. Aracı, kimlik sistemimize yapılan çağrıda bu Paket Kimliğini Microsoft'a sağlar. Uygulamanın Paket Kimliği, kayıt sırasında geliştirici tarafından bize sağlanan Paket Kimliği ile eşleşmiyorsa, uygulamanın talep ettiği kaynak için belirteçlere erişimi reddetmiş oluruz. Bu denetim, yalnızca geliştirici tarafından kaydedilmiş uygulamanın belirteçleri almasını sağlar.

**Geliştirici, SDK'nın aracıyı arayıp aramadığı veya broker olmayan destekli akışı kullanıp kullanmama seçeneğine sahiptir.** Ancak geliştirici broker destekli akışı kullanmamayı seçerse, kullanıcının cihaza zaten eklemiş olabileceği SSO kimlik bilgilerini kullanma avantajını kaybeder ve uygulamalarının Microsoft'un sağladığı iş özellikleriyle kullanılmasını engeller. Koşullu Erişim, Intune yönetim yetenekleri ve sertifika tabanlı kimlik doğrulama gibi müşteriler.

Bu oturum açmaların aşağıdaki avantajları vardır:

* Kullanıcı, satıcı ne olursa olsun tüm uygulamalarında SSO'yu deneyimler.
* Uygulamanız Koşullu Erişim gibi daha gelişmiş iş özelliklerini kullanabilir veya Intune ürün paketini kullanabilir.
* Uygulamanız, iş kullanıcıları için sertifika tabanlı kimlik doğrulamasını destekleyebilir.
* Uygulamanın kimliği ve kullanıcı olarak çok daha güvenli oturum açma deneyimi, ek güvenlik algoritmaları ve şifreleme ile broker uygulaması tarafından doğrulanır.

Bu girişlerin aşağıdaki dezavantajları vardır:

* iOS'ta kimlik bilgileri seçilirken kullanıcı uygulamanızın deneyiminden uzaklara geçiş yapılır.
* Uygulamanızda müşterileriniz için giriş deneyimini yönetme yeteneğinin kaybı.

SSO'yu etkinleştirmek için SDK'ların broker uygulamalarıyla nasıl çalıştığını aşağıda açıklayabilirsiniz:

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

## <a name="enabling-cross-app-sso-using-adal"></a>ADAL kullanarak çapraz uygulama SSO'nun etkinleştirilmesi

Burada ADAL iOS SDK'yı şu şekilde kullanıyoruz:

* Uygulama paketiniz için broker destekli olmayan SSO'ya açın
* Broker destekli SSO için desteği açın

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Broker olmayan destekli SSO için SSO'nun açık

Uygulamalar arasında broker destekli olmayan SSO için SDK'lar sizin için SSO'nun karmaşıklığının çoğunu yönetir. Bu, önbellekte doğru kullanıcıyı bulmayı ve sorgulamanız için kullanıcılarda oturum açmış kullanıcıların listesini korumayı içerir.

SSO'yu sahip olduğunuz uygulamalar arasında etkinleştirmek için aşağıdakileri yapmanız gerekir:

1. Tüm uygulamalarınızın aynı İstemci Kimliği veya Uygulama Kimliği'ni kullandığından emin olun.
2. Anahtarlıkları paylaşabilmeniz için tüm uygulamalarınızın Apple'dan aynı imza sertifikasını paylaştığından emin olun.
3. Her uygulamanız için aynı anahtarlık hakkını isteyin.
4. SDK'lara kullanmamızı istediğiniz ortak anahtarzincirinden bahsedin.

#### <a name="using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Uygulama paketinizdeki tüm uygulamalar için aynı İstemci Kimliğini / Uygulama Kimliğini kullanma

Kimlik platformunun başvurularınızda belirteçleri paylaşmasına izin verildiğini bilmesi için, uygulamalarınızın her birinin aynı İstemci Kimliğini veya Uygulama Kimliğini paylaşması gerekir. Bu, portala ilk uygulamanızı kaydettiğinizde size sağlanan benzersiz tanımlayıcıdır.

Yeniden yönlendirme IU'ları, aynı Uygulama Kimliğini kullanıyorsa, farklı uygulamaları Microsoft kimlik hizmetine tanımlamanıza olanak tanır. Her uygulama, biniş portalında kayıtlı birden çok Yönlendirme URIs'ine sahip olabilir. Süitinizdeki her uygulama farklı bir yeniden yönlendirme URI olacaktır. Bunun nasıl göründüğüne bir örnek aşağıdadır:

App1 Uri Yönlendirme:`x-msauth-mytestiosapp://com.myapp.mytestapp`

App2 Redirect URI:`x-msauth-mytestiosapp://com.myapp.mytestapp2`

App3 Redirect URI:`x-msauth-mytestiosapp://com.myapp.mytestapp3`

....

Bunlar aynı istemci kimliği / uygulama kimliği altında iç içe ve SDK yapılandırmabize dönmek uri yönlendirme dayalı baktı.

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

Bu yönlendirme URI'lerinin biçimi aşağıda açıklanmıştır. Komisyoncuyu desteklemek istemiyorsanız herhangi bir Yönlendirme URI'si kullanabilirsiniz, bu durumda yukarıdaki gibi görünmeleri gerekir*

#### <a name="create-keychain-sharing-between-applications"></a>Uygulamalar arasında anahtarzinciri paylaşımı oluşturma

Anahtarlık paylaşımını etkinleştirmek bu belgenin kapsamı dışındadır ve Apple tarafından belge [Ekleme Özellikleri'nde](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html)yer almaktadır. Önemli olan, anahtarzincirinizin ne olarak adlandırılmasını istediğinize karar vermeniz ve bu özelliği tüm uygulamalarınızda eklemenizdir.

Doğru ayarlanmış yetkilendirmeleriniz olduğunda, proje dizininizde `entitlements.plist` aşağıdakilere benzeyen bir şey içeren bir dosya görmeniz gerekir:

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

Her bir uygulamanızda anahtarlık yetkilendirmesi etkinleştirildikten ve SSO'yu kullanmaya hazır olduktan sonra, aşağıdaki ayarı `ADAuthenticationSettings` kullanarak anahtarlınızın kimliğini SDK'ya bildirin:

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [!WARNING]
> Uygulamalarınız arasında bir anahtarzinciri paylaştığınızda, herhangi bir uygulama kullanıcıları silebilir veya daha kötüsü uygulamanızdaki tüm belirteçleri silebilir. Arka plan çalışması yapmak için belirteçleri güveniyor uygulamaları varsa bu özellikle felaket. Anahtarlık paylaşmak, kimlik SDK'ları aracılığıyla tüm işlemleri kaldırmada çok dikkatli olmanız gerektiği anlamına gelir.

İşte bu kadar! SDK artık tüm başvurularınızda kimlik bilgilerini paylaşacaktır. Kullanıcı listesi uygulama örnekleri arasında da paylaşılacaktır.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Broker destekli SSO için SSO'ya açma

Bir uygulamanın aygıta yüklenen herhangi bir aracıyı kullanabilmesi **varsayılan olarak kapatılır.** Aracı ile uygulamanızı kullanmak için bazı ek yapılandırma yapmak ve uygulamanıza bazı kod eklemek gerekir.

İzlenen adımlar şunlardır:

1. Uygulama kodunuzdaki MS SDK'ya yapılan çağrıda broker modunu etkinleştirin.
2. Uri'yi yeni bir yönlendirme kurun ve bunu hem uygulamanıza hem de uygulama kaydınıza sağlayın.
3. URL Düzeni kaydetme.
4. info.plist dosyanıza izin ekleyin.

#### <a name="step-1-enable-broker-mode-in-your-application"></a>Adım 1: Uygulamanızda broker modunu etkinleştirin

Kimlik Doğrulama nesnenizin "bağlamını" veya ilk kurulumunu oluşturduğunuzda uygulamanızın aracıyı kullanma özelliği açık olur. Bunu, kodunuzda kimlik bilgileri türünü ayarlayarak yaparsınız:

```
/*! See the ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
Ayar `AD_CREDENTIALS_AUTO` SDK broker aramak için denemek için izin `AD_CREDENTIALS_EMBEDDED` verecek, broker için arama SDK engelleyecektir.

#### <a name="step-2-registering-a-url-scheme"></a>Adım 2: URL Düzenini Kaydetme

Kimlik platformu, aracıyı çağırmak ve ardından denetimi uygulamanıza geri döndürmek için URL'leri kullanır. Bu gidiş-dönüş yolculuğunu tamamlamak için, başvurunuz için kimlik platformunun haberi olan bir URL şemasına ihtiyacınız vardır. Bu, daha önce uygulamanıza kaydolmuş olabileceğiniz diğer uygulama düzenlerine ek olarak olabilir.

> [!WARNING]
> Aynı URL düzenini kullanarak başka bir uygulamanın şansını en aza indirmek için URL düzenini oldukça benzersiz hale getirmenizi öneririz. Apple, uygulama mağazasında kayıtlı OLAN URL şemalarının benzersizliğini zorlamaz.

Aşağıda, bunun proje yapılandırmanızda nasıl göründüğüne bir örnek verilmiştir. Bunu XCode'da da yapabilirsiniz:

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

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a>Adım 3: URL Şemanızla yeni bir yeniden yönlendirme URI kurun

Kimlik bilgileri belirteçlerini her zaman doğru uygulamaya iade ettiğimizden emin olmak için, iOS işletim sisteminin doğrulayabildiği şekilde uygulamanızı geri aradığımızdan emin olmamız gerekir. iOS işletim sistemi, Microsoft aracısı uygulamalarına onu çağıran uygulamanın Paket Kimliğini bildirir. Bu bir haydut uygulama tarafından taklit edilemez. Bu nedenle, jetonun doğru uygulamaya döndürülmesini sağlamak için aracı uygulamamızın URI ile birlikte bu kaldıraç. Bu benzersiz yönlendirme URI'yi hem uygulamanızda hem de geliştirici portalımızda Yeniden Yönlendirme URI olarak ayarlamanızı istiyoruz.

Yeniden yönlendirme URI uygun biçimde olmalıdır:

`<app-scheme>://<your.bundle.id>`

Örn: *x-msauth-mytestiosapp://com.myapp.mytestapp*

Bu yeniden yönlendirme URI'nin [Azure portalını](https://portal.azure.com/)kullanarak uygulama kaydınızda belirtilmesi gerekir. Azure AD uygulama kaydı hakkında daha fazla bilgi için azure [etkin dizinle tümleştirme](../develop/active-directory-how-to-integrate.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)'ye bakın.

##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-to-support-certificate-based-authentication"></a>Adım 3a: Sertifika tabanlı kimlik doğrulamasını desteklemek için uygulamanıza ve geliştirme portalınıza yeniden yönlendirme URI'si ekleyin

Sertifika tabanlı kimlik doğrulamayı desteklemek için, uygulamanızda bu desteği eklemek istiyorsanız, sertifika kimlik doğrulamasını işlemek için uygulamanızda ve [Azure portalına](https://portal.azure.com/) ikinci bir "msauth"un kaydedilmesi gerekir.

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

ör: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*

#### <a name="step-4-add-a-configuration-parameter-to-your-app"></a>Adım 4: Uygulamanız için yapılandırma parametresi ekleme

ADAL kullanır –canOpenURL: aracıcihaza yüklü olup olmadığını kontrol etmek için. iOS 9'da Apple, bir uygulamanın sorgulayabileceği şemaları kilitledi. LSApplicationQueriesSchemes bölümüne "msauth" eklemeniz `info.plist file`gerekir.

```
    <key>LSApplicationQueriesSchemes</key>
    <array>
        <string>msauth</string>
    </array>

```

### <a name="youve-configured-sso"></a>SSO'ya göre yapılandırıldınız!

Artık kimlik SDK, hem uygulamalarınızda kimlik bilgilerini otomatik olarak paylaşır hem de aygıtlarında varsa aracıyı çağırır.

## <a name="next-steps"></a>Sonraki adımlar

* Tek [oturum açma SAML protokolü](../develop/single-sign-on-saml-protocol.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) hakkında bilgi edinin
