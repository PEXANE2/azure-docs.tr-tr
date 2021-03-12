---
title: Azure AD 'de kaynak uygulaması kaydetme-FHIR için Azure API
description: İstemci uygulamalarının kimlik doğrulaması sırasında kaynağa erişim isteyebilmesi için Azure Active Directory bir kaynak (veya API) uygulamayı kaydedin.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: matjazl
ms.openlocfilehash: 8d70a7b44893ba9c9a0cc2d1d01c65e8e1584e0f
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2021
ms.locfileid: "103020090"
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

Azure için açık kaynaklı FHıR sunucusunu kullanıyorsanız, bir kaynak uygulamasını kaydetmek için [GitHub](https://github.com/microsoft/fhir-server/blob/master/docs/Register-Resource-Application.md) deposunda bulunan adımları izleyin. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Active Directory bir kaynak uygulamasını nasıl kaydedeceğinizi öğrendiniz. Sonra, gizli istemci uygulamanızı kaydedin.
 
>[!div class="nextstepaction"]
>[Gizli Istemci uygulamasını Kaydet](register-confidential-azure-ad-client-app.md)