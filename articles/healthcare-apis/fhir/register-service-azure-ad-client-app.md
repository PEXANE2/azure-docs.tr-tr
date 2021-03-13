---
title: Azure AD 'de bir hizmet uygulaması kaydetme-FHIR için Azure API
description: Azure Active Directory bir hizmet istemci uygulamasını nasıl kaydedeceğinizi öğrenin.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: matjazl
ms.openlocfilehash: 6f7bf122b292ca144eac406957f19a13c7ba6662
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2021
ms.locfileid: "103020062"
---
# <a name="register-a-service-client-application-in-azure-active-directory"></a>Bir hizmet istemci uygulamasını Azure Active Directory kaydetme

Bu makalede, bir hizmet istemci uygulamasını Azure Active Directory nasıl kaydedeceğinizi öğreneceksiniz. İstemci uygulama kayıtları, belirteçlerin doğrulanması ve alınması için kullanılabilen uygulamaların Azure Active Directory temsilleridir. Bir hizmet istemcisi, bir kullanıcının etkileşimli kimlik doğrulaması olmadan bir erişim belirteci almak için bir uygulama tarafından kullanılmak üzere tasarlanmıştır. Belirli uygulama izinlerine sahip olur ve erişim belirteçleri elde etmek için bir uygulama gizli anahtarı (parola) kullanır.

Yeni bir hizmet istemcisi oluşturmak için aşağıdaki adımları izleyin.

## <a name="app-registrations-in-azure-portal"></a>Azure portal Uygulama kayıtları

1. [Azure portal](https://portal.azure.com) **Azure Active Directory** gidin.

2. **Uygulama kayıtları**’nı seçin.

    ![Azure portal. Yeni uygulama kaydı.](media/how-to-aad/portal-aad-new-app-registration.png)

3. **Yeni kayıt** seçeneğini belirleyin.

4. Hizmet istemcisine bir görünen ad verin. Hizmet istemci uygulamaları genellikle bir yanıt URL 'SI kullanmaz.

    :::image type="content" source="media/service-client-app/service-client-registration.png" alt-text="Azure portal. Yeni hizmet Istemci uygulaması kaydı.":::

5. **Kaydet**’i seçin.

## <a name="api-permissions"></a>API izinleri

Uygulamanızı kaydettirdiğiniz için, bu uygulamanın kullanıcı adına isteyebilmesi gereken API izinlerini seçmeniz gerekir:

1. **API izinleri**' ni seçin.
1. **Izin Ekle**' yi seçin.

    FHıR için Azure API kullanıyorsanız, **Kuruluşumun kullandığı API**'Ler altında **Azure sağlık API 'Lerini** arayarak Azure sağlık API 'lerine bir izin ekleyeceksiniz. 

    Farklı bir kaynak uygulamasına başvuruyorsam, daha önce **API 'Lerim** altında oluşturduğunuz [FHıR API kaynak uygulaması kaydınızı](register-resource-azure-ad-client-app.md) seçin.

    :::image type="content" source="media/service-client-app/service-client-org-api.png" alt-text="Gizli istemci. Org API 'lerim" lightbox="media/service-client-app/service-client-org-api-expanded.png":::

1. Gizli uygulamanın bir kullanıcı adına sorabilebilmesi gereken kapsamları (izinler) seçin:

    :::image type="content" source="media/service-client-app/service-client-add-permission.png" alt-text="Hizmet istemcisi. Temsilci Izinleri":::

1. Uygulamaya izin verin. Gerekli izinlere sahip değilseniz Azure Active Directory yöneticinize danışın:

    :::image type="content" source="media/service-client-app/service-client-grant-permission.png" alt-text="Hizmet istemcisi. Izin ver":::

## <a name="application-secret"></a>Uygulama gizli dizisi

Hizmet istemcisinin belirteç almak için bir gizli dizi (parola) gerekiyor.

1. **Sertifikalar & parolaları**' nı seçin.
2. **Yeni istemci gizli dizisi**’ni seçin.

    ![Azure portal. Hizmet Istemci parolası](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-SECRET.png)

3. Gizli dizi açıklaması ve süresi (1 yıl, 2 yıl veya hiç) sağlayın.

4. Gizli dizi oluşturulduktan sonra, portalda yalnızca bir kez görüntülenir. Bunu bir yere iade edin ve güvenli bir şekilde depolayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Active Directory bir hizmet istemci uygulamasını nasıl kaydedeceğinizi öğrendiniz. Ardından, Postman kullanarak FHıR sunucunuza erişimi test edin.
 
>[!div class="nextstepaction"]
>[Postman ile FHıR için Azure API 'sine erişme](access-fhir-postman-tutorial.md)
