---
title: Renk malzemeleri
description: Renk malzemesi türünü açıklar.
author: jakrams
ms.author: jakras
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: cda4aa9a811bac0ccf20caec32ee38da9b46b6c7
ms.sourcegitcommit: f845ca2f4b626ef9db73b88ca71279ac80538559
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89613763"
---
# <a name="color-materials"></a>Renk malzemeleri

*Renk malzemeleri* , Azure uzaktan işlemede desteklenen [malzeme türlerinden](../../concepts/materials.md) biridir. Her türlü aydınlatma almaması gereken, ancak her zaman tam parlaklık olacak [kafesler](../../concepts/meshes.md) için kullanılırlar. Bu, araba panoları, açık bulanlar gibi ' ışılandırma ' malzemeleri veya [photogrammetri](https://en.wikipedia.org/wiki/Photogrammetry)aracılığıyla elde edilen modeller gibi statik aydınlatma içeren veriler için büyük bir durum olabilir.

Renk malzemeleri, daha basit gölgeleme modeli nedeniyle [PBR malzemelerinden](pbr-materials.md) daha etkilidir. Farklı saydamlık modlarını da destekler.

## <a name="common-material-properties"></a>Ortak malzeme özellikleri

Bu özellikler tüm malzemelerde ortaktır:

* **Albedocolor:** Bu renk, *Albedomap* veya * :::no-loc text="vertex"::: renkler*gibi diğer renklerle çarpılır. Bir malzemeden *Saydamlık* etkinse alfa kanalı, `1` tam donuk ve anlamı tamamen saydam olan opaklığı ayarlamak için kullanılır `0` . Varsayılan değer beyazdır.

  > [!NOTE]
  > Renk malzemeleri ortamı yansıtmadığı için tamamen saydam bir renk malzemesi görünmez hale gelir. Bu, [PBR malzemeleri](pbr-materials.md)için farklıdır.

* **Albedomap:** Piksel başına Albedo değerleri için [2B doku](../../concepts/textures.md) .

* **alphaclipenabled** ve **alphaclipthreshold:** *alphaclipenabled* değeri true ise, Albedo Alpha değerinin *alphaclipthreshold* değerinden düşük olduğu tüm pikseller çizilmez. Alfa kırpması, saydamlığı etkinleştirmeden ve işlemek için çok daha hızlı bir şekilde kullanılabilir. Alfa kırpılan malzemeler, ancak tamamen opak malzemelerden daha yavaş işleme almaya devam etmektedir. Varsayılan olarak Alfa kırpması devre dışıdır.

* **textureCoordinateScale** ve **TEXTURECOORDINATEOFFSET:** ölçek UV doku koordinatlarına çarpılır, bu da ona eklenir. Dokuları uzatmak ve kaydırmak için kullanılabilir. Varsayılan ölçek (1, 1) ve konum (0, 0).

* **Usevertexcolor:** Kafes :::no-loc text="vertex"::: renkler içeriyorsa ve bu seçenek etkinleştirilirse, kafeslerin :::no-loc text="vertex"::: rengi *Albedocolor* ve *albedomap*ile çarpılır. Varsayılan olarak *UI, Texcolor* devre dışıdır.

* **ıdoubleyüzlü:** Çift sidebir değer true olarak ayarlanırsa, kamera arka yüzlerine baksa bile bu malzemeden üçgenler işlenir. Varsayılan olarak bu seçenek devre dışıdır. Ayrıca bkz. [ :::no-loc text="Single-sided"::: işleme](single-sided-rendering.md).

## <a name="color-material-properties"></a>Renk malzemesi özellikleri

Aşağıdaki özellikler renk malzemelere özgüdür:

* **Vertexmix:** Bu değer `0` , ve `1` :::no-loc text="vertex"::: bir [kafesteki](../../concepts/meshes.md) rengin son renge ne kadar sıkı katkıda bulunduğunu belirtir. Varsayılan değer olan 1 ' de, :::no-loc text="vertex"::: renk Albedo rengine tamamen çarpılır. 0 değeri ile :::no-loc text="vertex"::: renkler tamamen yok sayılır.

* **transparencyMode:** [PBR malzemelerinin](pbr-materials.md)aksine, renk malzemeleri farklı saydamlık modları arasında ayrım yapar:

  1. **Donuk:** Varsayılan mod saydamlığı devre dışı bırakır. Alfa kırpması hala mümkündür, ancak yeterli olması halinde tercih edilmelidir.
  
  1. **Alfakarıştırılan:** Bu mod, PBR malzemeleri için saydamlık moduna benzerdir. Cam gibi görme malzemeleri için kullanılmalıdır.

  1. **Eklenebilir:** Bu mod, en basit ve en etkili saydamlık modudur. Malzemenin katkısı işlenen görüntüye eklenir. Bu mod, önemli nesneleri vurgulamak için kullanılan işaretçiler gibi ışıanın (ancak hala saydam) nesnelerin benzetimini yapmak için kullanılabilir.

## <a name="api-documentation"></a>API belgeleri

* [C# colormalzemeler sınıfı](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.colormaterial)
* [C# RemoteManager. Createmalzemesi ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.remotemanager.creatematerial)
* [C++ Colormalzemeler sınıfı](https://docs.microsoft.com/cpp/api/remote-rendering/colormaterial)
* [C++ RemoteManager:: Createmateryal()](https://docs.microsoft.com/cpp/api/remote-rendering/remotemanager#creatematerial)

## <a name="next-steps"></a>Sonraki adımlar

* [PBR malzemeleri](pbr-materials.md)
* [Dokular](../../concepts/textures.md)
* [Ağ yapıları](../../concepts/meshes.md)
