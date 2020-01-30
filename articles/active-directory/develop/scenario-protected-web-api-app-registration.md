---
title: Korumalı Web API 'SI uygulama kaydı | Mavisi
titleSuffix: Microsoft identity platform
description: Korumalı bir Web API 'SI oluşturmayı ve uygulamayı kaydettirmek için gereken bilgileri oluşturmayı öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 2d9e5d051f101288b8528b47fa88b4783a040950
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76775158"
---
# <a name="protected-web-api-app-registration"></a>Korumalı Web API 'SI: uygulama kaydı

Bu makalede, korumalı bir Web API 'SI için uygulama kaydının ayrıntıları açıklanmaktadır.

Bir uygulamayı kaydetmeye yönelik ortak adımlar için bkz. [hızlı başlangıç: Microsoft Identity platformu ile uygulama kaydetme](quickstart-register-app.md).

## <a name="accepted-token-version"></a>Kabul edilen belirteç sürümü

Microsoft Identity platform uç noktası, v 1.0 belirteçlerini ve v 2.0 belirteçlerini verebilir. Bu belirteçler hakkında daha fazla bilgi için bkz. [erişim belirteçleri](access-tokens.md).

Kabul edilen belirteç sürümü, uygulamanızı oluştururken seçtiğiniz **Desteklenen hesap türleri** değerine bağlıdır.

- **Desteklenen hesap türlerinin** değeri **herhangi bir kurumsal dizin ve kişisel Microsoft hesabında (örn. Skype, Xbox, Outlook.com) hesaplardır**, kabul edilen belirteç sürümü v 2.0 ' dır.
- Aksi takdirde, kabul edilen belirteç sürümü v 1.0 'dır.

Uygulamayı oluşturduktan sonra, aşağıdaki adımları izleyerek kabul edilen belirteç sürümünü belirleyebilir veya değiştirebilirsiniz:

1. Azure portal, uygulamanızı seçin ve ardından **bildirim**' ı seçin.
1. Bildirimde **Accesstokenacceptedversion** özelliğini bulun. Özelliğin varsayılan değeri 2 ' dir.
1. Değer, Web API 'sinin kabul ettiği belirteç sürümünün Azure Active Directory (Azure AD) olduğunu belirtir.
    - Değer 2 ise, Web API 'SI v 2.0 belirteçlerini kabul eder.
    - Değer **null**ise, Web API 'si v 1.0 belirteçlerini kabul eder.
1. Belirteç sürümünü değiştirdiyseniz **Kaydet**' i seçin.

> [!NOTE]
> Web API 'SI, kabul ettiği belirteç sürümünü belirtir. İstemci, Microsoft Identity platform (v 2.0) uç noktasından Web API 'niz için bir belirteç istediğinde, istemci, Web API 'sinin hangi belirteç sürümünü kabul ettiğini belirten bir belirteç alır.

## <a name="no-redirect-uri"></a>Yeniden yönlendirme URI 'SI yok

Kullanıcı etkileşimli olarak oturum açmamış olduğundan Web API 'Lerinin yeniden yönlendirme URI 'SI kaydetmesi gerekmez.

## <a name="exposed-api"></a>Sunulan API

Web API 'Lerine özgü diğer ayarlar, sunulan API ve sunulan kapsamlardır.

### <a name="application-id-uri-and-scopes"></a>Uygulama KIMLIĞI URI 'SI ve kapsamları

Kapsamlar genellikle form `resourceURI/scopeName`. Microsoft Graph için kapsamlar kısayollarına sahiptir. Örneğin, `User.Read` `https://graph.microsoft.com/user.read`için bir kısayoldur.

Uygulama kaydı sırasında şu parametreleri tanımlamanız gerekir:

- Kaynak URI 'SI
- Bir veya daha fazla kapsam
- Bir veya daha fazla uygulama rolü

Varsayılan olarak, uygulama kayıt portalı `api://{clientId}`Kaynak URI 'sini kullanmanızı önerir. Bu URI benzersizdir ancak insanlar okunabilir değil. URI 'yi değiştirirseniz, yeni değerin benzersiz olduğundan emin olun.

İstemci uygulamalarında kapsamlar, *temsilci izinleri* olarak görünür ve uygulama rolleri, Web API 'niz için *Uygulama izinleri* olarak gösterilir.

Kapsamlar, uygulamanızın kullanıcılarına sunulan izin penceresinde de görüntülenir. Bu nedenle, kapsamı tanımlayan karşılık gelen dizeleri sağlamanız gerekir:

- Bir kullanıcı tarafından görüldüğü gibi.
- Bir kiracı yöneticisi tarafından görüldüğü gibi, yönetici onayı de verebilir.

### <a name="exposing-delegated-permissions-scopes"></a>Temsilci izinleri gösterme (kapsamlar)

1. Uygulama kaydında **BIR API 'Yi kullanıma** sunma ' yı seçin.
1. **Kapsam ekle**’yi seçin.
1. İstenirse, **Kaydet ve devam et**' i seçerek ÖNERILEN uygulama kimliği URI 'sini (`api://{clientId}`) kabul edin.
1. Şu değerleri belirtin:
    - **Kapsam adı** ' nı seçin ve **access_as_user**girin.
    - **Kimlerin izin verebilir** ' i seçin ve **yöneticilerin ve kullanıcıların** seçili olduğundan emin olun.
    - **Yönetici onayı görünen adını** seçin ve **Kullanıcı olarak erişim TodoListService**girin.
    - **Yönetici onayı açıklaması** ' nı seçin ve **TodoListService Web API 'Sine Kullanıcı olarak erişir**yazın.
    - **Kullanıcı onayı görünen adı** ' nı seçin ve **Kullanıcı olarak erişim TodoListService**girin.
    - **Kullanıcı onay açıklaması** ' nı seçin ve **TodoListService Web API 'Sine Kullanıcı olarak erişir**yazın.
    - **Durum** değerini **etkin**olarak bırakın.
 1. **Kapsam Ekle**' yi seçin.

