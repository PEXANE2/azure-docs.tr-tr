---
author: msftradford
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 11/20/2020
ms.author: parkerra
ms.openlocfilehash: c9b4e0fdf95c2bb4888a3a11820f1785d8545bd4
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95993053"
---
### <a name="access-tokens"></a>Erişim Belirteçleri

Erişim belirteçleri Azure uzamsal bağlayıcılarla kimlik doğrulamaya yönelik daha sağlam bir yöntemdir. Özellikle de Uygulamanızı bir üretim dağıtımı için hazırlarsınız. Bu yaklaşımın Özeti, istemci uygulamanızın ile güvenli bir şekilde kimlik doğrulaması yapabilmesi için bir arka uç hizmeti ayarlamanıza olanak sağlar. Arka uç hizmetiniz, çalışma zamanında AAD ile ve Azure uzamsal bağlayıcı güvenli belirteç hizmeti ile bir erişim belirteci istemek için arabirimler sağlar. Bu belirteç daha sonra istemci uygulamasına dağıtılır ve Azure uzamsal bağlayıcılarla kimlik doğrulaması yapmak için SDK 'da kullanılır.
