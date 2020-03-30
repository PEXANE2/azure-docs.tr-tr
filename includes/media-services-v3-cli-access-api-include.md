---
title: include dosyası
description: include dosyası
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 05/01/2019
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: 9d8de8826fc3dfcc2360eb3b6c82b3ff8d65d845
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79461240"
---
### <a name="access-the-media-services-api"></a>Media Services API’sine erişim

Azure AD hizmet sorumlusu kimlik doğrulamasını kullanarak Azure Media Services API’sine bağlanın. Aşağıdaki komut bir Azure AD uygulaması oluşturur ve hesaba bir hizmet sorumlusu ekler. Uygulamanızı yapılandırmak için, döndürülen değerleri kullanmalısınız.

Komut dosyasını çalıştırmadan önce, bu kaynakları oluştururken seçtiğiniz adlarla `amsaccount` ve `amsResourceGroup` değiştirmelisiniz. `amsaccount`, hizmet sorumlusunu ekleyeceğiniz Azure Media Services hesabının adıdır.

Birden çok aboneliğe erişiminiz varsa, ilk olarak etkin aboneliği Medya Hizmetleri hesabının oluşturulduğu aboneliğe ayarlayın.

```azurecli
az account set --subscription subscriptionId
```

Aşağıdaki komut bir `json` çıkışı döndürür:

```azurecli
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup
```

Bu komut, aşağıdakine benzer bir yanıt oluşturur:

```json
{
  "AadClientId": "00000000-0000-0000-0000-000000000000",
  "AadEndpoint": "https://login.microsoftonline.com",
  "AadSecret": "00000000-0000-0000-0000-000000000000",
  "AadTenantId": "00000000-0000-0000-0000-000000000000",
  "AccountName": "amsaccount",
  "ArmAadAudience": "https://management.core.windows.net/",
  "ArmEndpoint": "https://management.azure.com/",
  "Region": "West US 2",
  "ResourceGroup": "amsResourceGroup",
  "SubscriptionId": "00000000-0000-0000-0000-000000000000"
}
```

Yanıtta bir `xml` almak istiyorsanız, aşağıdaki komutu kullanın:

```azurecli
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup --xml
```