### <a name="if-your-web-api-is-called-by-a-daemon-app"></a>Web API 'niz bir Daemon uygulaması tarafından çağrılırsa

Bu bölümde, Daemon uygulamalarının güvenli bir şekilde çağırabilmesi için korumalı Web API 'nizi nasıl kaydedeceğinizi öğreneceksiniz.

- Yalnızca *uygulama izinlerini* bildirir ve açığa çıkarmak için Daemon uygulamaları kullanıcılarla etkileşime girmezsiniz. Temsilci izinleri anlamlı hale getirir.
- Kiracı yöneticileri, Azure AD 'nin yalnızca API 'nin uygulama izinlerinden birine erişmek üzere kayıtlı olan uygulamalara Web API belirteçleri vermesini gerektirebilir.

#### <a name="exposing-application-permissions-app-roles"></a>Uygulama izinlerini gösterme (uygulama rolleri)

Uygulama izinlerini göstermek için bildirimi düzenlemeniz gerekir.

1. Uygulamanız için uygulama kaydında, **bildirim**' ı seçin.
1. Bildirimi düzenlemek için `appRoles` ayarını bulun ve uygulama rollerini ekleyin. Rol tanımları aşağıdaki örnek JSON bloğunda verilmiştir.
1. `allowedMemberTypes` yalnızca `"Application"` olarak ayarlı bırakın.
1. `id` benzersiz bir GUID olduğundan emin olun.
1. `displayName` ve `value` boşluk içermediğinden emin olun.
1. Bildirimi kaydedin.

Aşağıdaki örnek, `id` değerinin herhangi bir benzersiz GUID olabilecek `appRoles`içeriğini gösterir.

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

#### <a name="ensuring-that-azure-ad-issues-tokens-for-your-web-api-to-only-allowed-clients"></a>Azure AD 'nin, Web API 'niz için belirteçleri yalnızca izin verilen istemcilere verdiği doğrulanıyor

Web API 'SI, uygulama rolünü denetler. Bu rol, uygulama izinlerini göstermek için bir yazılım geliştiricisi yoludur. Ayrıca, Azure AD 'yi yalnızca kiracı yöneticisinin API erişimi için onayladığı uygulamalara API belirteçleri verecek şekilde yapılandırabilirsiniz.

Bu artırılmış güvenliği eklemek için:

1. Uygulama kaydınız için uygulamaya **genel bakış** sayfasına gidin.
1. **Yerel dizinde yönetilen uygulama**altında uygulamanızın adıyla bağlantıyı seçin. Bu seçimin etiketi kesilmiş olabilir. Örneğin, **Içinde yönetilen uygulamayı görebilirsiniz...**

   > [!NOTE]
   >
   > Bu bağlantıyı seçtiğinizde **Kurumsal uygulamaya genel bakış** sayfasına gidebilirsiniz. Bu sayfa, sizin oluşturduğunuz kiracıda uygulamanız için hizmet sorumlusu ile ilişkilendirilir. Tarayıcınızın geri düğmesini kullanarak uygulama kayıt sayfasına gidebilirsiniz.

1. Kurumsal uygulama sayfalarının **Yönet** bölümünde **Özellikler** sayfasını seçin.
1. Azure AD 'nin yalnızca belirli istemcilerden Web API 'nize erişmesine izin vermek istiyorsanız, **gereken Kullanıcı atamasını** **Evet**olarak ayarlayın.

   > [!IMPORTANT]
   >
   > **Gerekli Kullanıcı atamasını** **AYARLARSANıZ, Azure**ad, bir Web API erişim belirteci istediğinde bir istemcinin uygulama rolü atamalarını denetler. İstemci herhangi bir uygulama rolüne atanmamışsa, Azure AD "invalid_client: AADSTS501051: Application \<uygulama adı\> \<Web API\>" rolüne atanmaz.
   >
   > **Kullanıcı atamasını gerekli** tutarsanız, **Hayır**olarak AYARLARSANıZ, istemci Web API 'niz için bir erişim BELIRTECI istediğinde, Azure AD uygulama rolü atamalarını denetlemez. Tüm Daemon istemcileri, istemci kimlik bilgileri akışını kullanan tüm istemciler, hedef kitini belirterek API için bir erişim belirteci alabilir. Herhangi bir uygulama, API 'ye izin istemek zorunda kalmadan erişebilir.
   >
   > Ancak, önceki bölümde açıklandığı gibi, Web API 'niz uygulamanın kiracı yöneticisi tarafından yetkilendirilen doğru rolün olduğunu her zaman doğrulayabilirler. API, erişim belirtecinin bir rol talebine sahip olduğunu ve bu talebin değerinin doğru olduğunu doğrulayarak bu doğrulamayı gerçekleştirir. Önceki JSON örneğinde, değer `access_as_application`.

1. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kodu yapılandırması](scenario-protected-web-api-app-configuration.md)
