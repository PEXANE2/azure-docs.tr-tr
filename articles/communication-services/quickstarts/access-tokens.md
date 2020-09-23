---
title: Hızlı başlangıç-erişim belirteçleri oluşturma ve yönetme
titleSuffix: An Azure Communication Services quickstart
description: Azure Iletişim Hizmetleri yönetim istemci kitaplığı 'nı kullanarak kimlikleri ve erişim belirteçlerini yönetme hakkında bilgi edinin.
author: matthewrobertson
manager: jken
services: azure-communication-services
ms.author: marobert
ms.date: 08/20/2020
ms.topic: quickstart
ms.service: azure-communication-services
zone_pivot_groups: acs-js-csharp-java-python
ms.openlocfilehash: a76000ecacdf78196ec1b80a60940484f6421641
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948150"
---
# <a name="quickstart-create-and-manage-access-tokens"></a>Hızlı başlangıç: erişim belirteçleri oluşturma ve yönetme

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

Erişim belirteçlerinizi sağlamak ve yönetmek için Iletişim Hizmetleri yönetim istemci kitaplığını kullanarak Azure Iletişim Hizmetleri ile çalışmaya başlayın. Erişim belirteçleri, sohbet ve arama istemci kitaplıklarının doğrudan Azure Iletişim hizmetlerine karşı kimlik doğrulamasını sağlar. Bu belirteçler, uyguladığınız sunucu tarafı belirteç sağlama hizmetinde oluşturulur. Bunlar daha sonra istemci cihazlarındaki Iletişim Hizmetleri istemci kitaplıklarını başlatmak için kullanılır.

Bu öğretici boyunca görüntülerde görülen tüm fiyatlara yalnızca örnek amaçlıdır.

::: zone pivot="programming-language-csharp"
[!INCLUDE [.NET](./includes/user-access-token-net.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [JavaScript](./includes/user-access-token-js.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Python](./includes/user-access-token-python.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Java](./includes/user-access-token-java.md)]
::: zone-end

Uygulamanın çıktısı tamamlanan her eylemi açıklar:
<!---cSpell:disable --->
```console
Azure Communication Services - Access Tokens Quickstart

Issued a access token with 'voip' scope for identity with ID: 8:acs:fecfaddf-bf2c-4a0e-b52f-7d918c9536e6_65012b-1400da9050:
<token signature here>

Issued a access token with 'chat' scope for identity with ID: 8:acs:fecfaddf-bf2c-4a0e-b52f-7d918c9536e6_65012b-1400da9050:
<token signature here>

Successfully deleted the identity with ID: 8:acs:fecfaddf-bf2c-4a0e-b52f-7d918c9536e6_65012b-1400da9050

Deleted the identity with ID: 8:acs:fecfaddf-bf2c-4a0e-b52f-7d918c9536e6_65012b-1400da9050
```
<!---cSpell:enable --->

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir Iletişim Hizmetleri aboneliğini temizleyip kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler. [Kaynakları Temizleme](./create-communication-resource.md#clean-up-resources)hakkında daha fazla bilgi edinin.


## <a name="next-steps"></a>Sonraki Adımlar

Bu hızlı başlangıçta şunları öğrendiniz:

> [!div class="checklist"]
> * Kimlikleri yönetme
> * Erişim belirteçleri verme
> * Iletişim Hizmetleri Yönetimi istemci kitaplığını kullanma


> [!div class="nextstepaction"]
> [Uygulamanıza sesli çağrı ekleme](./voice-video-calling/getting-started-with-calling.md)

Ayrıca şunları yapmak isteyebilirsiniz:

 - [Kimlik doğrulaması hakkında bilgi edinin](../concepts/authentication.md)
 - [Uygulamanıza sohbet ekleyin](./chat/get-started.md)
 - [İstemci ve sunucu mimarisi hakkında bilgi edinin](../concepts/client-and-server-architecture.md)
