---
author: blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 01/09/2020
ms.author: larryfr
ms.openlocfilehash: 8fd774f8a3a73ceaffa7902b35e1b1dff12ef5af
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "75893966"
---
Bir Azure Machine Learning çalışma alanı veya çalışma alanı tarafından kullanılan bir kaynak oluştururken, aşağıdaki iletilere benzer bir hata alabilirsiniz:

* `No registered resource provider found for location {location}`
* `The subscription is not registered to use namespace {resource-provider-namespace}`

Çoğu kaynak sağlayıcısı otomatik olarak kaydedilir, ancak hepsi değildir. Bu iletiyi alırsanız, belirtilen sağlayıcıyı kaydetmeniz gerekir.

Kaynak sağlayıcılarını kaydetme hakkında bilgi için bkz. [kaynak sağlayıcısı kaydı hatalarını giderme](../articles/azure-resource-manager/templates/error-register-resource-provider.md).