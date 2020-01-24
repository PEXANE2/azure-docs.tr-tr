---
ms.openlocfilehash: 7b9954930c668e338cc289e948a5591c09ec2654
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76694552"
---
## <a name="provide-feedback-to-the-user"></a>Kullanıcıya geri bildirim sağlama

Oturum güncelleştirilmiş olayını işlemek için kod yazabilirsiniz. Bu olay, oturum, her ne kadar her ne kadar gelişdiğinde bu olay ateşlenir. Bunu yaptığınızda şunları yapmanıza izin verir:

- Cihaz taşındıkça ve oturum ortamının öğrendiğini öğrendiğinde kullanıcıya geri bildirim sağlamak için `UserFeedback` sınıfını kullanın. Bunu yapmak için
- Uzamsal Tutturucuların oluşturulması için yeterince izlenen uzamsal veriler olduğunu belirleme. Bunu `ReadyForCreateProgress` ya da `RecommendedForCreateProgress`ile belirlersiniz. `ReadyForCreateProgress` 1 ' den sonra, bir bulut uzamsal bağlayıcısını kaydetmek için yeterli veri sunuyoruz, ancak `RecommendedForCreateProgress` 1 ' in üzerine gelene kadar beklemeniz önerilir.
