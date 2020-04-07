---
title: Hiyerarşik durum geçersiz kılma
description: Hiyerarşik durum geçersiz kılma bileşenleri kavramını açıklar.
author: florianborn71
ms.author: flborn
ms.date: 02/10/2020
ms.topic: article
ms.openlocfilehash: f3be073857cc8583669ab26f306760478479e2ae
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80680797"
---
# <a name="hierarchical-state-override"></a>Hiyerarşik durum geçersiz kılma

Çoğu durumda, bir [modelin](../../concepts/models.md)parçalarının görünümünü dinamik olarak değiştirmek gerekir , örneğin alt grafikleri gizlemek veya parçaları saydam işlemeye değiştirmek gibi. Tüm sahne grafiğiüzerinde tekrarlamayı ve her düğümde malzeme klonlama ve atamayı yönetmeyi gerektirdiğinden, ilgili her parçanın malzemelerini değiştirmek pratik değildir.

Bu kullanım örneğini mümkün olan en az `HierarchicalStateOverrideComponent`genel kullanıma sahip olarak gerçekleştirmek için . Bu bileşen, sahne grafiğinin rasgele dallarında hiyerarşik durum güncelleştirmeleri uygular. Bu, bir durum sahne grafiğinde herhangi bir düzeyde tanımlanabilir ve ya yeni bir durum tarafından geçersiz kılınana kadar hiyerarşi aşağı damlayan veya bir yaprak nesnesi uygulanmaktadır anlamına gelir.

Örnek olarak, bir araba modeli düşünün ve tüm araba şeffaf olarak değiştirmek istiyorum, iç motor parçası dışında. Bu kullanım örneği, bileşenin yalnızca iki örneğini içerir:

* İlk bileşen modelin kök düğümüne atanır ve tüm araç için saydam işlemeyi açar.
* İkinci bileşen, motorun kök düğümüne atanır ve açık bir şekilde bkz.'yi kapatarak durumu yine geçersiz kılar.

## <a name="features"></a>Özellikler

Geçersiz kılınabilecek sabit durum kümesi şunlardır:

* **Gizli**: Sahne grafiğindeki ilgili mekalar gizlenir veya gösterilir.
* **Renk rengi**: İşlenen bir nesne, tek tek renk rengi ve renk tonu ağırlığı ile renklendirilebilir. Aşağıdaki resimde tekerleğin jantını renklendiren renk gösterilmektedir.
  
  ![Renk Ton](./media/color-tint.png)

* **See-through**: Geometri yarı saydam olarak işlenir, örneğin bir nesnenin iç kısımlarını ortaya çıkarmak için. Aşağıdaki resim, kırmızı fren kaliperi dışında tüm otomobilin see-through modunda işlendiğini gösterir:

  ![Transparan](./media/see-through.png)

  > [!IMPORTANT]
  > See-through efekti yalnızca *TileBasedComposition* [oluşturma modu](../../concepts/rendering-modes.md) kullanıldığında çalışır.

* **Seçili**: Geometri bir [seçim anahattı](outlines.md)ile işlenir.

  ![Seçim Anahattı](./media/selection-outline.png)

* **DisableCollision**: Geometri [uzamsal sorgulardan](spatial-queries.md)muaftır. **Gizli** bayrak çakışmaları kapatmaz, bu nedenle bu iki bayrak genellikle birlikte ayarlanır.

## <a name="hierarchical-overrides"></a>Hiyerarşik geçersiz kılmalar

Nesne `HierarchicalStateOverrideComponent` hiyerarşisinin birden çok düzeyine eklenebilir. Bir varlıkta her türün yalnızca bir bileşeni `HierarchicalStateOverrideComponent` olabileceğinden, her biri gizli, gezinme, seçili, renk tontve çakışma durumlarını yönetir.

Bu nedenle her durum biri olarak ayarlanabilir:

* `ForceOn`- durum bu düğüm üzerinde ve altında tüm mesh için etkindir
* `ForceOff`- devlet bu düğüm üzerinde ve altında tüm meshes için devre dışı bırakılır
* `InheritFromParent`- durum bu geçersiz kılma bileşeni etkilenmez

Durumları doğrudan veya `SetState` işlev aracılığıyla değiştirebilirsiniz:

```cs
HierarchicalStateOverrideComponent component = ...;

// set one state directly
component.HiddenState = HierarchicalEnableState.ForceOn;

// set a state with the SetState function
component.SetState(HierarchicalStates.SeeThrough, HierarchicalEnableState.InheritFromParent);

// set multiple states at once with the SetState function
component.SetState(HierarchicalStates.Hidden | HierarchicalStates.DisableCollision, HierarchicalEnableState.ForceOff);
```

### <a name="tint-color"></a>Renk tonu

Renk tonu geçersiz kılma, hem açık/kapalı/devralma durumu hem de renk tonu özelliği olması açısından biraz özeldir. Renk tonunun alfa kısmı renk tonu efektinin ağırlığını tanımlar: 0,0 olarak ayarlanırsa, renk tonu görünmez ve 1,0 olarak ayarlanırsa nesne saf renk rengiyle işlenir. Aradaki değerler için son renk renk rengiyle karıştırılır. Renk tonu rengi, renk animasyonu elde etmek için kare başına olarak değiştirilebilir.

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

Kendisi bir `HierarchicalStateOverrideComponent` örnek çok çalışma süresi yükü eklemez. Ancak, etkin bileşenlerin sayısını düşük tutmak her zaman iyi bir uygulamadır. Örneğin, seçilen nesneyi vurgulayan bir seçim sistemi uygularken, vurgu kaldırıldığında bileşenin silinmeniz önerilir. Bileşenleri nötr özelliklerle çevrede tutmak hızla biraraya gelebilir.

Saydam işleme, sunucunun GPU'larına standart işlemeden daha fazla iş yükü koyar. Sahne grafiğinin büyük bölümleri *see-through'a*değiştirilirse , birçok geometri katmanı görünür durumdaysa, bir performans darboğazına dönüşebilir. Aynı [seçim anahatları](../../overview/features/outlines.md#performance)ile nesneler için geçerlidir.

## <a name="next-steps"></a>Sonraki adımlar

* [Özetliyor](../../overview/features/outlines.md)
* [Oluşturma modları](../../concepts/rendering-modes.md)
* [Uzamsal Sorgular](../../overview/features/spatial-queries.md)
