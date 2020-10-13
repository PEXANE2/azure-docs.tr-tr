---
author: baanders
description: Azure Digital TWINS sorgu işlemleri için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 7/28/2020
ms.author: baanders
ms.openlocfilehash: 450ea0b8024192f6f351b4893fe6e8e07db236f7
ms.sourcegitcommit: 50802bffd56155f3b01bfb4ed009b70045131750
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91931996"
---
## <a name="query-language-features"></a>Sorgu dili özellikleri

Azure dijital TWINS, ikizi grafiğine karşı kapsamlı sorgu özellikleri sağlar. Sorgular, benzer bir sorgu dili olan SQL benzeri sözdizimi kullanılarak açıklanmış ve birçok karşılaştırılabilir özelliği [IoT Hub sorgu diline](../articles/iot-hub/iot-hub-devguide-query-language.md) benzer.

> [!NOTE]
> Tüm Azure dijital TWINS sorgu işlemleri büyük/küçük harfe duyarlıdır.

Azure Digital TWINS sorgu dilinde kullanılabilen işlemler aşağıda verilmiştir.

Dijital TWINS 'i bunlara göre al...
* Model ( `IS_OF_MODEL` işlecini kullanarak)
* Özellikler ( [etiket özellikleri](../articles/digital-twins/how-to-use-tags.md)dahil)
* arabirimler
* ilişkiler
  - ilişkilerin özellikleri

Sorgularınızı aşağıdaki işlemlerle daha da geliştirebilirsiniz:
* Birden çok ilişki türü (sorgular) üzerinden TWINS Al `JOIN` . 
  - Önizleme süresince en fazla beş düzeye `JOIN` izin verilir.
* Yalnızca en üstteki sorgu sonuçlarını seçin ( `Select TOP` işleç)
* Kullanarak bir sonuç kümesindeki öğelerin sayısını say `Select COUNT`
* Skaler işlevleri kullanın: `IS_BOOL` , `IS_DEFINED` , `IS_NULL` , `IS_NUMBER` , `IS_OBJECT` , `IS_PRIMITIVE` , `IS_STRING` , `STARTSWITH` , `ENDSWITH` .
* Sorgu karşılaştırma işleçlerini kullanın: `IN` / `NIN` , `=` , `!=` , `<` , `>` , `<=` , `>=` .
* Herhangi bir bileşim ( `AND` , `OR` , `NOT` işleç) `IS_OF_MODEL` , skaler işlevler ve karşılaştırma işleçlerini kullanın.
* Devamlılık kullan: sorgu nesnesi, bir sayfa boyutu (100 'e kadar) ile birlikte oluşturulur. Tek seferde dijital TWINS 'i, API 'ye sonraki çağrılarda devamlılık belirteci sağlayarak alabilirsiniz.