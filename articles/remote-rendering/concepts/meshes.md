---
title: Kafes
description: Azure Uzaktan İşleme kapsamındaki meshes tanımı
author: florianborn71
ms.author: flborn
ms.date: 02/05/2020
ms.topic: conceptual
ms.openlocfilehash: b800272a538243d39a89e4eed64bc5cbc2d53ad8
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681772"
---
# <a name="meshes"></a>Kafes

## <a name="mesh-resource"></a>Kafes kaynağı

Meshes değişmez paylaşılan [bir kaynaktır](../concepts/lifetime.md), bu sadece [model dönüştürme](../how-tos/conversion/model-conversion.md)yoluyla oluşturulabilir. Meshes bir veya birden çok *submeshes*içerir. Her alt kafes, varsayılan olarak işlenmesi gereken bir [malzemeye](materials.md) başvurur. Bir kafesi 3B alana yerleştirmek için, Bir [Varlığa](entities.md) [Bir Kafes Bileşeni](#meshcomponent) ekleyin.

### <a name="mesh-resource-properties"></a>Örgü kaynak özellikleri

Sınıf `Mesh` özellikleri şunlardır:

* **Malzemeler:** Bir dizi malzeme. Her malzeme farklı bir alt kafes tarafından kullanılır. Dizideki birden çok giriş aynı [malzemeye](materials.md)başvurabilir. Bu veriler çalışma zamanında değiştirilemez.

* **Sınırlar:** Kafes tepelerinin yerel alan eksen hizalı sınırlayıcı kutusu (AABB).

## <a name="meshcomponent"></a>MeshComponent

Sınıf, `MeshComponent` kafes kaynağının bir örneğini yerleştirmek için kullanılır. Her MeshComponent tek bir kafese başvurur. Her alt kafesi işlemek için hangi malzemelerin kullanıldığını geçersiz kılabilir.

### <a name="meshcomponent-properties"></a>MeshComponent özellikleri

* **Kafes:** Bu bileşen tarafından kullanılan kafes kaynağı.

* **Malzemeler:** Kafes bileşeninin kendisinde belirtilen malzeme dizisi. Dizi her zaman kafes kaynağındaki *Malzemeler* dizisiyle aynı uzunluğa sahip olacaktır. Kafes varsayılanından geçersiz kılınmayan malzemeler bu dizide *null* olarak ayarlanır.

* **Kullanılmış Malzemeler:** Her alt kafes için kullanılan malzemelerin dizi. Null olmayan değerler için *Malzemeler* dizisindeki verilerle aynı olacaktır. Aksi takdirde kafes örneğindeki *Malzemeler* dizisinden gelen değeri içerir.

## <a name="next-steps"></a>Sonraki adımlar

* [Malzemeler](materials.md)
