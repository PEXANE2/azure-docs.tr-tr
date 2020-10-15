---
author: baanders
description: Azure Digital TWINS sorgu işlemleri için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 7/28/2020
ms.author: baanders
ms.openlocfilehash: 333a7ec4ae0e5c8cbc94a603e2ccf81ee92e7d48
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92078468"
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
* Bir sorgunun döndüreceği sütunları seçmek için projeksiyonları kullanma
* Skaler işlevleri kullanın: `IS_BOOL` , `IS_DEFINED` , `IS_NULL` , `IS_NUMBER` , `IS_OBJECT` , `IS_PRIMITIVE` , `IS_STRING` , `STARTSWITH` , `ENDSWITH` .
* Sorgu karşılaştırma işleçlerini kullanın: `IN` / `NIN` , `=` , `!=` , `<` , `>` , `<=` , `>=` .
* Herhangi bir bileşim ( `AND` , `OR` , `NOT` işleç) `IS_OF_MODEL` , skaler işlevler ve karşılaştırma işleçlerini kullanın.
* Devamlılık kullan: sorgu nesnesi, bir sayfa boyutu (100 'e kadar) ile birlikte oluşturulur. Tek seferde dijital TWINS 'i, API 'ye sonraki çağrılarda devamlılık belirteci sağlayarak alabilirsiniz.