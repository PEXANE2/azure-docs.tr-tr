---
title: Android 'de aracılı kimlik doğrulaması | Mavisi
titleSuffix: Microsoft identity platform
description: Microsoft Identity platformunda Android için aracılı kimlik doğrulama & yetkilendirmesi 'ne genel bakış
services: active-directory
author: shoatman
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/17/2020
ms.author: shoatman
ms.custom: aaddev
ms.reviewer: shoatman, hahamil, brianmel
ms.openlocfilehash: 5042bfad2cfe06c7c368c6b476aa1b02d67bcc9c
ms.sourcegitcommit: 6a4687b86b7aabaeb6aacdfa6c2a1229073254de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91760763"
---
# <a name="brokered-authentication-in-android"></a>Android 'de aracılı kimlik doğrulaması

Cihaz genelinde çoklu oturum açma 'ya (SSO) katılmak ve kuruluş koşullu erişim ilkelerini karşılamak için Microsoft 'un kimlik doğrulama aracılarından birini kullanmanız gerekir. Bir aracı ile tümleştirme aşağıdaki avantajları sağlar:

- Cihaz çoklu oturum açma
- İçin koşullu erişim:
  - Intune Uygulama Koruması
  - Cihaz kaydı (Workplace Join)
  - Mobil Cihaz Yönetimi
- Cihaz genelinde hesap yönetimi
  -  Android AccountManager & hesap ayarlarını kullanarak
  - "İş hesabı"-özel hesap türü

Android 'de, Microsoft kimlik doğrulama Aracısı, [Microsoft Authenticator uygulama](https://play.google.com/store/apps/details?id=com.azure.authenticator) ve [Intune şirket portalı](https://play.google.com/store/apps/details?id=com.microsoft.windowsintune.companyportal)birlikte sunulan bir bileşendir.

Aşağıdaki diyagramda, uygulamanız, Microsoft kimlik doğrulama kitaplığı (MSAL) ve Microsoft 'un kimlik doğrulama aracıları arasındaki ilişki gösterilmektedir.

![Bir uygulamanın MSAL, Broker Apps ve Android Account Manager ile ilişkisini gösteren diyagram.](./media/brokered-auth/brokered-deployment-diagram.png)

## <a name="installing-apps-that-host-a-broker"></a>Aracı barındıran uygulamalar yükleme

Aracı barındırma uygulamaları, cihaz sahibi tarafından, uygulama mağazalarından (genellikle Google Play Store) herhangi bir zamanda yüklenebilir. Ancak bazı API 'Ler (kaynaklar), cihazların şunları gerektiren koşullu erişim Ilkeleriyle korunmaktadır:

- Kayıtlı (çalışma alanına katılmış) ve/veya
- Cihaz yönetimine kaydolmuş veya
- Intune Uygulama Koruması kayıtlı

Bir cihazda zaten yüklü bir aracı uygulaması yoksa, MSAL, uygulamanın bir belirteci etkileşimli olarak almaya çalıştığı anda kullanıcıya bir tane yüklemesini ister. Daha sonra uygulamanın, cihazı gerekli ilkeyle uyumlu hale getirmek için kullanıcıyı bu adımları izleyerek yapması gerekir.

## <a name="effects-of-installing-and-uninstalling-a-broker"></a>Aracıyı yükleme ve kaldırma etkileri

### <a name="when-a-broker-is-installed"></a>Bir aracı yüklendiğinde

Bir aracı bir cihaza yüklendiğinde, sonraki tüm etkileşimli Belirteç istekleri (öğesine çağrılar `acquireToken()` ), msal tarafından yerel olarak değil, aracı tarafından işlenir. Daha önce MSAL için kullanılabilir olan tüm SSO durumları, aracıda kullanılamaz. Sonuç olarak, kullanıcının yeniden kimlik doğrulaması yapması veya cihaz tarafından bilinen mevcut hesapların listesinden bir hesap seçmeniz gerekir.

Bir aracı yüklemek için kullanıcının yeniden oturum açması gerekmez. Yalnızca kullanıcının bir sorunu çözmesi gerektiğinde, bir `MsalUiRequiredException` sonraki istek aracıya gider. `MsalUiRequiredException` çeşitli nedenlerle oluşturulabilir ve etkileşimli olarak çözülmesi gerekir. Örnek:

- Kullanıcı, hesabıyla ilişkili parolayı değiştirdi.
- Kullanıcının hesabı artık bir koşullu erişim ilkesini karşılamamaktadır.
- Kullanıcı, uygulamanın hesabıyla ilişkilendirilmesi için onayını iptal etti.

#### <a name="multiple-brokers"></a>Birden çok aracılar

Bir cihaza birden çok aracı yüklüyse, önce yüklenen aracı her zaman etkin aracıdır. Bir cihazda yalnızca tek bir aracı etkin olabilir.

### <a name="when-a-broker-is-uninstalled"></a>Bir aracı kaldırıldığında

Yalnızca bir aracı barındırma uygulaması yüklüyse ve kaldırılırsa, kullanıcının yeniden oturum açması gerekecektir. Etkin Aracıyı kaldırmak, hesabı ve ilişkili belirteçleri cihazdan kaldırır.

