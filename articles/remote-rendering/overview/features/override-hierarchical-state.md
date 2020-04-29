---
title: Hiyerarşik durumu geçersiz kılma
description: Hiyerarşik durum geçersiz kılma bileşenleri kavramını açıklar.
author: florianborn71
ms.author: flborn
ms.date: 02/10/2020
ms.topic: article
ms.openlocfilehash: f3be073857cc8583669ab26f306760478479e2ae
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80680797"
---
# <a name="hierarchical-state-override"></a>Hiyerarşik durumu geçersiz kılma

Çoğu durumda, bir [modelin](../../concepts/models.md)bölümlerinin görünümünü dinamik olarak değiştirmek, örneğin alt grafikleri gizlemek veya bölümleri saydam işlemeye geçirmek gerekir. Dahil edilen her bir bölümün materyalini değiştirmek, tüm sahne grafiğinin üzerinde yineleme yapmak ve her düğümde malzeme kopyalamayı ve atamayı yönetmek gerektirdiğinden pratik değildir.

En az olası ek yük ile bu kullanım örneğini başarmak için, kullanın `HierarchicalStateOverrideComponent`. Bu bileşen, sahne grafiğinin rastgele dallarında hiyerarşik durum güncelleştirmelerini uygular. Yani, bir durum sahne grafiğinde herhangi bir düzeyde tanımlanabilir ve yeni bir durum tarafından geçersiz kılınana ya da bir yaprak nesneye uygulanana kadar hiyerarşiyi aşağı doğru ayırır.

Örnek olarak, bir otomobil modelini göz önünde bulundurun ve iç altyapı bölümü dışında tüm arabayı saydam olacak şekilde değiştirmek isteyebilirsiniz. Bu kullanım örneği, bileşenin yalnızca iki örneğini içerir:

* İlk bileşen modelin kök düğümüne atanır ve tüm otomobil için saydam işlemeyi etkinleştirir.
* İkinci bileşen, altyapının kök düğümüne atanır ve doğrudan göster modunu devre dışı bırakarak durumu yeniden geçersiz kılar.

## <a name="features"></a>Özellikler

Geçersiz kılınabilen sabit durumlar kümesi şunlardır:

* **Gizli**: sahne grafiğindeki ilgili kafesler gizlenir veya gösterilir.
* Renk **tonu rengi**: işlenmiş bir nesne, tek tek renk tonu rengi ve renk tonu ağırlığı ile renk tonlu olabilir. Aşağıdaki görüntüde, bir tekerleğin kıyısı renk gösterilmektedir.
  
  ![Renk renk tonu](./media/color-tint.png)

* **Bkz.-Through**: geometri, örneğin bir nesnenin iç parçalarını açığa çıkarmak için yarı şeffaf olarak işlenir. Aşağıdaki görüntüde, kırmızı hızınızı düşüren Caliper dışında, bkz.-Through modunda işlenen tüm otomobil gösterilmektedir:

  ![Bkz.](./media/see-through.png)

  > [!IMPORTANT]
  > Yalnızca *Tilebasedcomposition* [işleme modu](../../concepts/rendering-modes.md) kullanıldığında, diğer adım etkisi geçerlidir.

* **Seçili**: geometri bir [seçim ana hattı](outlines.md)ile işlenir.

  ![Seçim ana hattı](./media/selection-outline.png)

* **Disableçarpışma**: geometri, [uzamsal sorgulardan](spatial-queries.md)muaf tutulur. **Gizli** bayrağı, çakışmaları devre dışı bırakır, bu nedenle bu iki bayrak genellikle birlikte ayarlanır.

## <a name="hierarchical-overrides"></a>Hiyerarşik geçersiz kılmalar

, `HierarchicalStateOverrideComponent` Bir nesne hiyerarşisinin birden çok düzeyine eklenebilir. Bir varlıktaki her bir türün yalnızca bir bileşeni olabilir, her biri `HierarchicalStateOverrideComponent` gizli, bkz.-Through, seçilen, renk tonu ve çarpışma için durumları yönetir.

Bu nedenle, her durum şunlardan birine ayarlanabilir:

* `ForceOn`-Bu düğüm üzerindeki ve altındaki tüm kafesde durum etkindir
* `ForceOff`-Bu düğüm üzerindeki ve altındaki tüm kafesler için durum devre dışı bırakıldı
* `InheritFromParent`-durum bu geçersiz kılma bileşeninden etkilenmemiştir

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

### <a name="tint-color"></a>Renk tonu rengi

Hem açık/kapalı/devralma durumu hem de bir renk tonu rengi özelliği olan renk tonu rengi geçersiz kılma biraz özeldir. Renk tonu renginin alfa kısmı, renk azaltma efektinin kalınlığını tanımlar: 0,0 olarak ayarlanırsa, renk tonu rengi görünür olmaz ve 1,0 olarak ayarlandıysa nesne saf renk tonu rengiyle işlenir. -Between değerleri için, son renk, renk tonu rengiyle karışacaktır. Renkli bir animasyon elde etmek için renk tonu rengi çerçeve başına temelinde değiştirilebilir.

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

Bir örneği, `HierarchicalStateOverrideComponent` çok çalışma zamanı ek yükü eklemez. Ancak, etkin bileşenlerin sayısını düşük tutmak her zaman iyi bir uygulamadır. Örneğin, çekilen nesneyi vurgulayan bir seçim sistemi uygularken, vurgu kaldırıldığında bileşeni silmeniz önerilir. Bileşenlerin nötr özelliklerle birlikte tutulması hızlı bir şekilde eklenebilir.

Saydam işleme, sunucunun GPU 'lara standart işleme göre daha fazla iş yükü koyar. Sahne grafiğinin büyük *bölümleri, çok*sayıda geometrinin görünür olması halinde, görünen bir performans sorunu haline gelebilir. Aynı [seçim anahatları](../../overview/features/outlines.md#performance)olan nesneler için de geçerlidir.

## <a name="next-steps"></a>Sonraki adımlar

* [Özetler](../../overview/features/outlines.md)
* [İşleme modları](../../concepts/rendering-modes.md)
* [Uzamsal sorgular](../../overview/features/spatial-queries.md)
