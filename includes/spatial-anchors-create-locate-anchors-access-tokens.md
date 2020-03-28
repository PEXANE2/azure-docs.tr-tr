---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 02/21/2019
ms.author: rgarcia
ms.openlocfilehash: 63725d55e2b2935ec6a899789249259b096865c3
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "67188066"
---
### <a name="access-tokens"></a>Erişim Belirteçleri

Erişim Belirteçleri, Azure Uzamsal Bağlantı Larını doğrulamak için daha sağlam bir yöntemdir. Özellikle de uygulamanızı bir üretim dağıtımına hazırlarken. Bu yaklaşımın özeti, istemci uygulamanızın güvenli bir şekilde kimlik doğrulaması yapabileceğiniz bir arka uç hizmeti ayarlamaktır. Arka uç hizmetiniz çalışma zamanında AAD ile ve Bir Erişim Jetonu istemek için Azure Uzamsal Çapalar Güvenli Belirteç Hizmeti ile aralar. Bu belirteç daha sonra istemci uygulamasına teslim edilir ve Azure Uzamsal Çapalar ile kimlik doğrulaması için SDK'da kullanılır.
