---
title: Android'de aracılı kimlik doğrulama | Azure
titlesuffix: Microsoft identity platform
description: Microsoft kimlik platformunda Android için aracılı kimlik doğrulama & yetkilendirmeye genel bakış
services: active-directory
author: shoatman
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/14/2019
ms.author: shoatman
ms.custom: aaddev
ms.reviewer: shoatman, hahamil, brianmel
ms.openlocfilehash: a734589178438fd65d9a2d156fd91fc82807f578
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76697906"
---
# <a name="brokered-authentication-in-android"></a>Android'de aracılı kimlik doğrulaması

Aygıt genelinde Ki Tek Oturum Açma'ya (SSO) katılmak ve kuruluş Koşullu Erişim ilkelerini karşılamak için Microsoft'un kimlik doğrulama aracılarından birini kullanmanız gerekir. Bir komisyoncu ile tümleştirme aşağıdaki avantajları sağlar:

- Cihaz tek oturum açma
- Koşullu erişim:
  - Intune Uygulama Koruması
  - Cihaz Kaydı (İşyeri Birleştirme)
  - Mobil Cihaz Yönetimi
- Cihaz çapında Hesap Yönetimi
  -  Android AccountManager & Hesap Ayarları üzerinden
  - "İş Hesabı" - özel hesap türü

Android'de, Microsoft Authentication Broker [Microsoft Authenticator App](https://play.google.com/store/apps/details?id=com.azure.authenticator) ve [Intune Company Portal](https://play.google.com/store/apps/details?id=com.microsoft.windowsintune.companyportal) ile birlikte bir bileşenidir

> [!TIP]
> Brokeri barındıran tek bir uygulama aynı anda broker olarak etkin olacaktır. Broker olarak hangi uygulamanın aktif olduğu cihazdaki kurulum emri ile belirlenir. İlk yüklenen veya cihazdaki son hediye, etkin broker olur.

Aşağıdaki diyagram, uygulamanız, Microsoft Kimlik Doğrulama Kitaplığı (MSAL) ve Microsoft'un kimlik doğrulama aracıları arasındaki ilişkiyi göstermektedir.

![Broker Dağıtım Diyagramı](./media/brokered-auth/brokered-deployment-diagram.png)

## <a name="installing-apps-that-host-a-broker"></a>Broker barındıran uygulamaları yükleme

Aracı barındırma uygulamaları, cihaz sahibi tarafından herhangi bir zamanda uygulama mağazasından (genellikle Google Play Store) yüklenebilir. Ancak, bazı API'ler (kaynaklar) aygıtların aşağıdakileri olmasını gerektiren Koşullu Erişim İlkeleri tarafından korunur:

- Kayıtlı (işyeri katıldı) ve/veya
- Cihaz Yönetimine kaydolması veya
- Intune Uygulama Koruması'na kaydoldu

Bir aygıtın zaten yüklü bir broker uygulaması yoksa, MSAL kullanıcıya uygulama etkileşimli olarak bir belirteç almaya çalışır çalışmaz bir tane yüklemesini bildirir. Uygulamanın daha sonra cihazı gerekli ilkeyle uyumlu hale getirmek için kullanıcıyı bu adımlara yönlendirmesi gerekir.

## <a name="effects-of-installing-and-uninstalling-a-broker"></a>Bir komisyoncuyükleme nin ve kaldırmanın etkileri

### <a name="when-a-broker-is-installed"></a>Bir komisyoncu yüklendiğinde

Bir aygıta bir aracı yüklendiğinde, sonraki tüm etkileşimli `acquireToken()`belirteç istekleri (aramalar) MSAL tarafından yerel olarak değil, broker tarafından işlenir. Daha önce MSAL tarafından kullanılabilen herhangi bir SSO durumu komisyoncu tarafından kullanılamaz. Sonuç olarak, kullanıcının yeniden kimlik doğrulaması veya aygıt tarafından bilinen varolan hesaplar listesinden bir hesap seçmesi gerekir.

Bir aracıyüklemek, kullanıcının yeniden oturum açmasını gerektirmez. Yalnızca kullanıcının bir `MsalUiRequiredException` isteği çözmesi gerektiğinde bir sonraki istek komisyoncuya gider. `MsalUiRequiredException`birkaç nedenden dolayı atılır ve etkileşimli olarak çözülmesi gerekir. Bu bazı yaygın nedenleri şunlardır:

