---
title: Ağ yapıları
description: Azure uzaktan Işleme kapsamındaki kafeslerin tanımı
author: florianborn71
ms.author: flborn
ms.date: 02/05/2020
ms.topic: conceptual
ms.openlocfilehash: eea43f48abef5e2b258251d46eca1061a2263519
ms.sourcegitcommit: f845ca2f4b626ef9db73b88ca71279ac80538559
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89613846"
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

## <a name="api-documentation"></a>API belgeleri

* [C# kafes sınıfı](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.mesh)
* [C# MeshComponent sınıfı](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.meshcomponent)
* [C++ kafes sınıfı](https://docs.microsoft.com/cpp/api/remote-rendering/mesh)
* [C++ MeshComponent sınıfı](https://docs.microsoft.com/cpp/api/remote-rendering/meshcomponent)

## <a name="next-steps"></a>Sonraki adımlar

* [Malzemeler](materials.md)
