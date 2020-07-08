---
title: Ağ yapıları
description: Azure uzaktan Işleme kapsamındaki kafeslerin tanımı
author: florianborn71
ms.author: flborn
ms.date: 02/05/2020
ms.topic: conceptual
ms.openlocfilehash: b800272a538243d39a89e4eed64bc5cbc2d53ad8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80681772"
---
# <a name="meshes"></a>Ağ yapıları

## <a name="mesh-resource"></a>Kafes kaynağı

Kafesler yalnızca [model dönüştürme](../how-tos/conversion/model-conversion.md)ile oluşturulabilecek sabit bir [paylaşılan kaynaktır](../concepts/lifetime.md). Kafesler bir veya birden çok *alt kafes*içeriyor. Her alt ağ, varsayılan olarak işlenmesi gereken bir [malzemeye](materials.md) başvurur. 3B alana bir ağ yerleştirmek için bir [varlığa](entities.md)bir [meshcomponent](#meshcomponent) ekleyin.

### <a name="mesh-resource-properties"></a>Kafes kaynağı özellikleri

`Mesh`Sınıf özellikleri şunlardır:

* **Malzemeler:** Bir dizi malzeme. Her malzeme farklı bir alt ağ tarafından kullanılır. Dizide birden çok girdi aynı [malzemeye](materials.md)başvurabilir. Bu veriler çalışma zamanında değiştirilemez.

* **Sınırlar:** Kafes köşelerine ait yerel alan ekseni hizalanmış bir sınırlayıcı kutusu (AABB).

## <a name="meshcomponent"></a>MeshComponent

`MeshComponent`Sınıf, bir kafes kaynağı örneğini yerleştirmek için kullanılır. Her bir MeshComponent tek bir kafes 'e başvurur. Her bir alt ağı işlemek için kullanılan malzemeleri geçersiz kılabilir.

### <a name="meshcomponent-properties"></a>MeshComponent özellikleri

* **Ağ:** Bu bileşen tarafından kullanılan kafes kaynağı.

* **Malzemeler:** Kafes bileşeninin kendisinde belirtilen malzeme dizisi. Dizi, her zaman kafes kaynağındaki *malzemeler* dizisiyle aynı uzunluğa sahip olacaktır. Bu dizide varsayılan olarak geçersiz kılınamayan malzemeler, bu dizide *null* olarak ayarlanır.

* **Usedmalzemeleri:** Her alt ağ için gerçekten kullanılan malzemeler dizisi. Null olmayan değerler için, *malzemeler* dizisindeki verilerle aynı olacaktır. Aksi takdirde, kafes örneğindeki *malzemeler* dizisinden değeri içerir.

## <a name="next-steps"></a>Sonraki adımlar

* [Malzemeler](materials.md)
