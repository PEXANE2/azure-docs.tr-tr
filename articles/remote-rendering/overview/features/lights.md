---
title: Işıklar
description: Işık kaynağı açıklaması ve özellikleri
author: florianborn71
ms.author: flborn
ms.date: 02/10/2020
ms.topic: article
ms.openlocfilehash: 0a4a226af1347b5302b0c3964889fc072f89e7f8
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80680953"
---
# <a name="lights"></a>Işıklar

Varsayılan olarak, uzaktan işlenen nesneler bir [gökyüzü ışığı](sky.md)kullanılarak yanar. Çoğu uygulama için bu zaten yeterlidir, ancak sahneye daha fazla ışık kaynakları ekleyebilirsiniz.

> [!IMPORTANT]
> Sadece [PBR malzemeleri](pbr-materials.md) ışık kaynaklarından etkilenir. [Renk malzemeleri](color-materials.md) her zaman tamamen parlak görünür.

> [!NOTE]
> Döküm gölgeler için şanda desteklenmiyor. Azure Uzaktan İşleme, gerektiğinde birden fazla GPU kullanarak büyük miktarda geometri oluşturmak için optimize edilebiyi optimize edecektir. Gölge döküm için geleneksel yaklaşımlar bu tür senaryolarda iyi çalışmaz.

## <a name="common-light-component-properties"></a>Ortak ışık bileşeni özellikleri

Tüm ışık türleri soyut taban `LightComponent` sınıfından türetilmiştir ve bu özellikleri paylaşır:

* **Renk:** [Gama uzayında](https://en.wikipedia.org/wiki/SRGB)ışığın rengi. Alfa yok sayılır.

* **Yoğunluk:** Işığın parlaklığı. Nokta ve nokta ışıkları için yoğunluk, ışığın ne kadar parlar olduğunu da tanımlar.

## <a name="point-light"></a>Nokta ışığı

Azure Uzaktan İşleme'de, daha yumuşak ışık kaynaklarını simüle etmek için yalnızca tek bir noktadan değil, küçük bir küreden veya küçük bir tüpten de ışık `PointLightComponent` yontabilir.

### <a name="pointlightcomponent-properties"></a>PointLightComponent özellikleri

* **Yarıçap:** Varsayılan yarıçap sıfırdır, bu durumda ışık bir nokta ışığı gibi davranır. Yarıçap sıfırdan büyükse, küresel ışık kaynağı olarak hareket eder, bu da aynasal vurgular görünümünü değiştirir.

* **Uzunluk:** Her `Length` ikisi `Radius` de ve sıfır olmayan ise, ışık bir tüp ışığı gibi davranır. Bu neon tüpleri simüle etmek için kullanılabilir.

* **ZayıflamaCutoff:** (0,0) bırakılırsa, ışığın zayıflaması yalnızca onun `Intensity`. Ancak, ışığın yoğunluğunun doğrusal olarak 0'a düşürüldüğü özel min/max mesafeler sağlayabilirsiniz. Bu özellik, belirli bir ışığın daha küçük bir etki aralığını zorlamak için kullanılabilir.

* **Yansıtılan Küp Haritası:** Geçerli bir [küp eşlemi](../../concepts/textures.md)olarak ayarlanırsa, doku ışığın çevresindeki geometriye yansıtılır. Küp haritanın rengi ışığın rengiyle modüle edilir.

## <a name="spot-light"></a>Spot ışık

Benzer `SpotLightComponent` `PointLightComponent` ama ışık bir koni şeklinde sınırlıdır. Koninin yönü *sahibi varlığın negatif z ekseni*tarafından tanımlanır.

### <a name="spotlightcomponent-properties"></a>SpotLightComponent özellikleri

* **Yarıçap:** Aynı şey `PointLightComponent`için.

* **SpotAngleDeg:** Bu aralık, derece cinsinden ölçülen koninin iç ve dış açısını tanımlar. İç açıdaki her şey tam parlaklıkla aydınlatılır. Bir düşüş penumbra benzeri bir etki oluşturur dış açı doğru uygulanır.

* **FalloffExponent:** Düşmenin iç ve dış koni açısı arasındaki geçişleri ne kadar keskin bir şekilde tanımlar. Daha yüksek bir değer daha keskin bir geçişle sonuçlanır. 1.0 varsayılan doğrusal bir geçiş sonuçlanır.

* **ZayıflamaCutoff:** Aynı şey `PointLightComponent`için.

* **Öngörülen2dTexture:** Geçerli bir [2B dokuya](../../concepts/textures.md)ayarlanırsa, görüntü ışığın parladığı geometriye yansıtılır. Dokunun rengi ışığın rengiyle modüle edilir.

## <a name="directional-light"></a>Yönlü ışık

Sonsuz `DirectionalLightComponent` uzaktaki bir ışık kaynağını simüle eder. Işık, sahibi varlığın *negatif z ekseninin*yönüne doğru parlar. Varlığın konumu göz ardı edilir.

Ek özellik yok.

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

Işık kaynaklarının performans oluşturma üzerinde önemli bir etkisi vardır. Bunları dikkatli ve yalnızca uygulama nın gerektirdiği takdirde kullanın. Statik yönlü bileşen de dahil olmak üzere herhangi bir statik küresel aydınlatma durumu, ek görüntüleme maliyeti olmadan, özel bir [gökyüzü dokusu](sky.md)ile elde edilebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Malzemeler](../../concepts/materials.md)
* [Gök -yüzü](sky.md)
