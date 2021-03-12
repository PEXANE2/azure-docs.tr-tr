---
title: Azure CLı kullanarak erişim belirteci edinme-FHIR için Azure API
description: Bu makalede, Azure CLı kullanarak FHıR için Azure API için bir erişim belirteci alma açıklanmaktadır.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: matjazl
ms.openlocfilehash: b7bdb7f4a22d080f382fea984e710980428067ff
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2021
ms.locfileid: "103020307"
---
# <a name="get-access-token-for-azure-api-for-fhir-using-azure-cli"></a>Azure CLı kullanarak FHıR için Azure API için erişim belirteci alın

Bu makalede, Azure CLı kullanarak FHıR için Azure API 'SI için bir erişim belirteci elde etme hakkında bilgi edineceksiniz. [FHıR Için Azure API 'si sağladığınızda](fhir-paas-portal-quickstart.md), hizmete erişimi olan bir kullanıcı veya hizmet sorumlusu kümesi yapılandırırsınız. Kullanıcı nesne KIMLIĞINIZ izin verilen nesne kimlikleri listesinde ise, Azure CLı kullanılarak alınan bir belirteci kullanarak hizmete erişebilirsiniz.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

## <a name="obtain-a-token"></a>Belirteç alma

FHıR için Azure API 'si, `resource` `Audience` fhır sunucusunun URI 'sine eşit bir veya daha fazla kullanır `https://<FHIR ACCOUNT NAME>.azurehealthcareapis.com` . Aşağıdaki komutla bir belirteç edinebilir ve bir değişkende (adında `$token` ) saklayabilirsiniz:

```azurecli-interactive
token=$(az account get-access-token --resource=https://<FHIR ACCOUNT NAME>.azurehealthcareapis.com --query accessToken --output tsv)
```

## <a name="use-with-azure-api-for-fhir"></a>FHıR için Azure API ile kullanma

```azurecli-interactive
curl -X GET --header "Authorization: Bearer $token" https://<FHIR ACCOUNT NAME>.azurehealthcareapis.com/Patient
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure CLı kullanarak FHıR için Azure API 'SI için bir erişim belirteci edinmeyi öğrendiniz. Postman kullanarak FHıR API 'sine nasıl erişebileceğinizi öğrenmek için Postman öğreticisine geçin.

>[!div class="nextstepaction"]
>[Postman kullanarak FHıR API 'sine erişme](access-fhir-postman-tutorial.md)
