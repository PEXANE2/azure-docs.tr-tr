---
title: Azure AD 'de kaynak uygulaması kaydetme-FHIR için Azure API
description: İstemci uygulamalarının kimlik doğrulaması sırasında kaynağa erişim isteyebilmesi için Azure Active Directory bir kaynak (veya API) uygulamayı kaydedin.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 10b951300b8386b057744a980abd5d847b6b6907
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2020
ms.locfileid: "84871858"
---
# <a name="register-a-resource-application-in-azure-active-directory"></a>Azure Active Directory bir kaynak uygulaması kaydetme

Bu makalede, Azure Active Directory bir kaynak (veya API) uygulamasını nasıl kaydedeceğinizi öğreneceksiniz. Kaynak uygulama, FHıR sunucu API 'sinin kendisinin Azure Active Directory bir gösterimidir ve istemci uygulamalar, kimlik doğrulanırken kaynağa erişim isteğinde bulunabilir. Kaynak uygulama, OAuth Parde *hedef kitle* olarak da bilinir.

## <a name="azure-api-for-fhir"></a>FHIR için Azure API'si

FHıR için Azure API kullanıyorsanız, hizmeti dağıtırken bir kaynak uygulaması otomatik olarak oluşturulur. Uygulamanızı dağıtmakta olduğunuz Azure Active Directory kiracısında FHIR için Azure API 'yi kullandığınız sürece, bu nasıl yapılır kılavuzuna atlayabilirsiniz ve bunun yerine, Azure API 'nizi FHIR için Azure API 'nizi kullanarak kullanmaya başlayın.

Farklı bir Azure Active Directory kiracı kullanıyorsanız (aboneliğiniz ile ilişkilendirilmemiş), FHıR kaynak uygulamasına yönelik Azure API 'sini PowerShell ile kiracınıza aktarabilirsiniz:

```azurepowershell-interactive
New-AzADServicePrincipal -ApplicationId 4f6778d8-5aef-43dc-a1ff-b073724b9495
```

isterseniz Azure CLı 'yi de kullanabilirsiniz:

```azurecli-interactive
az ad sp create --id 4f6778d8-5aef-43dc-a1ff-b073724b9495
```

## <a name="fhir-server-for-azure"></a>Azure için FHıR sunucusu

Azure için açık kaynaklı FHıR sunucusunu kullanıyorsanız, kaynak uygulamasını kaydetmek için aşağıdaki adımları izleyin.

### <a name="app-registrations-in-azure-portal"></a>Azure portal Uygulama kayıtları

1. [Azure portalda](https://portal.azure.com) sol taraftaki gezinti panelinden **Azure Active Directory**’ye tıklayın.

2. **Azure Active Directory** dikey penceresinde **uygulama kayıtları**' e tıklayın:

    ![Azure portal. Yeni uygulama kaydı.](media/how-to-aad/portal-aad-new-app-registration.png)

3. **Yeni kayda**tıklayın.

### <a name="add-a-new-application-registration"></a>Yeni bir uygulama kaydı ekleyin

Yeni uygulamanın ayrıntılarını girin. Görünen ad için belirli bir gereksinim yoktur, ancak FHıR sunucusunun URI 'sine ayarlanması, bulmayı kolaylaştırır:

![Yeni uygulama kaydı](media/how-to-aad/portal-aad-register-new-app-registration-NAME.png)

### <a name="set-identifier-uri-and-define-scopes"></a>Tanımlayıcı URI 'SI ayarlama ve kapsamları tanımlama

Kaynak uygulamanın, kaynağa erişim isteğinde bulunan istemcilerin kullanabileceği bir tanımlayıcı URI 'SI (uygulama KIMLIĞI URI 'SI) vardır. Bu değer, `aud` erişim belirtecinin talebini dolduracaktır. Bu URI 'yi FHıR sunucunuzun URI 'SI olacak şekilde ayarlamanız önerilir. FHıR uygulamalarında akıllı için, *hedef kitle* , fhır sunucusunun URI 'si olduğu varsayılır.

1. **API 'Yi kullanıma** sunma

2. *Uygulama kimliği URI 'sinin*yanındaki **Ayarla** ' ya tıklayın.

3. Tanımlayıcı URI 'sini girin ve **Kaydet**' e tıklayın. İyi bir tanımlayıcı URI 'SI, FHıR sunucunuzun URI 'SI olmalıdır.

4. **Kapsam Ekle** ' ye tıklayın ve API 'niz için tanımlamak istediğiniz kapsamları ekleyin. Gelecekte kaynak uygulamanıza izin vermek için en az bir kapsam eklemeniz gerekir. Eklemek istediğiniz belirli kapsamınız yoksa, user_impersonation kapsam olarak ekleyebilirsiniz.

![Hedef kitle ve kapsam](media/how-to-aad/portal-aad-register-new-app-registration-AUD-SCOPE.png)

### <a name="define-application-roles"></a>Uygulama rollerini tanımlama

FHIR için Azure API ve Azure için OSS FHıR sunucusu, rol tabanlı erişim denetimi için [Azure Active Directory Uygulama rollerini](https://docs.microsoft.com/azure/architecture/multitenant-identity/app-roles) kullanır. FHıR sunucu API 'niz için hangi rollerin kullanılabilir olduğunu tanımlamak için, kaynak uygulamanın [bildirimini](https://docs.microsoft.com/azure/active-directory/active-directory-application-manifest/)açın:

1. **Bildirim**' e tıklayın:

    ![Uygulama rolleri](media/how-to-aad/portal-aad-register-new-app-registration-APP-ROLES.png)

2. `appRoles`Özelliğinde, kullanıcılar veya uygulamalar istediğiniz rolleri ekleyin:

    ```json
    "appRoles": [
      {
        "allowedMemberTypes": [
          "User",
          "Application"
        ],
        "description": "FHIR Server Administrators",
        "displayName": "admin",
        "id": "1b4f816e-5eaf-48b9-8613-7923830595ad",
        "isEnabled": true,
        "value": "admin"
      },
      {
        "allowedMemberTypes": [
          "User"
        ],
        "description": "Users who can read",
        "displayName": "reader",
        "id": "c20e145e-5459-4a6c-a074-b942bbd4cfe1",
        "isEnabled": true,
        "value": "reader"
      }
    ],
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Active Directory bir kaynak uygulamasını nasıl kaydedeceğinizi öğrendiniz. Ardından, FHıR için Azure API 'sini dağıtın.
 
>[!div class="nextstepaction"]
>[FHIR için Azure API'sini dağıtma](fhir-paas-powershell-quickstart.md)