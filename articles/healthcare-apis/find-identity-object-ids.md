---
title: Kimlik doğrulama için kimlik nesne kimliklerini bulma-FHıR için Azure API
description: Bu makalede, FHıR için Azure API kimlik doğrulamasını yapılandırmak üzere gereken kimlik nesnesi kimliklerini bulma açıklanmaktadır
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: matjazl
ms.openlocfilehash: 5b42d61d59a3c816c3b664297470cfbf91f17439
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87851775"
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
az ad user show --id myuser@consoso.com --query objectId --out tsv
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
az ad sp show --id XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX --query objectId --out tsv
```

## <a name="find-a-security-group-object-id"></a>Güvenlik grubu nesne KIMLIĞI bul

Bir güvenlik grubunun nesne KIMLIĞINI bulmak isterseniz, aşağıdaki PowerShell komutunu kullanabilirsiniz:

```azurepowershell-interactive
$(Get-AzureADGroup -Filter "DisplayName eq 'mygroup'").ObjectId
```
Burada `mygroup` ilgilendiğiniz grubun adıdır.

Azure CLı kullanıyorsanız şunları kullanabilirsiniz:

```azurecli-interactive
az ad group show --group "mygroup" --query objectId --out tsv
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, FHıR için Azure API 'yi bir dış veya ikincil Azure Active Directory kiracı kullanacak şekilde yapılandırmak için gereken kimlik nesnesi kimliklerini bulmayı öğrendiniz. Daha sonra yerel RBAC ayarlarını yapılandırmak için nesne kimliklerini kullanma hakkında bilgi edinin:
 
>[!div class="nextstepaction"]
>[Yerel RBAC ayarlarını yapılandırma](configure-local-rbac.md)
