---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 02/21/2019
ms.author: rgarcia
ms.openlocfilehash: 63725d55e2b2935ec6a899789249259b096865c3
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "67188066"
---
### <a name="access-tokens"></a>Erişim belirteçleri

Erişim belirteçleri Azure uzamsal bağlayıcılarla kimlik doğrulamaya yönelik daha sağlam bir yöntemdir. Özellikle de Uygulamanızı bir üretim dağıtımı için hazırlarsınız. Bu yaklaşımın Özeti, istemci uygulamanızın ile güvenli bir şekilde kimlik doğrulaması yapabilmesi için bir arka uç hizmeti ayarlamanıza olanak sağlar. Arka uç hizmetiniz, çalışma zamanında AAD ile ve Azure uzamsal bağlayıcı güvenli belirteç hizmeti ile bir erişim belirteci istemek için arabirimler sağlar. Bu belirteç daha sonra istemci uygulamasına dağıtılır ve Azure uzamsal bağlayıcılarla kimlik doğrulaması yapmak için SDK 'da kullanılır.
