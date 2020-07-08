---
title: Düzlemleri kesme
description: Kesilen düzlemleri ve bunların nasıl kullanılacağını açıklar.
author: jakrams
ms.author: jakras
ms.date: 02/06/2020
ms.topic: article
ms.openlocfilehash: 7adf9a9701eb2492f0b13a26af1dbaf8de631373
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84021373"
---
# <a name="cut-planes"></a>Düzlemleri kesme

*Kesilen düzlem* , sanal düzlemin bir tarafında bulunan ve [kafeslerin](../../concepts/meshes.md)içini gösteren bir görsel özelliktir.
Aşağıdaki görüntüde etkisi gösterilmektedir. Sol, sağ taraftaki orijinal kafesi gösterdiği gibi, kafesin içinde görünür:

![Düzlemi kes](./media/cutplane-1.png)

## <a name="limitations"></a>Sınırlamalar

* Azure uzaktan Işleme, şu anda **en fazla sekiz etkin kesme düzlemleri**destekler. Daha fazla kesme bileşeni oluşturabilirsiniz, ancak aynı anda daha fazlasını etkinleştirmeyi denerseniz, etkinleştirme yok sayılacak. Hangi bileşenin sahneyi etkileyeceğini değiştirmek istiyorsanız, önce diğer düzlemleri devre dışı bırakın.
* Her bir kesim, tüm uzaktan işlenen nesneleri etkiler. Şu anda belirli nesneleri veya kafes parçalarını hariç tutma yolu yoktur.
* Düzlemleri kesin bir görsel özelliktir, bu, [uzamsal sorguların](spatial-queries.md)sonucunu etkilemez. Bir kes açık bir ağ içine ışın dönüştürmek istiyorsanız, ışın başlangıç noktasını kes düzlede olacak şekilde ayarlayabilirsiniz. Bu şekilde, ışın yalnızca görünür bölümler alabilir.

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

Her etkin kesilen düzlem, işleme sırasında küçük bir maliyet doğurur. Gerekli olmadığında kesilen düzlemleri devre dışı bırakın veya silin.

## <a name="cutplanecomponent"></a>CutPlaneComponent

Bir *CutPlaneComponent*oluşturarak sahneye kesme düzlemi eklersiniz. Düzlemin konumu ve yönü, bileşenin sahip [varlığına](../../concepts/entities.md)göre belirlenir.

```cs
void CreateCutPlane(AzureSession session, Entity ownerEntity)
{
    CutPlaneComponent cutPlane = (CutPlaneComponent)session.Actions.CreateComponent(ObjectType.CutPlaneComponent, ownerEntity);
    cutPlane.Normal = Axis.X; // normal points along the positive x-axis of the owner object's orientation
    cutPlane.FadeColor = new Color4Ub(255, 0, 0, 128); // fade to 50% red
    cutPlane.FadeLength = 0.05f; // gradient width: 5cm
}
```

```cpp
void CreateCutPlane(ApiHandle<AzureSession> session, ApiHandle<Entity> ownerEntity)
{
    ApiHandle<CutPlaneComponent> cutPlane = session->Actions()->CreateComponent(ObjectType::CutPlaneComponent, ownerEntity)->as<CutPlaneComponent>();;
    cutPlane->Normal(Axis::X); // normal points along the positive x-axis of the owner object's orientation
    Color4Ub fadeColor;
    fadeColor.channels = { 255, 0, 0, 128 }; // fade to 50% red
    cutPlane->FadeColor(fadeColor);
    cutPlane->FadeLength(0.05f); // gradient width: 5cm
}
```


### <a name="cutplanecomponent-properties"></a>CutPlaneComponent özellikleri

Aşağıdaki özellikler kesilmiş bir düzlem bileşeni üzerinde kullanıma sunuldu:

* `Enabled`: Bileşeni devre dışı bırakarak, geçici olarak kesme düzlemleri kapatabilirsiniz. Devre dışı bırakılan kesme düzlemleri işleme ek yüküne neden olmaz ve ayrıca genel kesme düzeyi sınırına göre sayılmaz.

* `Normal`: Düzlem için normal olarak kullanılacak yönü (+ X,-X, + Y,-Y, + Z,-Z) belirtir. Bu yön, sahip varlığın yönüne göredir. Tam yerleştirme için sahip varlığını taşıyın ve döndürün.

* `FadeColor`ve `FadeLength` :

  *FadeColor* Alpha değeri sıfır değilse, kesme düzlemine yakın olan pikseller FADECOLOR 'in RGB bölümüne doğru bir şekilde görünür. Alfa kanalının kuvveti, silinme rengine tamamen veya kısmen doğru bir şekilde mi solacağını belirler. *FadeLength* , bu belirme mesafesinin ne kadar olacağını tanımlar.

## <a name="next-steps"></a>Sonraki adımlar

* [Tek taraflı işleme](single-sided-rendering.md)
* [Uzamsal sorgular](spatial-queries.md)
