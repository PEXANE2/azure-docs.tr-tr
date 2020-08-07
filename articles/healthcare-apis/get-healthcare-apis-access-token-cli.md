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
ms.openlocfilehash: 7528f9d4e3b3043af1e4790c063eb6ddc6d9a828
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87849055"
---
# <a name="get-access-token-for-azure-api-for-fhir-using-azure-cli"></a>Azure CLı kullanarak FHıR için Azure API için erişim belirteci alın

Bu makalede, Azure CLı kullanarak FHıR için Azure API 'SI için bir erişim belirteci elde etme hakkında bilgi edineceksiniz. [FHıR Için Azure API 'si sağladığınızda](fhir-paas-portal-quickstart.md), hizmete erişimi olan bir kullanıcı veya hizmet sorumlusu kümesi yapılandırırsınız. Kullanıcı nesne KIMLIĞINIZ izin verilen nesne kimlikleri listesinde ise, Azure CLı kullanılarak alınan bir belirteci kullanarak hizmete erişebilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="sign-in-with-azure-cli"></a>Azure CLI'de oturum açma

Bir belirteç elde etmeden önce, belirteç almak istediğiniz kullanıcıyla oturum açmanız gerekir:

```azurecli-interactive
az login
```

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
