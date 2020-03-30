---
title: Uygulamalar için Hizmet Şartları ve gizlilik bildirimi | Azure
description: Azure AD kullanmak üzere kayıtlı uygulamalar için hizmet koşullarını ve gizlilik bildirimini nasıl yapılandırabileceğinizi öğrenin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: ryanwi
ms.reviwer: lenalepa, sureshja
ms.custom: aaddev
ms.openlocfilehash: 8fc85781f139b45e9e37f6e0f7cc36974041352d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78300018"
---
# <a name="how-to-configure-terms-of-service-and-privacy-statement-for-an-app"></a>Nasıl yapIlir: Bir uygulama için hizmet koşullarını ve gizlilik bildirimini yapılandırma

Azure Active Directory (Azure AD) ve Microsoft hesaplarıyla tümleşen uygulamalar oluşturan ve yöneten geliştiriciler, uygulamanın hizmet koşullarına ve gizlilik bildirimine bağlantılar içermelidir. Hizmet koşulları ve gizlilik bildirimi, kullanıcı onayı deneyimi aracılığıyla kullanıcılara açıklanır. Kullanıcılarınızın uygulamanıza güvenebileceklerini bilmelerine yardımcı olurlar. Hizmet koşulları ve gizlilik bildirimi, özellikle kullanıcıya bakan çok kiracılı uygulamalar için çok önemlidir-- birden çok dizin tarafından kullanılan veya herhangi bir Microsoft hesabında kullanılabilen uygulamalar.

Uygulamanız için hizmet koşulları ve gizlilik bildirimi belgelerini oluşturmaktan ve bu belgelere URL sağlamaktan siz sorumlusunuz. Bu bağlantıları sağlayamayan çok kiracılı uygulamalariçin, uygulamanızın kullanıcı onayı deneyimi, kullanıcıların uygulamanızı kabul etmesini engelleyebilecek bir uyarı gösterir.

> [!NOTE]
> * Tek kiracılı uygulamalar bir uyarı göstermez.
> * İki bağlantıdan biri veya her ikisi eksikse, uygulamanız bir uyarı gösterir.

## <a name="user-consent-experience"></a>Kullanıcı onayı deneyimi

Aşağıdaki örnekler, hizmet koşulları ve gizlilik bildirimi yapılandırıldığında ve bu bağlantılar yapılandırılmadığını gösteren kullanıcı onayı deneyimini gösterir.

![Gizlilik bildirimi ve sağlanan hizmet koşulları ile ve olmadan ekran görüntüleri](./media/howto-add-terms-of-service-privacy-statement/user-consent-exp-privacy-statement-terms-service.png)

## <a name="formatting-links-to-the-terms-of-service-and-privacy-statement-documents"></a>Hizmet şartları ve gizlilik bildirimi belgelerine bağlantıları biçimlendirme

Uygulamanızın hizmet koşulları ve gizlilik bildirimi belgelerine bağlantılar eklemeden önce URL'lerin bu yönergelere uyduğundan emin olun.

| Yönerge     | Açıklama                           |
|---------------|---------------------------------------|
| Biçimlendir        | Geçerli URL                             |
| Geçerli şemalar | HTTP ve HTTPS<br/>HTTPS'yi öneririz |
| Uzunluk üst sınırı    | 2048 karakter                       |

Örnekler: `https://myapp.com/terms-of-service` ve`https://myapp.com/privacy-statement`

## <a name="adding-links-to-the-terms-of-service-and-privacy-statement"></a>Hizmet şartları ve gizlilik bildirimine bağlantılar ekleme

Hizmet koşulları ve gizlilik bildirimi hazır olduğunda, aşağıdaki yöntemlerden birini kullanarak uygulamanızda bu belgelere bağlantılar ekleyebilirsiniz:

* [Azure portalı aracılığıyla](#azure-portal)
* [Uygulama nesnesi JSON'u kullanma](#app-object-json)
* [Microsoft Grafik API'sini kullanma](#msgraph-rest-api)

### <a name="using-the-azure-portal"></a><a name="azure-portal"></a>Azure portalını kullanma
Azure portalında aşağıdaki adımları izleyin.

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. **Uygulama Kayıtları** bölümüne gidin ve uygulamanızı seçin.
3. Marka **bölmesini** açın.
4. **Hizmet Şartları URL'si** ve **Gizlilik Bildirimi URL** alanlarını doldurun.
5. Yaptığınız değişiklikleri kaydedin.

    ![Uygulama özellikleri hizmet koşulları ve gizlilik bildirimi URL'leri içerir](./media/howto-add-terms-of-service-privacy-statement/azure-portal-terms-service-privacy-statement-urls.png)

### <a name="using-the-app-object-json"></a><a name="app-object-json"></a>Uygulama nesnesi JSON'u kullanma

Uygulama nesnesi JSON'u doğrudan değiştirmeyi tercih ederseniz, uygulamanızın hizmet şartları ve gizlilik bildirimine bağlantılar eklemek için Azure portalındaki veya Uygulama Kayıt Portalı'ndaki bildirim düzenleyicisini kullanabilirsiniz.

```json
    "informationalUrls": { 
        "termsOfService": "<your_terms_of_service_url>", 
        "privacy": "<your_privacy_statement_url>" 
    }
```

### <a name="using-the-microsoft-graph-api"></a><a name="msgraph-rest-api"></a>Microsoft Grafik API'sini kullanma

Tüm uygulamalarınızı programlı bir şekilde güncellemek için, hizmet şartları ve gizlilik bildirimi belgelerine bağlantılar eklemek için tüm uygulamalarınızı güncelleştirmek için Microsoft Graph API'yi kullanabilirsiniz.

```
PATCH https://graph.microsoft.com/v1.0/applications/{application id}
{ 
    "appId": "{your application id}", 
    "info": { 
        "termsOfServiceUrl": "<your_terms_of_service_url>", 
        "supportUrl": null, 
        "privacyStatementUrl": "<your_privacy_statement_url>", 
        "marketingUrl": null, 
        "logoUrl": null 
    }
}
```

> [!NOTE]
> * Bu alanlardan herhangi biri için atadığınız önceden varolan `supportUrl`değerlerin `marketingUrl`üzerine yazmamaya dikkat edin: , ve`logoUrl`
> * Microsoft Graph API yalnızca bir Azure REKLAM hesabıyla oturum açtettiğinizde çalışır. Kişisel Microsoft hesapları desteklenmez.