Intune Şirket Portalı yüklenip etkin aracı olarak çalışıyorsa ve Microsoft Authenticator da yüklüyse, Intune Şirket Portalı (etkin aracı) kaldırılırsa, kullanıcının yeniden oturum açması gerekir. Yeniden oturum açtıklarında, Microsoft Authenticator uygulama etkin aracı olur.

## <a name="integrating-with-a-broker"></a>Bir aracı ile tümleştirme

### <a name="generating-a-redirect-uri-for-a-broker"></a>Bir aracı için yeniden yönlendirme URI 'SI oluşturma

Aracıda uyumlu bir yeniden yönlendirme URI 'SI kaydetmeniz gerekir. Aracının yeniden yönlendirme URI 'SI, uygulamanızın paket adını ve uygulamanızın imzasının Base64 kodlamalı gösterimini içermelidir.

Yeniden yönlendirme URI 'sinin biçimi: `msauth://<yourpackagename>/<base64urlencodedsignature>`

[Anahtar aracını](https://manpages.debian.org/buster/openjdk-11-jre-headless/keytool.1.en.html) , uygulamanızın imzalama anahtarlarını kullanarak base64 kodlu bir imza karması oluşturmak için kullanabilir ve ardından Azure Portal kullanarak YENIDEN yönlendirme URI 'nizi bu karmayı kullanarak oluşturabilirsiniz.

Linux ve macOS:

```bash
keytool -exportcert -alias androiddebugkey -keystore ~/.android/debug.keystore | openssl sha1 -binary | openssl base64
```

Windows:

```powershell
keytool -exportcert -alias androiddebugkey -keystore %HOMEPATH%\.android\debug.keystore | openssl sha1 -binary | openssl base64
```

*Keytool*ile imza karması oluşturduktan sonra, YENIDEN yönlendirme URI 'sini oluşturmak için Azure Portal kullanın:

1. [Azure Portal](https://portal.azure.com) oturum açın ve **uygulama kayıtları**Android uygulamanızı seçin.
1. **Kimlik doğrulama**  >  **platformu Ekle**  >  **Android**' i seçin.
1. Açılan **Android uygulamanızı yapılandırın** bölmesinde, daha önce oluşturduğunuz **imza karmasını** ve bir **paket adını**girin.
1. **Yapılandır** düğmesini seçin.

Azure portal, yeniden yönlendirme URI 'sini oluşturur ve **Android yapılandırma** bölmesinin **yeniden yönlendirme URI 'si** alanında görüntüler.

Uygulamanızı imzalama hakkında daha fazla bilgi için, Android Studio kullanıcı kılavuzunda [uygulamanızı](https://developer.android.com/studio/publish/app-signing) imzalama bölümüne bakın.

> [!IMPORTANT]
> Uygulamanızın üretim sürümü için üretim imzalama anahtarınızı kullanın.

### <a name="configure-msal-to-use-a-broker"></a>Aracıyı kullanmak için MSAL yapılandırma

Uygulamanızda bir aracı kullanmak için, aracı yeniden yönlendirmeyi yapılandırdığınız için test etmeniz gerekir. Örneğin, hem aracının etkin yeniden yönlendirme URI 'sini, hem de MSAL yapılandırma dosyanıza aşağıdaki ayarları ekleyerek kaydettiğiniz olduğunu belirtin:

```json
"redirect_uri" : "<yourbrokerredirecturi>",
"broker_redirect_uri_registered": true
```

### <a name="broker-related-exceptions"></a>Aracıda ilgili özel durumlar

MSAL, aracı ile iki şekilde iletişim kurar:

- Aracı ile bağlantılı hizmet
- Android AccountManager

MSAL önce bu hizmeti çağırmak hiçbir Android izni gerektirmediğinden, ilk olarak aracı ile ilişkili hizmeti kullanır. Bağlama hizmetine bağlama başarısız olursa, MSAL Android AccountManager API 'sini kullanır. MSAL yalnızca, uygulamanıza zaten izin verildiyse bunu yapar `"READ_CONTACTS"` .

`MsalClientException`Hata kodu ile karşılaşırsanız `"BROKER_BIND_FAILURE"` , iki seçenek vardır:

- Kullanıcıdan Microsoft Authenticator uygulaması ve Intune Şirket Portalı için güç iyileştirmesini devre dışı vermesini isteyin.
- Kullanıcıdan izin vermesini isteyin `"READ_CONTACTS"`

## <a name="verifying-broker-integration"></a>Aracı tümleştirmesi doğrulanıyor

Aracı tümleştirmesi 'nin çalıştığını hemen temizlemeyebilir, ancak aşağıdakileri denetlemek için aşağıdaki adımları kullanabilirsiniz:

1. Android cihazınızda, aracıyı kullanarak bir istek doldurun.
1. Android cihazınızdaki Ayarlar ' da, kimlik doğrulamasından geçen hesaba karşılık gelen yeni oluşturulan bir hesabı arayın. Hesap *iş hesabı*türünde olmalıdır.

Testi yinelemek istiyorsanız, hesabı ayarlardan kaldırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Android cihazları Için paylaşılan cihaz modu](msal-android-shared-devices.md) , bir Android cihazını birden çok çalışan tarafından kolayca paylaşılabilen şekilde yapılandırmanıza olanak tanır.
