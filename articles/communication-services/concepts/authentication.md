---
title: Azure Iletişim hizmetlerinde kimlik doğrulama
titleSuffix: An Azure Communication Services concept document
description: Bir uygulamanın veya hizmetin Iletişim hizmetlerinde kimlik doğrulaması yapabileceği çeşitli yollar hakkında bilgi edinin.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2021
ms.topic: conceptual
ms.service: azure-communication-services
ms.openlocfilehash: b3cd0643a74ccadb8390ce906eb391420de15a29
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2021
ms.locfileid: "103490798"
---
# <a name="authenticate-to-azure-communication-services"></a>Azure Iletişim hizmetlerinde kimlik doğrulama

Azure Iletişim Hizmetleri ile her istemci etkileşiminin kimliğinin doğrulanması gerekir. Tipik bir mimaride, Kullanıcı ve sorun belirteçleri oluşturmak için, güvenilir Kullanıcı erişimi hizmetinde [istemci ve sunucu mimarisi](./client-and-server-architecture.md), *erişim anahtarları* veya *yönetilen kimlik* ' e bakın. Ve Güvenilen Kullanıcı erişimi hizmeti tarafından verilen *Kullanıcı erişim belirteci* , istemci uygulamalarının diğer iletişim hizmetlerine erişmesi için kullanılır, örneğin sohbet veya arama hizmeti.

Azure Iletişim Hizmetleri SMS hizmeti, kimlik doğrulaması için *erişim anahtarlarını* veya *yönetilen kimliği* de kabul eder. Bu genellikle güvenilen bir hizmet ortamında çalışan bir hizmet uygulamasında gerçekleşir.

Her yetkilendirme seçeneği kısaca aşağıda açıklanmıştır:

- SMS ve kimlik işlemlerine yönelik anahtar kimlik doğrulamasına **erişin** . Erişim anahtarı kimlik doğrulaması, güvenilir bir hizmet ortamında çalışan hizmet uygulamaları için uygundur. Erişim anahtarı, Azure Iletişim Hizmetleri portalında bulunabilir. Bir hizmet uygulaması, erişim anahtarı ile kimlik doğrulamak için, ilgili SMS veya kimlik istemci kitaplıklarını başlatmak üzere erişim anahtarını kimlik bilgisi olarak kullanır, bkz. [erişim belirteçleri oluşturma ve yönetme](../quickstarts/access-tokens.md). Erişim anahtarı, kaynağınızın bağlantı dizesinin bir parçası olduğundan, bkz. [Iletişim Hizmetleri kaynaklarını oluşturma ve yönetme](../quickstarts/create-communication-resource.md), bağlantı dizesiyle kimlik doğrulama, erişim anahtarı ile kimlik doğrulamaya eşdeğerdir.
- SMS ve kimlik işlemleri için **yönetilen kimlik** kimlik doğrulaması. Yönetilen kimlik, bkz. [yönetilen kimlik](../quickstarts/managed-identity.md), güvenilir bir hizmet ortamında çalışan hizmet uygulamaları için uygundur. Bir hizmet uygulaması yönetilen bir kimlikle kimlik doğrulaması yapmak için, yönetilen kimliğin KIMLIĞI ve gizli anahtarı ile bir kimlik bilgisi oluşturur, ardından karşılık gelen SMS veya Identity istemci kitaplıklarını başlatır, bkz. [erişim belirteçleri oluşturma ve yönetme](../quickstarts/access-tokens.md).
- Sohbet için **Kullanıcı erişim belirteci** kimlik doğrulaması ve çağırma. Kullanıcı erişimi belirteçleri, istemci uygulamalarınızın Azure Iletişim sohbeti ve çağrı Hizmetleri ile kimlik doğrulamasını sağlar. Bu belirteçler, oluşturduğunuz bir "Güvenilen Kullanıcı erişimi hizmeti" içinde oluşturulur. Daha sonra, sohbeti başlatmak ve istemci kitaplıklarını çağırmak için belirteci kullanan istemci cihazlara sağlanırlar. Daha fazla bilgi için bkz. [uygulamanıza sohbet ekleme](../quickstarts/chat/get-started.md) örneğin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Iletişim Hizmetleri kaynaklarını](../quickstarts/create-communication-resource.md) 
>  oluşturma ve yönetme [Azure CLI](../quickstarts/managed-identity-from-cli.md) 
>  'dan Azure Active Directory yönetilen bir kimlik uygulaması oluşturma [Kullanıcı erişim belirteçleri oluşturma](../quickstarts/access-tokens.md)

Daha fazla bilgi için aşağıdaki makaleleri inceleyin:
- [İstemci ve sunucu mimarisi hakkında bilgi edinin](../concepts/client-and-server-architecture.md)
