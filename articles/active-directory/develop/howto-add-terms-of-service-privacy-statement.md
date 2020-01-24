---
title: Uygulamalar için hizmet koşulları ve gizlilik bildirimi | Mavisi
description: Azure AD 'yi kullanmak üzere kayıtlı uygulamalar için hizmet koşulları 'nı ve gizlilik bildirimi 'ni nasıl yapılandırabileceğinizi öğrenin.
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
ms.openlocfilehash: f28c33f20556825d84edda34752ac64714327526
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76697345"
---
# <a name="how-to-configure-terms-of-service-and-privacy-statement-for-an-app"></a>Nasıl yapılır: bir uygulama için hizmet koşullarını ve gizlilik bildirimini yapılandırma

Azure Active Directory (Azure AD) ve Microsoft hesaplarıyla tümleştirilen uygulamaları oluşturan ve yöneten geliştiriciler, uygulamanın hizmet koşulları ve gizlilik bildirimi bağlantılarını içermelidir. Hizmet koşulları ve gizlilik bildirimi, kullanıcılar tarafından Kullanıcı onay deneyimi aracılığıyla ortaya çıkmış. Kullanıcılarınıza, uygulamanıza güvenebileceklerini bilmeleri için yardımcı olurlar. Hizmet koşulları ve gizlilik bildirimi, birden çok dizin tarafından kullanılan veya tüm Microsoft hesabı kullanılabilen çok kiracılı uygulamalar için özellikle önemlidir.

Uygulamanız için hizmet koşulları ve gizlilik bildirimi belgelerinin oluşturulması ve bu belgelere URL 'Ler sağlanması sizin sorumluluğunuzdadır. Bu bağlantıları sağlamayan çok kiracılı uygulamalarda, uygulamanız için Kullanıcı onay deneyimi bir uyarı gösterir ve bu da kullanıcıların uygulamanıza uyum sağlamasını engelleyebilir.

> [!NOTE]
> * Tek kiracılı uygulamalar, bir uyarı göstermez.
> * İki bağlantılardan biri veya her ikisi de eksikse, uygulamanız bir uyarı gösterir.

## <a name="user-consent-experience"></a>Kullanıcı onayı deneyimi

Aşağıdaki örneklerde, hizmet koşulları ve gizlilik bildirimi yapılandırıldığında ve bu bağlantılar yapılandırılmadığında Kullanıcı onay deneyimi gösterilmektedir.

![Gizlilik bildirimi ve hizmet koşulları ile ve olmadan ekran görüntüleri](./media/howto-add-terms-of-service-privacy-statement/user-consent-exp-privacy-statement-terms-service.png)

## <a name="formatting-links-to-the-terms-of-service-and-privacy-statement-documents"></a>Hizmet koşulları ve gizlilik bildirimi belgeleri için bağlantıları biçimlendirme

Uygulamanızın hizmet koşulları ve gizlilik bildirimi belgeleri için bağlantıları eklemeden önce, URL 'Lerin bu yönergeleri izlediğinden emin olun.

| Yönerge     | Açıklama                           |
|---------------|---------------------------------------|
| Biçimlendir        | Geçerli URL                             |
| Geçerli şemalar | HTTP ve HTTPS<br/>HTTPS önerilir |
| Uzunluk üst sınırı    | 2048 karakter                       |

Örnekler: `https://myapp.com/terms-of-service` ve `https://myapp.com/privacy-statement`

## <a name="adding-links-to-the-terms-of-service-and-privacy-statement"></a>Hizmet koşullarına ve gizlilik bildirimine bağlantılar ekleme

Hizmet koşulları ve gizlilik bildirimi hazırlandığı zaman, aşağıdaki yöntemlerden birini kullanarak uygulamanıza bu belgelere bağlantılar ekleyebilirsiniz:

* [Azure portal aracılığıyla](#azure-portal)
* [JSON uygulama nesnesini kullanma](#app-object-json)
* [MSGraph Beta REST API kullanma](#msgraph-beta-rest-api)

### <a name="azure-portal"></a>Azure portal kullanma
Azure portal aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com/)’ında oturum açın.
2. **Uygulama kayıtları** bölümüne gidin ve uygulamanızı seçin.
3. **Marka** bölmesini açın.
4. **Hizmet koşulları URL 'sini** ve **Gizlilik bildirimi URL 'si** alanlarını doldurun.
5. Yaptığınız değişiklikleri kaydedin.

    ![Uygulama özellikleri, hizmet koşullarını ve gizlilik bildirimi URL 'Lerini içerir](./media/howto-add-terms-of-service-privacy-statement/azure-portal-terms-service-privacy-statement-urls.png)

### <a name="app-object-json"></a>JSON uygulama nesnesini kullanma

JSON uygulama nesnesini doğrudan değiştirmeyi tercih ediyorsanız, uygulamanızın hizmet koşullarına ve gizlilik bildirimine yönelik bağlantıları dahil etmek için Azure portal veya uygulama kayıt portalındaki bildirim düzenleyicisini kullanabilirsiniz.

```json
    "informationalUrls": { 
        "termsOfService": "<your_terms_of_service_url>", 
        "privacy": "<your_privacy_statement_url>" 
    }
```

### <a name="msgraph-beta-rest-api"></a>MSGraph Beta REST API kullanma

Tüm uygulamalarınızı program aracılığıyla güncelleştirmek için, tüm uygulamalarınızı hizmet koşulları ve gizlilik bildirimi belgelerinin bağlantılarını içerecek şekilde güncelleştirmek üzere MSGraph Beta REST API kullanabilirsiniz.

```
PATCH https://graph.microsoft.com/beta/applications/{application id}
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
> * Bu alanlardan birine atadığınız önceden var olan değerlerin üzerine yazılmamaya dikkat edin: `supportUrl`, `marketingUrl`ve `logoUrl`
> * MSGraph Beta REST API yalnızca bir Azure AD hesabıyla oturum açtığınızda çalışır. Kişisel Microsoft hesapları desteklenmez.
