---
title: Renkli malzemeler
description: Renk malzeme türünü açıklar.
author: jakrams
ms.author: jakras
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: 7cbcaefcc087c9f1c7c09668a27fbdef9a4802d3
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681083"
---
# <a name="color-materials"></a>Renkli malzemeler

*Renk malzemeleri,* Azure Uzaktan İşleme'de desteklenen [malzeme türlerinden](../../concepts/materials.md) biridir. Onlar aydınlatma her türlü almamalıdır [meshes](../../concepts/meshes.md) için kullanılır, ama daha ziyade her zaman tam parlaklık. Bu durum, araba panoları, ampuller gibi 'parlayan' malzemeler veya [fotogrametri](https://en.wikipedia.org/wiki/Photogrammetry)yoluyla elde edilen modeller gibi statik aydınlatmayı zaten içeren veriler için geçerli olabilir.

Renk malzemeleri, daha basit gölgelendirme modeli sayesinde [PBR malzemelerine](pbr-materials.md) göre daha verimli işleme yedaha verimlidir. Ayrıca farklı saydamlık modlarını destekler.

## <a name="common-material-properties"></a>Ortak malzeme özellikleri

Bu özellikler tüm malzemeler için ortaktur:

* **albedoColor:** Bu renk *albedoMap* veya *vertex renkler*gibi diğer renkler ile çarpılır. Bir malzemeüzerinde *saydamlık* etkinse, alfa kanalı opaklığı `1` ayarlamak için kullanılır, `0` anlamı tamamen opak ve anlamı tamamen saydamdır. Varsayılan değer beyazdır.

  > [!NOTE]
  > Renk malzemeleri çevreyi yansıtmadığından, tamamen saydam bir renk malzemesi görünmez olur. Bu [PBR malzemeler](pbr-materials.md)için farklıdır.

* **albedoMap:** Piksel başına albedo değerleri için [2B doku.](../../concepts/textures.md)

* **alphaClipEnabled** ve **alphaClipThreshold:** *alphaClipEnabled* doğruysa, albedo alfa değerinin *alphaClipThreshold'den* daha düşük olduğu tüm pikseller çizilmez. Alfa kırpma bile saydamlık etkinleştirmeden kullanılabilir ve işlemek için çok daha hızlıdır. Alfa kırpılmış malzemeler hala tam opak malzemeler daha işlemek için daha yavaş olsa da. Varsayılan olarak alfa kırpma devre dışı bırakılır.

* **textureCoordinateScale** ve **textureCoordinateOffset:** Ölçek UV doku koordinatları içine çarpılır, ofset eklenir. Dokuları germek ve kaydırmak için kullanılabilir. Varsayılan ölçek (1, 1) ve ofset (0, 0) 'dir.

* **useVertexColor:** Kafes tepe noktası renkleri içeriyorsa ve bu seçenek etkinse, kafeslerin tepe noktası renkleri *albedoColor* ve *albedoMap*ile çarpılır. Varsayılan olarak vertex renkleri devre dışı bırakılır.

* **isDoubleSided:** Çift taraflılık gerçek olarak ayarlanmışsa, kamera arka yüzlerine baksa bile bu malzemeye sahip üçgenler işlenir. Varsayılan olarak bu seçenek devre dışı bırakılır. Ayrıca bakınız [Tek taraflı görüntüleme.](single-sided-rendering.md)

## <a name="color-material-properties"></a>Renk malzemesi özellikleri

Aşağıdaki özellikler renk malzemelerine özgüdir:

* **vertexMix:** Arasındaki `0` bu `1` değer ve [bir kafesteki](../../concepts/meshes.md) tepe noktası renginin son renge ne kadar güçlü bir şekilde katkıda bulunup bulunup bulunur. 1 varsayılan değeri, tepe noktası rengi tamamen albedo renk çarpılır. 0 değeri yle, tepe noktası renkleri tamamen yoksayılır.

* **transparencyMode:** [PBR malzemelerinin](pbr-materials.md)aksine, renk malzemeleri farklı saydamlık modları arasında ayrım yapın:

  1. **Opak:** Varsayılan mod saydamlığı devre dışı bırakır. Alfa kırpma hala mümkündür, ancak, ve tercih edilmelidir, eğer yeterli.
  
  1. **AlphaBlended:** Bu mod, PBR malzemeleri için saydamlık moduna benzer. Cam gibi see-through malzemeler için kullanılmalıdır.

  1. **Katkı maddesi:** Bu mod en basit ve en verimli saydamlık modudur. Malzemenin katkısı işlenen görüntüye eklenir. Bu mod, önemli nesneleri vurgulamak için kullanılan işaretçiler gibi parlayan (ancak yine de saydam) nesneleri simüle etmek için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [PBR malzemeleri](pbr-materials.md)
* [Doku](../../concepts/textures.md)
* [Kafes](../../concepts/meshes.md)
