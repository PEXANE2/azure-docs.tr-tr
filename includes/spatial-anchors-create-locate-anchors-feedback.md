---
ms.openlocfilehash: 7b9954930c668e338cc289e948a5591c09ec2654
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "76694552"
---
## <a name="provide-feedback-to-the-user"></a>Kullanıcıya geri bildirim sağlama

Oturum güncelleştirilmiş olayını işlemek için kod yazabilirsiniz. Bu olay, oturum, her ne kadar her ne kadar gelişdiğinde bu olay ateşlenir. Bunu yaptığınızda şunları yapmanıza izin verir:

- `UserFeedback`Cihaz taşındıkça ve oturum ortamının öğrendiğini öğrendiğinde kullanıcıya geri bildirim sağlamak için sınıfını kullanın. Bunu yapmak için
- Uzamsal Tutturucuların oluşturulması için yeterince izlenen uzamsal veriler olduğunu belirleme. Bunu ya da ile belirlersiniz `ReadyForCreateProgress` `RecommendedForCreateProgress` . `ReadyForCreateProgress`1 ' den sonra bir bulut uzamsal bağlayıcısını kaydetmek için yeterli veri sunuyoruz, ancak `RecommendedForCreateProgress` bunu yapmak için 1 ' in üzerinde olmasını beklemeniz önerilir.
