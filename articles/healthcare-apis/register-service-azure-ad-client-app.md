---
title: Azure AD 'de bir hizmet uygulaması kaydetme-FHIR için Azure API
description: Bir hizmet istemci uygulamasını, belirteçlerin kimliğini doğrulamak ve almak için kullanılabilecek Azure Active Directory nasıl kaydedeceğinizi öğrenin.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: matjazl
ms.openlocfilehash: 34eec3ad0d2fc193744898b6f08cbe50c261c945
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87853033"
---
# <a name="register-a-service-client-application-in-azure-active-directory"></a>Bir hizmet istemci uygulamasını Azure Active Directory kaydetme

Bu makalede, bir hizmet istemci uygulamasını Azure Active Directory nasıl kaydedeceğinizi öğreneceksiniz. İstemci uygulama kayıtları, belirteçlerin doğrulanması ve alınması için kullanılabilen uygulamaların Azure Active Directory temsilleridir. Bir hizmet istemcisi, bir kullanıcının etkileşimli kimlik doğrulaması olmadan bir erişim belirteci almak için bir uygulama tarafından kullanılmak üzere tasarlanmıştır. Belirli uygulama izinlerine sahip olur ve erişim belirteçleri elde etmek için bir uygulama gizli anahtarı (parola) kullanır.

Yeni bir hizmet istemcisi oluşturmak için aşağıdaki adımları izleyin.

## <a name="app-registrations-in-azure-portal"></a>Azure portal Uygulama kayıtları

1. [Azure portalda](https://portal.azure.com) sol taraftaki gezinti panelinden **Azure Active Directory**’ye tıklayın.

2. **Azure Active Directory** dikey penceresinde **uygulama kayıtları**' e tıklayın:

    ![Azure portal. Yeni uygulama kaydı.](media/how-to-aad/portal-aad-new-app-registration.png)

3. **Yeni kayıt**’a tıklayın.

## <a name="service-client-application-details"></a>Hizmet istemci uygulaması ayrıntıları

* Hizmet istemcisi bir görünen ada ihtiyaç duyuyor ve bir yanıt URL 'SI de sağlayabilirsiniz, ancak genellikle kullanılmaz.

    ![Azure portal. Yeni hizmet Istemci uygulaması kaydı.](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-NAME.png)

## <a name="api-permissions"></a>API izinleri

Hizmet istemci uygulaması rollerine izin vermeniz gerekir. 

1. **API izinlerini** açın ve [FHıR API kaynak uygulaması kaydınızı](register-resource-azure-ad-client-app.md)seçin. FHıR için Azure API kullanıyorsanız, **Kuruluşumun kullandığı API**'Ler altında Azure sağlık API 'Lerini arayarak Azure sağlık API 'lerine bir izin ekleyeceksiniz.

    ![Azure portal. Hizmet Istemcisi API 'SI Izinleri](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-API-PERMISSIONS.png)

2. Kaynak uygulamasında tanımlandıklarından istediğiniz uygulama rollerini seçin:

    ![Azure portal. Hizmet Istemcisi uygulama Izinleri](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-APPLICATION-PERMISSIONS.png)

3. Uygulamaya izin verin. Gerekli izinlere sahip değilseniz Azure Active Directory yöneticinize danışın:

    ![Azure portal. Hizmet Istemcisi yönetici onayı](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-ADMIN-CONSENT.png)

## <a name="application-secret"></a>Uygulama gizli dizisi

Hizmet istemcisi, belirteçleri alırken kullanacağınız bir gizli dizi (parola) gerektirir.

1. **Sertifika &amp; gizli** dizileri ' ne tıklayın

2. **Yeni istemci gizli dizisine** tıklayın.

    ![Azure portal. Hizmet Istemci parolası](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-SECRET.png)

3. Gizli dizi süresi belirtin.

4. Oluşturulduktan sonra, portalda yalnızca bir kez görüntülenir. Bunu bir yere iade edin ve güvenli bir şekilde depolayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Active Directory bir hizmet istemci uygulamasını nasıl kaydedeceğinizi öğrendiniz. Ardından, Azure 'da bir FHıR API 'SI dağıtın.
 
>[!div class="nextstepaction"]
>[Açık kaynaklı FHıR sunucusunu dağıtma](fhir-oss-powershell-quickstart.md)