---
ms.openlocfilehash: 7b9954930c668e338cc289e948a5591c09ec2654
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "76694552"
---
## <a name="provide-feedback-to-the-user"></a>Kullanıcıya geri bildirim sağlama

Oturum güncelleştirilmiş olayını işlemek için kod yazabilirsiniz. Bu olay, oturum, her ne kadar her ne kadar gelişdiğinde bu olay ateşlenir. Bunu yaptığınızda şunları yapmanıza izin verir:

- `UserFeedback`Cihaz taşındıkça ve oturum ortamının öğrendiğini öğrendiğinde kullanıcıya geri bildirim sağlamak için sınıfını kullanın. Bunu yapmak için
- Uzamsal Tutturucuların oluşturulması için yeterince izlenen uzamsal veriler olduğunu belirleme. Bunu ya da ile belirlersiniz `ReadyForCreateProgress` `RecommendedForCreateProgress` . `ReadyForCreateProgress`1 ' den sonra bir bulut uzamsal bağlayıcısını kaydetmek için yeterli veri sunuyoruz, ancak `RecommendedForCreateProgress` bunu yapmak için 1 ' in üzerinde olmasını beklemeniz önerilir.