- Kullanıcı, hesabıyla ilişkili parolayı değiştirdi.
- Kullanıcının hesabı artık bir Koşullu Erişim ilkesini karşılamaz.
- Kullanıcı, uygulamanın hesabıyla ilişkilendirilmesi için onayını iptal etti.

### <a name="when-a-broker-is-uninstalled"></a>Bir komisyoncu kaldırıldığında

Yalnızca bir broker barındırma uygulaması yüklüyse ve bu uygulama kaldırılırsa, kullanıcının yeniden oturum açması gerekir. Etkin aracının kaldırılması hesabı ve ilişkili belirteçleri aygıttan kaldırır.

Intune Company Portal yüklüyse ve etkin broker olarak çalışıyorsa ve Microsoft Authenticator da yüklüyse, Intune Company Portal (etkin broker) yüklenirse kullanıcının yeniden oturum açması gerekir. Yeniden oturum açmalarında, Microsoft Authenticator uygulaması etkin aracı olur.

## <a name="integrating-with-a-broker"></a>Komisyoncu ile tümleştirme

### <a name="generating-a-redirect-uri-for-a-broker"></a>Bir komisyoncu için yeniden yönlendirme URI oluşturma

Komisyoncu ile uyumlu bir yeniden yönlendirme URI kayıt gerekir. Aracı için URI yönlendirme uygulamanızın paket adının yanı sıra uygulamanızın imzasının base64 kodlanmış temsilini de içermelidir.

URI yönlendirme biçimi:`msauth://<yourpackagename>/<base64urlencodedsignature>`

Uygulamanızın imza anahtarlarını kullanarak Base64 url kodlanmış imzanızı oluşturun. Hata ayıklama imzalama anahtarlarınızı kullanan bazı örnek komutlar aşağıda verilmiştir:

#### <a name="macos"></a>macOS

```bash
keytool -exportcert -alias androiddebugkey -keystore ~/.android/debug.keystore | openssl sha1 -binary | openssl base64
```

#### <a name="windows"></a>Windows

```powershell
keytool -exportcert -alias androiddebugkey -keystore %HOMEPATH%\.android\debug.keystore | openssl sha1 -binary | openssl base64
```

Uygulamanızı imzalama hakkında bilgi için [uygulamanızı imzalayın.](https://developer.android.com/studio/publish/app-signing)

> [!IMPORTANT]
> Uygulamanızın üretim sürümü için üretim imzalama anahtarınızı kullanın.

### <a name="configure-msal-to-use-a-broker"></a>MSAL'ı broker kullanacak şekilde yapılandırın

Uygulamanızda bir aracı kullanmak için, aracınızı yeniden yönlendirdiğinizi doğrulamalısınız. Örneğin, hem aracınızın etkinleştirilen yeniden yönlendirme URI'yi hem de MSAL yapılandırma dosyanıza aşağıdakileri ekleyerek kaydettiğinizi belirtin:

```javascript
"redirect_uri" : "<yourbrokerredirecturi>",
"broker_redirect_uri_registered": true
```

> [!TIP]
> Yeni Azure portal uygulama kaydı UI, aracının URI'yi yeniden yönlendirmesini oluşturmanıza yardımcı olur. Uygulamanızı eski deneyimi kullanarak kaydettiyseniz veya Bunu Microsoft uygulama kayıt portalını kullanarak kaydettiyseniz, yeniden yönlendirme URI'sini oluşturmanız ve portaldaki yeniden yönlendirme URI'lerinin listesini el ile güncelleştirmeniz gerekebilir.

### <a name="broker-related-exceptions"></a>Broker ile ilgili özel durumlar

MSAL komisyoncu ile iki şekilde iletişim kurar:

- Broker bağlı hizmet
- Android Hesap Yöneticisi

MSAL, bu hizmeti aramanın herhangi bir Android izni gerektirmediği için önce broker bağlı hizmetini kullanır. Bağlı hizmete bağlanma başarısız olursa, MSAL Android AccountManager API'sını kullanır. MSAL bunu yalnızca uygulamanız zaten izin `"READ_CONTACTS"` verilmişse yapar.

Hata kodu `"BROKER_BIND_FAILURE"` `MsalClientException` ile alırsanız, iki seçenek vardır:

- Kullanıcıdan Microsoft Authenticator uygulaması ve Intune Company Portalı için güç optimizasyonunu devre dışı etmesini isteyin.
- Kullanıcıdan `"READ_CONTACTS"` izin vermesini isteyin
