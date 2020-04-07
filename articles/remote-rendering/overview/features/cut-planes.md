---
title: Uçakları kes
description: Kesilen uçakların ne olduğunu ve nasıl kullanılacağını açıklar
author: jakrams
ms.author: jakras
ms.date: 02/06/2020
ms.topic: article
ms.openlocfilehash: 8075d9cd4530bafb12a338830baf0fe22eb03bce
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681031"
---
# <a name="cut-planes"></a>Uçakları kes

*Kesilmiş düzlem,* sanal düzlemin bir tarafındaki pikselleri kesen ve [meshes'in](../../concepts/meshes.md)içini ortaya çıkaran görsel bir özelliktir.
Aşağıdaki resim etkisini göstermektedir. Sol, sağdaki mesh içine bakabilirsiniz orijinal örgü gösterir:

![Düzlemi kes](./media/cutplane-1.png)

## <a name="limitations"></a>Sınırlamalar

* Azure Uzaktan İşlem, şimdilik en **fazla sekiz etkin kesme düzlemi**destekler. Daha fazla kesme düzlem bileşeni oluşturabilirsiniz, ancak aynı anda daha fazla etkinleştirmeye çalışırsanız, etkinleştirme yoksayacaktır. Hangi bileşenin sahneyi etkilemesi gerektiğini değiştirmek istiyorsanız, önce diğer düzlemleri devre dışı düşürün.
* Her kesilmiş düzlem, uzaktan işlenen tüm nesneleri etkiler. Şu anda belirli nesneleri veya kafes parçalarını dışlamanın bir yolu yoktur.
* Kesme düzlemleri tamamen görsel bir özelliktir, [uzamsal sorguların](spatial-queries.md)sonucunu etkilemezler. Açık bir kafese döküm ışınlamak istiyorsanız, rayın başlangıç noktasını kesik düzlemde olacak şekilde ayarlayabilirsiniz. Bu şekilde ışın sadece görünür parçalara çarpabilir.

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

Her etkin kesme düzlemi işleme sırasında küçük bir maliyete neden oluyor. Gerekli olmadığında kesilen düzlemleri devre dışı veya silin.

## <a name="cutplanecomponent"></a>CutPlaneComponent

*Bir CutPlaneComponent*oluşturarak sahneye bir kesme düzlemi eklersiniz. Düzlemin konumu ve yönü bileşenin sahibi [taraf](../../concepts/entities.md)tarafından belirlenir.

```cs
void CreateCutPlane(AzureSession session, Entity ownerEntity)
{
    CutPlaneComponent cutPlane = (CutPlaneComponent)session.Actions.CreateComponent(ObjectType.CutPlaneComponent, ownerEntity);
    cutPlane.Normal = Axis.X; // normal points along the positive x-axis of the owner object's orientation
    cutPlane.FadeColor = new Color4Ub(255, 0, 0, 128); // fade to 50% red
    cutPlane.FadeLength = 0.05f; // gradient width: 5cm
}
```

### <a name="cutplanecomponent-properties"></a>CutPlaneComponent özellikleri

Aşağıdaki özellikler kesilmiş düzlem bileşeninde açığa alınır:

* **Etkinleştirilmiş:** Bileşeni devre dışı bırakarak kesilmiş düzlemleri geçici olarak kapatabilirsiniz. Devre dışı bırakılan uçaklar genel akış oluşturma yapmaz ve aynı zamanda küresel kesme düzlem sınırına dahil değildir.

* **Normal:** Düzlem normal olarak hangi yönün (+X,-X,+Y,-Y,+Z,-Z) kullanılacağını belirtir. Bu yön, sahibi varlığın yönlendirmesine göredir. Tam yerleşim için sahibi varlığı taşıyın ve döndürün.

* **FadeColor** ve **FadeLength:**

  *FadeColor'un* alfa değeri sıfır değilse, kesilen düzleme yakın pikseller FadeColor'un RGB kısmına doğru kaybolur. Alfa kanalının gücü, solgun renk doğru tamamen solmaya ya da sadece kısmen olup olmadığını belirler. *FadeLength,* bu solmanın hangi mesafede gerçekleşeceğini tanımlar.

## <a name="next-steps"></a>Sonraki adımlar

* [Tek taraflı görüntüleme](single-sided-rendering.md)
* [Uzamsal sorgular](spatial-queries.md)
