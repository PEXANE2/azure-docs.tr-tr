---
author: baanders
description: Azure Digital TWINS sorgu işlemleri için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 7/28/2020
ms.author: baanders
ms.openlocfilehash: 70ff1847548c1328a709cf17c02bba3dd25ba213
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87486671"
---
## <a name="query-language-features"></a>Sorgu dili özellikleri

Azure dijital TWINS, ikizi grafiğine karşı kapsamlı sorgu özellikleri sağlar. Sorgular, benzer bir sorgu dili olan SQL benzeri sözdizimi kullanılarak açıklanmış ve birçok karşılaştırılabilir özelliği [IoT Hub sorgu diline](../articles/iot-hub/iot-hub-devguide-query-language.md) benzer.

> [!NOTE]
> Tüm Azure dijital TWINS sorgu işlemleri büyük/küçük harfe duyarlıdır.

Azure Digital TWINS sorgu deposu dilinde kullanılabilen işlemler şunlardır.

Dijital TWINS 'i bunlara göre al...
* Model ( `IS_OF_MODEL` işlecini kullanarak)
* Özellikler ( [etiket özellikleri](../articles/digital-twins/how-to-use-tags.md)dahil)
* arabirimler
* ilişkiler
  - ilişkilerin özellikleri

Sorgularınızı aşağıdaki işlemlerle daha da geliştirebilirsiniz:
* Birden çok ilişki türü (sorgular) üzerinden TWINS Al `JOIN` . 
  - `JOIN`İzin verilen (genel önizleme için bir düzey) sayısında sınırlamalar vardır.
* Yalnızca en üstteki sorgu sonuçlarını seçin ( `Select TOP` işleç)
* Skaler işlevleri kullanın: `IS_BOOL` , `IS_DEFINED` , `IS_NULL` , `IS_NUMBER` , `IS_OBJECT` , `IS_PRIMITIVE` , `IS_STRING` , `STARTSWITH` , `ENDSWITH` .
* Sorgu karşılaştırma işleçlerini kullanın: `IN` / `NIN` , `=` , `!=` , `<` , `>` , `<=` , `>=` .
* Herhangi bir bileşim ( `AND` , `OR` , `NOT` işleç) `IS_OF_MODEL` , skaler işlevler ve karşılaştırma işleçlerini kullanın.
* Devamlılık kullan: sorgu nesnesi, bir sayfa boyutu (100 'e kadar) ile birlikte oluşturulur. Tek seferde dijital TWINS 'i, API 'ye sonraki çağrılarda devamlılık belirteci sağlayarak alabilirsiniz.