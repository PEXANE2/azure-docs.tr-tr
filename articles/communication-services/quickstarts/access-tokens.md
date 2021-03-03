---
title: Hızlı başlangıç-erişim belirteçleri oluşturma ve yönetme
titleSuffix: An Azure Communication Services quickstart
description: Azure Iletişim Hizmetleri kimlik istemci kitaplığını kullanarak kimlikleri ve erişim belirteçlerini yönetme hakkında bilgi edinin.
author: tomaschladek
manager: nmurav
services: azure-communication-services
ms.author: tchladek
ms.date: 08/20/2020
ms.topic: quickstart
ms.service: azure-communication-services
zone_pivot_groups: acs-js-csharp-java-python
ms.openlocfilehash: 8937210b366981d9768e5329989a8ad14f5e8f35
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101657102"
---
# <a name="quickstart-create-and-manage-access-tokens"></a>Hızlı başlangıç: erişim belirteçleri oluşturma ve yönetme

Iletişim Hizmetleri kimlik istemci kitaplığını kullanarak Azure Iletişim Hizmetleri ile çalışmaya başlayın. Kimlik oluşturmanızı ve erişim belirteçlerinizi yönetmenizi sağlar. Kimlik, Azure Iletişim hizmetindeki (örneğin, Kullanıcı veya cihaz) uygulamanızın varlığını temsil eder. Erişim belirteçleri, sohbet ve arama istemci kitaplıklarının doğrudan Azure Iletişim hizmetlerine karşı kimlik doğrulamasını sağlar. Sunucu tarafı hizmetinde erişim belirteçleri oluşturmanızı öneririz. Daha sonra erişim belirteçleri istemci cihazlarındaki Iletişim Hizmetleri istemci kitaplıklarını başlatmak için kullanılır.

Bu öğretici boyunca görüntülerde görülen tüm fiyatlar yalnızca tanıtım amaçlıdır.

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

Created an identity: 8:acs:4ccc92c8-9815-4422-bddc-ceea181dc774_00000006-19e0-2727-80f5-8b3a0d003502

Issued an access token with 'voip' scope that expires at Fri Nov 27 2020 16:47:05 GMT-0800 (Pacific Standard Time):
<token signature here>

Successfully revoked all access tokens for identity with ID: 8:acs:4ccc92c8-9815-4422-bddc-ceea181dc774_00000006-19e0-2727-80f5-8b3a0d003502

Deleted the identity with ID: 8:acs:4ccc92c8-9815-4422-bddc-ceea181dc774_00000006-19e0-2727-80f5-8b3a0d003502
```
<!---cSpell:enable --->

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir Iletişim Hizmetleri aboneliğini temizleyip kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler. [Kaynakları Temizleme](./create-communication-resource.md#clean-up-resources)hakkında daha fazla bilgi edinin.


## <a name="next-steps"></a>Sonraki Adımlar

Bu hızlı başlangıçta şunları öğrendiniz:

> [!div class="checklist"]
> * Kimlikleri yönetme
> * Erişim belirteçleri verme
> * Iletişim Hizmetleri kimlik istemci kitaplığını kullanma


> [!div class="nextstepaction"]
> [Uygulamanıza sesli arama ekleme](./voice-video-calling/getting-started-with-calling.md)

Ayrıca şunları yapmak isteyebilirsiniz:

 - [Kimlik doğrulaması hakkında bilgi edinin](../concepts/authentication.md)
 - [Uygulamanıza sohbet ekleme](./chat/get-started.md)
 - [İstemci ve sunucu mimarisi hakkında bilgi edinin](../concepts/client-and-server-architecture.md)
