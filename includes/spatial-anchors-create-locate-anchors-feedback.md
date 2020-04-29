---
ms.openlocfilehash: 7b9954930c668e338cc289e948a5591c09ec2654
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76694552"
---
## <a name="provide-feedback-to-the-user"></a>Kullanıcıya geri bildirim sağlama

Oturum güncelleştirilmiş olayını işlemek için kod yazabilirsiniz. Bu olay, oturum, her ne kadar her ne kadar gelişdiğinde bu olay ateşlenir. Bunu yaptığınızda şunları yapmanıza izin verir:

- Cihaz taşındıkça ve oturum ortamının öğrendiğini öğrendiğinde kullanıcıya geri bildirim sağlamak için `UserFeedback` sınıfını kullanın. Bunu yapmak için
- Uzamsal Tutturucuların oluşturulması için yeterince izlenen uzamsal veriler olduğunu belirleme. Bunu ya `ReadyForCreateProgress` `RecommendedForCreateProgress`da ile belirlersiniz. 1 `ReadyForCreateProgress` ' den sonra bir bulut uzamsal bağlayıcısını kaydetmek için yeterli veri sunuyoruz, ancak bunu yapmak için 1 ' `RecommendedForCreateProgress` in üzerinde olmasını beklemeniz önerilir.
