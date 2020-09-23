---
title: dosya dahil etme
description: dosya dahil etme
services: azure-communication-services
author: matthewrobertson
manager: nimag
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 08/20/2020
ms.topic: include
ms.custom: include file
ms.author: marobert
ms.openlocfilehash: c0ba8e76e069bf9dc1c96aecdc670699c32b3c96
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948159"
---
## <a name="object-model"></a>Nesne modeli

Kullanıcı erişimi belirteçleri, uygulamanızın kullanıcılarına doğrudan Azure Iletişim Hizmetleri kaynağına bağlanmasını sağlar. Bu belirteçleri sunucunuzda oluşturun, istemci uygulamanıza geri geçirin ve ardından Iletişim Hizmetleri istemci kitaplıklarını başlatmak için bunları kullanın. Bunu başarmak için, kullanıcılarınızın kimliğini doğrulayabilecek ve Kullanıcı erişim belirteçleri veren bir güvenilen hizmet uç noktası oluşturmanız gerekir. Bu hizmet, uygulamanızın kullanıcı kimlikleri ve Azure Iletişim Hizmetleri kullanıcıları arasında kalıcı bir eşleme sürdürmelidir.

Communicationıdentityclient sınıfı, tüm Kullanıcı ve erişim belirtecinin ilgili işlevlerini sağlar. Bunu bir bağlantı dizesiyle birlikte başlatır ve ardından kullanıcıları yönetmek ve erişim belirteçleri vermek için kullanın.

> [!WARNING]
> Belirteçleri, bir kullanıcının kaynaklarına erişim izni vermediği için hassas verilerden oluşur. Bu nedenle, belirteçlerin tehlikeye düşmeden korunması önemlidir. Yalnızca uygulamanızın yetkili kullanıcılarına erişim belirteçleri veren bir güvenilen hizmet uç noktası oluşturmanız gerekir. Kullanıcı erişim belirteçlerini bir yedekleme deposuna önbellekliyorsanız, şifrelemeyi kullanmanız önemle önerilir.

### <a name="access-token-scopes"></a>Erişim belirteci kapsamları

Kapsamlar, bir Kullanıcı erişim belirtecinin yetkilendiribileceği Azure Iletişim Hizmetleri işlevselliğinin tam olarak belirtilmesine izin verir. Kapsamlar, bireysel kullanıcı erişim belirteçlerine uygulanır. Azure Iletişim Hizmetleri, Kullanıcı erişimi belirteçleri için aşağıdaki kapsamları destekler:

| Ad   | Açıklama                                                                         |
| ------ | ----------------------------------------------------------------------------------- |
| `Chat` | Bir sohbete katılma olanağı verir                                         |
| `VoIP` | Çağıran istemci kitaplığını kullanarak VOıP çağrıları yapma ve alma özelliği verir * |
| `Pstn` | Çağıran istemci kitaplığını kullanarak bir PSTN çağrısı yapma olanağı verir *           |

\* Özellik henüz desteklenmiyor ve yakında kullanıma sunulacaktır

Bir kullanıcının belirli bir işlevselliğe erişme yeteneğini kaldırmak istiyorsanız, önce, daha sınırlı bir kapsam kümesiyle yeni bir belirteç vermeden önce, istenmeyen kapsamları içerebilen [mevcut erişim belirteçlerini iptal](#revoke-user-access-tokens) etmelisiniz.
