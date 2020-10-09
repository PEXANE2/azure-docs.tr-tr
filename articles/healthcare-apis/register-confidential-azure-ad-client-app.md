---
title: Azure AD 'de gizli bir istemci uygulamasını kaydetme-FHIR için Azure API
description: Bir kullanıcı adına kimlik doğrulaması yapan ve kaynak uygulamalarına erişim isteyen Azure Active Directory bir gizli istemci uygulamasını kaydedin.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: matjazl
ms.openlocfilehash: 8021fb3fa9f11ef895569f48a2ae21b3f7adcd36
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2020
ms.locfileid: "91826220"
---
# <a name="register-a-confidential-client-application-in-azure-active-directory"></a>Gizli bir istemci uygulamasını Azure Active Directory kaydetme

Bu öğreticide, Azure Active Directory bir gizli istemci uygulamasını nasıl kaydedeceğinizi öğreneceksiniz. 

İstemci uygulama kaydı, bir kullanıcı adına kimlik doğrulaması yapmak ve [kaynak uygulamalarına](register-resource-azure-ad-client-app.md)erişim istemek için kullanılabilecek bir uygulamanın Azure Active Directory gösterimidir. Gizli bir istemci uygulaması, gizli dizi tutmak ve erişim belirteçleri istenirken gizli anahtar sağlamak için güvenilir bir uygulamadır. Gizli uygulama örnekleri, sunucu tarafı uygulamalardır.

Portalda yeni bir gizli uygulama kaydetmek için aşağıdaki adımları izleyin.

## <a name="register-a-new-application"></a>Yeni uygulama kaydetme

1. [Azure portal](https://portal.azure.com) **Azure Active Directory**gidin.

1. **Uygulama kayıtları**’nı seçin.

    ![Azure portal. Yeni uygulama kaydı.](media/how-to-aad/portal-aad-new-app-registration.png)

1. **Yeni kayıt**seçeneğini belirleyin.

1. Uygulamaya bir görünen ad verin.

1. Yanıt URL 'SI girin. Bu ayrıntılar daha sonra değiştirilebilir, ancak uygulamanızın yanıt URL 'sini biliyorsanız, şimdi girin.

    ![Yeni gizli Istemci uygulaması kaydı.](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT.png)
1. **Kaydet**’i seçin.

## <a name="api-permissions"></a>API izinleri

Uygulamanızı kaydettirdiğiniz için, bu uygulamanın kullanıcı adına isteyebilmesi gereken API izinlerini seçmeniz gerekir:

1. **API izinleri**' ni seçin.

    ![Gizli istemci. API İzinleri](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-Permissions.png)

1. **Izin Ekle**' yi seçin.

    FHıR için Azure API kullanıyorsanız, **Kuruluşumun kullandığı API**'Ler altında **Azure sağlık API 'Lerini** arayarak Azure sağlık API 'lerine bir izin ekleyeceksiniz. Bunu yalnızca [FHıR Için Azure API 'sini zaten dağıttıysanız](fhir-paas-powershell-quickstart.md)bulabilirsiniz.

    Farklı bir kaynak uygulamasına başvuruyorsam, daha önce **API 'Lerim**altında oluşturduğunuz [FHıR API kaynak uygulaması kaydınızı](register-resource-azure-ad-client-app.md) seçin.


    :::image type="content" source="media/conf-client-app/confidential-client-org-api.png" alt-text="Gizli istemci. Org API 'lerim" lightbox="media/conf-client-app/confidential-app-org-api-expanded.png":::
    

3. Gizli uygulamanın bir kullanıcı adına sorabilebilmesi gereken kapsamları (izinler) seçin:

    :::image type="content" source="media/conf-client-app/confidential-client-add-permission.png" alt-text="Gizli istemci. Org API 'lerim":::

## <a name="application-secret"></a>Uygulama gizli dizisi

1. **Sertifikalar & parolaları**' nı seçin.
1. **Yeni istemci gizli dizisi**’ni seçin. 

    ![Gizli istemci. Uygulama gizli anahtarı](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-SECRET.png)

2. Gizli dizi açıklaması ve süresi (1 yıl, 2 yıl veya hiç) sağlayın.

3. Oluşturulduktan sonra portalda yalnızca bir kez görüntülenir. Bunu bir yere göz önünde saklayın ve güvenli bir şekilde depolayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Active Directory bir gizli istemci uygulamasının nasıl kaydedileceği hakkında bilgi edindiniz. Daha sonra, Postman kullanarak FHıR sunucunuza erişebilirsiniz
 
>[!div class="nextstepaction"]
>[Postman ile FHıR için Azure API 'sine erişme](access-fhir-postman-tutorial.md)
