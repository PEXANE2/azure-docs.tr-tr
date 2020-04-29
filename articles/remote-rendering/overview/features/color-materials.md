---
title: Renk malzemeleri
description: Renk malzemesi türünü açıklar.
author: jakrams
ms.author: jakras
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: 7cbcaefcc087c9f1c7c09668a27fbdef9a4802d3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80681083"
---
# <a name="color-materials"></a>Renk malzemeleri

*Renk malzemeleri* , Azure uzaktan işlemede desteklenen [malzeme türlerinden](../../concepts/materials.md) biridir. Her türlü aydınlatma almaması gereken, ancak her zaman tam parlaklık olacak [kafesler](../../concepts/meshes.md) için kullanılırlar. Bu, araba panoları, açık bulanlar gibi ' ışılandırma ' malzemeleri veya [photogrammetri](https://en.wikipedia.org/wiki/Photogrammetry)aracılığıyla elde edilen modeller gibi statik aydınlatma içeren veriler için büyük bir durum olabilir.

Renk malzemeleri, daha basit gölgeleme modeli nedeniyle [PBR malzemelerinden](pbr-materials.md) daha etkilidir. Farklı saydamlık modlarını da destekler.

## <a name="common-material-properties"></a>Ortak malzeme özellikleri

Bu özellikler tüm malzemelerde ortaktır:

* **Albedocolor:** Bu renk, *Albedomap* veya *köşe renkleri*gibi diğer renklerle çarpılır. Bir malzemeden *Saydamlık* etkinse alfa kanalı, tam donuk ve `1` `0` anlamı tamamen saydam olan opaklığı ayarlamak için kullanılır. Varsayılan değer beyazdır.

  > [!NOTE]
  > Renk malzemeleri ortamı yansıtmadığı için tamamen saydam bir renk malzemesi görünmez hale gelir. Bu, [PBR malzemeleri](pbr-materials.md)için farklıdır.

* **Albedomap:** Piksel başına Albedo değerleri için [2B doku](../../concepts/textures.md) .

* **alphaclipenabled** ve **alphaclipthreshold:** *alphaclipenabled* değeri true ise, Albedo Alpha değerinin *alphaclipthreshold* değerinden düşük olduğu tüm pikseller çizilmez. Alfa kırpması, saydamlığı etkinleştirmeden ve işlemek için çok daha hızlı bir şekilde kullanılabilir. Alfa kırpılan malzemeler, ancak tamamen opak malzemelerden daha yavaş işleme almaya devam etmektedir. Varsayılan olarak Alfa kırpması devre dışıdır.

* **textureCoordinateScale** ve **TEXTURECOORDINATEOFFSET:** ölçek UV doku koordinatlarına çarpılır, bu da ona eklenir. Dokuları uzatmak ve kaydırmak için kullanılabilir. Varsayılan ölçek (1, 1) ve konum (0, 0).

* **Usevertexcolor:** Kafes köşe renkleri içeriyorsa ve bu seçenek etkinleştirilirse, kafeslerin köşe renkleri *Albedocolor* ve *Albedomap*ile çarpılır. Varsayılan olarak köşe renkleri devre dışıdır.

* **ıdoubleyüzlü:** Çift sidebir değer true olarak ayarlanırsa, kamera arka yüzlerine baksa bile bu malzemeden üçgenler işlenir. Varsayılan olarak bu seçenek devre dışıdır. Ayrıca bkz. [tek taraflı işleme](single-sided-rendering.md).

## <a name="color-material-properties"></a>Renk malzemesi özellikleri

Aşağıdaki özellikler renk malzemelere özgüdür:

* **Vertexmix:** Bu değer `0` , ve `1` bir [kafesteki](../../concepts/meshes.md) köşe renginin son renge ne kadar katkıda bulunduğunu belirtir. Varsayılan değer olan 1 ' de, köşe rengi Albedo rengine tamamen çarpılır. 0 değeri ile köşe renkleri tamamen yok sayılır.

* **transparencyMode:** [PBR malzemelerinin](pbr-materials.md)aksine, renk malzemeleri farklı saydamlık modları arasında ayrım yapar:

  1. **Donuk:** Varsayılan mod saydamlığı devre dışı bırakır. Alfa kırpması hala mümkündür, ancak yeterli olması halinde tercih edilmelidir.
  
  1. **Alfakarıştırılan:** Bu mod, PBR malzemeleri için saydamlık moduna benzerdir. Cam gibi görme malzemeleri için kullanılmalıdır.

  1. **Eklenebilir:** Bu mod, en basit ve en etkili saydamlık modudur. Malzemenin katkısı işlenen görüntüye eklenir. Bu mod, önemli nesneleri vurgulamak için kullanılan işaretçiler gibi ışıanın (ancak hala saydam) nesnelerin benzetimini yapmak için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [PBR malzemeleri](pbr-materials.md)
* [Dokular](../../concepts/textures.md)
* [Ağ yapıları](../../concepts/meshes.md)
