---
title: Örnek modeller
description: Örnek modeller için kaynakları listeler.
author: florianborn71
ms.author: flborn
ms.date: 01/29/2020
ms.topic: sample
ms.openlocfilehash: 0c28d36c7934fbbac0ddd11562c8dc237b0360ee
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80679488"
---
# <a name="sample-models"></a>Örnek modeller

Bu makalede, Azure uzaktan Işleme hizmetini test etmek için kullanılabilecek örnek veriler için bazı kaynaklar listelenmektedir.

## <a name="built-in-sample-model"></a>Yerleşik örnek model

**Builtin://Engine** URL 'si kullanılarak her zaman yüklenebilecek yerleşik bir örnek model sunuyoruz

![Örnek model](./media/sample-model.png "Örnek model")

Model istatistikleri:

| Adı | Değer |
|-----------|:-----------|
| [Gerekli VM boyutu](../how-tos/session-rest-api.md#create-a-session) | Stand |
| Üçgen sayısı | 18.700.000 |
| Taşınabilir parça sayısı | 2073 |
| Malzeme sayısı | 94 |

## <a name="third-party-data"></a>Üçüncü taraf verileri

Khronos Grubu, test için bir glTF örnek modeli kümesi tutar. ARR, hem Text (*. gltf*) hem de binary (*. GLB*) biçiminde gltf biçimini destekler. En iyi görsel sonuçlar için PBR modellerini kullanmanızı öneririz:

* [glTF örnek modelleri](https://github.com/KhronosGroup/glTF-Sample-Models)

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Unity ile model Işleme](../quickstarts/render-model.md)
* [Hızlı başlangıç: bir modeli işleme için dönüştürme](../quickstarts/convert-model.md)
