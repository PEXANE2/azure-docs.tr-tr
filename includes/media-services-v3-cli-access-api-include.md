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
ms.openlocfilehash: 58e9156091702718dccd75eb4a57e5b6d8c1f073
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76896347"
---
## <a name="access-the-media-services-api"></a>Media Services API’sine erişim

Azure AD hizmet sorumlusu kimlik doğrulamasını kullanarak Azure Media Services API’sine bağlanın. Aşağıdaki komut bir Azure AD uygulaması oluşturur ve hesaba bir hizmet sorumlusu ekler. Uygulamanızı yapılandırmak için, döndürülen değerleri kullanmalısınız.

Betiği çalıştırmadan önce, `amsaccount` ve `amsResourceGroup`, bu kaynakları oluştururken seçtiğiniz adlarla değiştirmelisiniz. `amsaccount`, hizmet sorumlusunu ekleyeceğiniz Azure Media Services hesabının adıdır.

Birden çok aboneliğe erişiminiz varsa, önce etkin aboneliği Media Services hesabının oluşturulduğu aboneliğe ayarlayın.

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
