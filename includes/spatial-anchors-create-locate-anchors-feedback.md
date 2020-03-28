---
ms.openlocfilehash: 7b9954930c668e338cc289e948a5591c09ec2654
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76694552"
---
## <a name="provide-feedback-to-the-user"></a>Kullanıcıya geri bildirim sağlama

Oturum güncelleştirilmiş olayı işlemek için kod yazabilirsiniz. Bu olay, oturum çevrenizdeki anlayışını her geliştirtiğinde ateşler. Bunu yapmak, şunları yapmanızı sağlar:

- Aygıt `UserFeedback` hareket ettikçe ve oturum ortam anlayışını güncellerken kullanıcıya geri bildirim sağlamak için sınıfı kullanın. Bunu yapmak için,
- Hangi noktada uzamsal çapalar oluşturmak için yeterli izlenmiş uzamsal veri olduğunu belirleyin. Bunu ya da `ReadyForCreateProgress` `RecommendedForCreateProgress`. 1'in üzerinde `ReadyForCreateProgress` olduğunda, bir bulut uzamsal çapasını kaydetmek `RecommendedForCreateProgress` için yeterli veriye sahibiz, ancak bunu yapmak için 1'in üzerine çıkmasını beklemenizi öneririz.
