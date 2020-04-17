---
title: Korumalı web API uygulama kaydı | Azure
titleSuffix: Microsoft identity platform
description: Korumalı bir web API'sini nasıl oluşturabileceğinizi ve uygulamayı kaydetmek için ihtiyacınız olan bilgileri öğrenin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 997dd98d35b74effe5d195f9a781fa0935286ee9
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537228"
---
# <a name="protected-web-api-app-registration"></a>Korumalı web API: Uygulama kaydı

Bu makalede, korumalı bir web API için uygulama kaydının özellikleri açıklanmaktadır.

Bir uygulamayı kaydetmek için ortak adımlar için [Quickstart: Microsoft kimlik platformuna bir uygulama kaydolun.](quickstart-register-app.md)

## <a name="accepted-token-version"></a>Kabul edilen belirteç sürümü

Microsoft kimlik platformu bitiş noktası v1.0 belirteçleri ve v2.0 belirteçleri verebilir. Bu belirteçler hakkında daha fazla bilgi için [Erişim belirteçleri'ne](access-tokens.md)bakın.

Kabul edilen belirteç sürümü, uygulamanızı oluştururken seçtiğiniz **Desteklenen hesap türleri** değerine bağlıdır.

- Desteklenen hesap **türlerinin** değeri **herhangi bir kuruluş dizinindeki ve kişisel Microsoft hesaplarındaki (örneğin Skype, Xbox, Outlook.com) Hesaplar**ise, kabul edilen belirteç sürümü v2.0'dır.
- Aksi takdirde, kabul edilen belirteç sürümü v1.0'dır.

Uygulamayı oluşturduktan sonra, kabul edilen belirteç sürümünü aşağıdaki adımları izleyerek belirleyebilir veya değiştirebilirsiniz:

1. Azure portalında uygulamanızı seçin ve ardından **Bildirim'i**seçin.
1. Özellik **erişimTokenAcceptedVersion'u** bildirimde bulun. Özelliğin varsayılan değeri 2'dir.
1. Değer, web API'nin kabul ettiği sürümdeki Azure Etkin Dizini'ne (Azure AD) göredeğişir.
    - Değer 2 ise, web API v2.0 belirteçleri kabul eder.
    - Değer **null**ise, web API v1.0 belirteçleri kabul eder.
1. Belirteç sürümünü değiştirdiyseniz, **Kaydet'i**seçin.

> [!NOTE]
> Web API, hangi belirteç sürümünü kabul ettiği belirtilir. İstemci, Microsoft kimlik platformundan (v2.0) web API'niz için bir belirteç istediğinde, istemci web API'nin hangi belirteç sürümünü kabul ettiğini gösteren bir belirteç alır.

## <a name="no-redirect-uri"></a>URI'yi yönlendirmeyok

Hiçbir kullanıcı etkileşimli olarak oturum etmediğinden, Web API'leri yeniden yönlendirme URI'sini kaydetmeye gerek yoktur.

## <a name="exposed-api"></a>Maruz KALAN API

Web API'lerine özgü diğer ayarlar, açıkta kalan API'ler ve açıkta kalan kapsamlardır.

### <a name="application-id-uri-and-scopes"></a>Uygulama Kimliği URI ve kapsamları

Kapsamlar genellikle forma `resourceURI/scopeName`sahiptir. Microsoft Graph için kapsamların kısayolları vardır. Örneğin, `User.Read` `https://graph.microsoft.com/user.read`bir kısayol.

Uygulama kaydı sırasında şu parametreleri tanımlamanız gerekir:

- Kaynak URI
- Bir veya daha fazla kapsam
- Bir veya daha fazla uygulama rolü

Varsayılan olarak, uygulama kayıt portalı kaynak URI `api://{clientId}`kullanmanızı önerir. Bu URI benzersiz ama insan okunabilir değildir. URI'yi değiştirirseniz, yeni değerin benzersiz olduğundan emin olun.

İstemci uygulamalarında *kapsamlar, yetkin izinler* olarak gösterir ve uygulama rolleri web API'nız için *uygulama izinleri* olarak gösteriş gösterir.

Kapsamlar, uygulamanızın kullanıcılarına sunulan onay penceresinde de görünür. Bu nedenle kapsamı açıklayan karşılık gelen dizeleri sağlamanız gerekir:

- Bir kullanıcı tarafından görüldüğü gibi.
- Yönetici nin rızasını verebilecek bir kiracı yönetici tarafından görüldüğü gibi.

### <a name="exposing-delegated-permissions-scopes"></a>Devredilen izinlerin (kapsamların) açığa çıkarılması

1. Uygulama kaydında **API'yi ortaya çıkar'ı** seçin.
1. **Kapsam ekle**’yi seçin.
1. İstenirse, Kaydet ve`api://{clientId}`Devam'ı seçerek önerilen uygulama kimliği URI () seçeneğini kabul **edin.**
1. Bu değerleri belirtin:
    - **Kapsam adını** seçin ve **access_as_user**girin.
    - **Kimlerin onay kullanabileceğini** seçin ve **Yöneticilerin ve kullanıcıların** seçildiğinden emin olun.
    - **Yönetici onayı ekran adını** seçin ve Kullanıcı olarak Access **TodoListService**girin.
    - **Yönetici onayı açıklamasını** seçin ve kullanıcı olarak **TodoListService web API'sine erişin.**
    - **Kullanıcı onayı ekran adını** seçin ve Kullanıcı olarak Access **TodoListService**girin.
    - **Kullanıcı onayı açıklamasını** seçin ve kullanıcı olarak **TodoListService web API'sine erişin.**
    - **Durum** değerini **Etkin**olarak ayarlamayı tutun.
 1. **Kapsam Ekle'yi**seçin.

### <a name="if-your-web-api-is-called-by-a-daemon-app"></a>Web API'niz bir daemon uygulaması tarafından çağrılırsa

Bu bölümde, daemon uygulamalarının güvenli bir şekilde arayabilmeleri için korumalı web API'nizi nasıl kaydedebileceğinizi öğrenirsiniz.

- Daemon uygulamaları kullanıcılarla etkileşimde olmadığı için yalnızca *uygulama izinlerini* beyan eder ve açıklarsınız. Yetki niçin bir anlamı yok.
- Kiracı yöneticiler, Azure AD'nin web API belirteçlerini yalnızca API'nin uygulama izinlerinden birine erişmek için kaydolmuş uygulamalara yayınlamasını gerektirebilir.

#### <a name="exposing-application-permissions-app-roles"></a>Uygulama izinlerinin açığa çıkarılması (uygulama rolleri)

Uygulama izinlerini ortaya çıkarmak için bildirimi nizgerekir.

1. Başvurunuz için başvuru **kaydında, Manifesto'yu**seçin.
1. Bildirimi ayarlamak için `appRoles` ayarı bulun ve uygulama rolleri ekleyin. Rol tanımları aşağıdaki örnek JSON bloğunda verilmiştir.
1. `allowedMemberTypes` Sadece ayarlayın. `"Application"`
1. Benzersiz `id` bir GUID olduğundan emin olun.
1. Boşluk `displayName` içerdiğinden emin olun ve `value` almayın.
1. Manifestoyu kaydedin.

Aşağıdaki örnek, değerin `appRoles`benzersiz bir `id` GUID olabileceği içeriği gösterir.

```JSon
"appRoles": [
    {
    "allowedMemberTypes": [ "Application" ],
    "description": "Accesses the TodoListService-Cert as an application.",
    "displayName": "access_as_application",
    "id": "ccf784a6-fd0c-45f2-9c08-2f9d162a0628",
    "isEnabled": true,
    "lang": null,
    "origin": "Application",
    "value": "access_as_application"
    }
],
```

#### <a name="ensuring-that-azure-ad-issues-tokens-for-your-web-api-to-only-allowed-clients"></a>Azure AD'nin web API'nizin yalnızca istemcilere izin vermesine izin vermek için belirteçler verdiğine emin olun

Web API'si uygulama rolünü denetler. Bu rol, bir yazılım geliştiricisinin uygulama izinlerini ortaya çıkarma yoludur. Azure AD'yi, API belirteçlerini yalnızca kiracı yöneticinin API erişimi için onayladığı uygulamalarla birlikte yapılandırabilirsiniz.

Bu artırılmış güvenliği eklemek için:

1. Uygulama kaydınız için uygulamaya **Genel Bakış** sayfasına gidin.
1. **Yerel dizinde Yönetilen uygulama**altında, uygulamanızın adını içeren bağlantıyı seçin. Bu seçimin etiketi kesilmiş olabilir. Örneğin, **Yönetilen uygulamayı görebilirsiniz ...**

   > [!NOTE]
   >
   > Bu bağlantıyı seçtiğinizde, Kurumsal **Uygulamaya Genel Bakış** sayfasına gidersiniz. Bu sayfa, oluşturduğunuz kiracıdaki uygulamanızın servis sorumlusuyla ilişkilidir. Tarayıcınızın arka düğmesini kullanarak uygulama kayıt sayfasına gidebilirsiniz.

1. Kurumsal uygulama sayfalarının **Yönet** bölümündeki **Özellikler** sayfasını seçin.
1. Azure AD'nin web API'nıza yalnızca belirli istemcilerden erişmesine izin **Yes**vermek istiyorsanız, **Kullanıcı atamasını gerekli** olarak ayarlar sınız?

   > [!IMPORTANT]
   >
   > **Kullanıcı atamasını gerekli** olarak **Yes**ayarlarsanız? İstemci herhangi bir uygulama rolüne atanmamışsa, Azure AD hata iletisini döndürecek "invalid_client: AADSTS501051: Uygulama \<uygulama adı\> \<web API'si\>için bir role atanmadı".
   >
   > **Kullanıcı atamasını gerekli tutarsanız?** **No** Herhangi bir daemon istemcisi, yani istemci kimlik bilgilerini kullanan herhangi bir istemci, yalnızca hedef kitlesini belirterek API için bir erişim belirteci alabilirsiniz. Herhangi bir uygulama, izin istemeden API'ye erişebilir.
   >
   > Ancak önceki bölümde açıklandığı gibi, web API'niz her zaman uygulamanın kiracı yönetici tarafından yetkilendirilen doğru role sahip olduğunu doğrulayabilir. API, bu doğrulamayı, erişim belirtecinin bir rol talebi olduğunu ve bu talebin değerinin doğru olduğunu doğrulayarak gerçekleştirir. Önceki JSON örneğinde, değer `access_as_application`.

1. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kodu yapılandırması](scenario-protected-web-api-app-configuration.md)
