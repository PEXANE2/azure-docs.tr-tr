---
title: Kimlik doğrulama için kimlik nesne kimliklerini bulma-FHıR için Azure API
description: Bu makalede, FHıR için Azure API kimlik doğrulamasını yapılandırmak üzere gereken kimlik nesnesi kimliklerini bulma açıklanmaktadır
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 88c0349ce220229920a39ba4cb6640f3d4dde93b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84871941"
---
# <a name="find-identity-object-ids-for-authentication-configuration"></a>Kimlik doğrulama yapılandırması için kimlik nesne kimliklerini bul

Bu makalede, FHıR için Azure API 'sini veri düzlemi için [dış veya ikincil Active Directory kiracı kullanacak](configure-local-rbac.md) şekilde yapılandırırken gereken kimlik nesnesi kimliklerini bulmayı öğreneceksiniz.

## <a name="find-user-object-id"></a>Kullanıcı nesne KIMLIĞINI bul

Kullanıcı adına sahip bir kullanıcınız varsa `myuser@consoso.com` , `ObjectId` aşağıdaki PowerShell komutunu kullanarak kullanıcıları bulabilirsiniz:

```azurepowershell-interactive
$(Get-AzureADUser -Filter "UserPrincipalName eq 'myuser@consoso.com'").ObjectId
```

isterseniz Azure CLı 'yi de kullanabilirsiniz:

```azurecli-interactive
az ad user show --upn-or-object-id myuser@consoso.com | jq -r .objectId
```

## <a name="find-service-principal-object-id"></a>Hizmet sorumlusu nesne KIMLIĞINI bul

Bir [hizmet istemci uygulaması](register-service-azure-ad-client-app.md) kaydetmiş olduğunuzu ve bu hizmet istemcisinin fhır IÇIN Azure API 'sine erişmesine izin vermek istiyorsanız, istemci hizmet SORUMLUSUNUN nesne kimliğini aşağıdaki PowerShell komutuyla bulabilirsiniz:

```azurepowershell-interactive
$(Get-AzureADServicePrincipal -Filter "AppId eq 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX'").ObjectId
```

`XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX`hizmet istemci uygulama kimliği nerede. Alternatif olarak, hizmet istemcisinin ' de kullanabilirsiniz `DisplayName` :

```azurepowershell-interactive
$(Get-AzureADServicePrincipal -Filter "DisplayName eq 'testapp'").ObjectId
```

Azure CLı kullanıyorsanız şunları kullanabilirsiniz:

```azurecli-interactive
az ad sp show --id XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX | jq -r .objectId
```

## <a name="find-a-security-group-object-id"></a>Güvenlik grubu nesne KIMLIĞI bul

Bir güvenlik grubunun nesne KIMLIĞINI bulmak isterseniz, aşağıdaki PowerShell komutunu kullanabilirsiniz:

```azurepowershell-interactive
$(Get-AzureADGroup -Filter "DisplayName eq 'mygroup'").ObjectId
```
Burada `mygroup` ilgilendiğiniz grubun adıdır.

Azure CLı kullanıyorsanız şunları kullanabilirsiniz:

```azurecli-interactive
az ad group show --group "mygroup" | jq -r .objectId
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, FHıR için Azure API 'yi bir dış veya ikincil Azure Active Directory kiracı kullanacak şekilde yapılandırmak için gereken kimlik nesnesi kimliklerini bulmayı öğrendiniz. Daha sonra yerel RBAC ayarlarını yapılandırmak için nesne kimliklerini kullanma hakkında bilgi edinin:
 
>[!div class="nextstepaction"]
>[Yerel RBAC ayarlarını yapılandırma](configure-local-rbac.md)