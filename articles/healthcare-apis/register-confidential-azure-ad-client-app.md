---
title: Azure AD 'de gizli bir istemci uygulamasını kaydetme-FHIR için Azure API
description: Bir kullanıcı adına kimlik doğrulaması yapan ve kaynak uygulamalarına erişim isteyen Azure Active Directory bir gizli istemci uygulamasını kaydedin.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 6642adf5f51be1f4f54c538624829fe9004d3907
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84871879"
---
# <a name="register-a-confidential-client-application-in-azure-active-directory"></a>Gizli bir istemci uygulamasını Azure Active Directory kaydetme

Bu öğreticide, Azure Active Directory bir gizli istemci uygulamasını nasıl kaydedeceğinizi öğreneceksiniz. 

İstemci uygulama kaydı, bir kullanıcı adına kimlik doğrulaması yapmak ve [kaynak uygulamalarına](register-resource-azure-ad-client-app.md)erişim istemek için kullanılabilecek bir uygulamanın Azure Active Directory gösterimidir. Gizli bir istemci uygulaması, gizli dizi tutmak ve erişim belirteçleri istenirken gizli anahtar sağlamak için güvenilir bir uygulamadır. Gizli uygulama örnekleri, sunucu tarafı uygulamalardır.

Portalda yeni bir gizli uygulama kaydetmek için aşağıdaki adımları izleyin.

## <a name="app-registrations-in-azure-portal"></a>Azure portal Uygulama kayıtları

1. [Azure portalda](https://portal.azure.com) sol taraftaki gezinti panelinden **Azure Active Directory**’ye tıklayın.

2. **Azure Active Directory** dikey penceresinde **uygulama kayıtları**' e tıklayın:

    ![Azure portal. Yeni uygulama kaydı.](media/how-to-aad/portal-aad-new-app-registration.png)

3. **Yeni kayda**tıklayın.

## <a name="register-a-new-application"></a>Yeni uygulama kaydetme

1. Uygulamaya bir görünen ad verin.

2. Yanıt URL 'SI girin. Bu ayrıntılar daha sonra değiştirilebilir, ancak uygulamanızın yanıt URL 'sini biliyorsanız, şimdi girin.

    ![Yeni gizli Istemci uygulaması kaydı.](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT.png)

## <a name="api-permissions"></a>API izinleri

Sonraki API izinleri ekle:

1. **API izinlerini**açın:

    ![Gizli istemci. API Izinleri](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-Permissions.png)

2. **Izin Ekle** 'ye tıklayın

3. Uygun kaynak API 'SI seçin:

    FHIR (yönetilen hizmet) için Azure API 'SI için **Kuruluşumun kullandığı API 'ler** ' e tıklayın ve "Azure sağlık API 'leri" için arama yapın. Azure için açık kaynaklı FHıR sunucusu için [fhır API 'Si kaynak uygulama kaydınızı](register-resource-azure-ad-client-app.md)seçin:

    ![Gizli istemci. API 'lerim](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-MyApis.png)

4. Gizli uygulamanın bir kullanıcı adına sorabilebilmesi gereken kapsamları (izinler) seçin:

    ![Gizli istemci. Temsilci Izinleri](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-DelegatedPermissions.png)

## <a name="application-secret"></a>Uygulama gizli dizisi

1. Bir uygulama gizli anahtarı (istemci parolası) oluşturun:

    ![Gizli istemci. Uygulama gizli anahtarı](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-SECRET.png)

2. Gizli dizi açıklaması ve süresi sağlar.

3. Oluşturulduktan sonra portalda yalnızca bir kez görüntülenir. Bunu bir yere göz önünde saklayın ve güvenli bir şekilde depolayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Active Directory bir gizli istemci uygulamasının nasıl kaydedileceği hakkında bilgi edindiniz. Artık [FHıR Için Azure API](fhir-paas-powershell-quickstart.md)'yi dağıtmaya hazırsınız.

FHıR için Azure API 'yi dağıttıktan sonra, kullanılabilir ek ayarları inceleyebilirsiniz.
 
>[!div class="nextstepaction"]
>[FHIR için Azure API'sini dağıtma](fhir-paas-powershell-quickstart.md)