---
title: Örnek modeller
description: Örnek modeller için kaynakları listeler.
author: florianborn71
ms.author: flborn
ms.date: 01/29/2020
ms.topic: sample
ms.openlocfilehash: 0c28d36c7934fbbac0ddd11562c8dc237b0360ee
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679488"
---
# <a name="sample-models"></a>Örnek modeller

Bu makalede, Azure Uzaktan İşlem hizmetini sınaması için kullanılabilecek örnek veriler için bazı kaynaklar listelenir.

## <a name="built-in-sample-model"></a>Dahili örnek model

**URL** builtin://Engine kullanılarak her zaman yüklenebilen yerleşik bir örnek model salıyoruz

![Örnek model](./media/sample-model.png "Örnek model")

Model istatistikleri:

| Adı | Değer |
|-----------|:-----------|
| [Gerekli VM boyutu](../how-tos/session-rest-api.md#create-a-session) | Standart |
| Üçgen sayısı | 18.7 Milyon |
| Hareketli parça sayısı | 2073 |
| Malzeme sayısı | 94 |

## <a name="third-party-data"></a>Üçüncü taraf verileri

Khronos Grubu test etmek için glTF örnek modelleri bir dizi tutar. ARR glTF biçimini hem metinde (*.gltf)* hem de ikili (*.glb*) biçiminde destekler. En iyi görsel sonuçlar için PBR modellerini kullanmanızı öneririz:

* [glTF Örnek Modelleri](https://github.com/KhronosGroup/glTF-Sample-Models)

## <a name="next-steps"></a>Sonraki adımlar

* [Quickstart: Unity ile bir model oluşturma](../quickstarts/render-model.md)
* [Quickstart: Oluşturma için bir modeli dönüştürme](../quickstarts/convert-model.md)
