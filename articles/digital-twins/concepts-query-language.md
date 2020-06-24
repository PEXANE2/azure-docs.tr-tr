---
title: Sorgu dili
titleSuffix: Azure Digital Twins
description: Azure Digital TWINS sorgu deposu dilinin temellerini anlayın.
author: baanders
ms.author: baanders
ms.date: 3/26/2020
ms.topic: conceptual
ms.service: digital-twins
ROBOTS: NOINDEX, NOFOLLOW
ms.openlocfilehash: e4952bc3df2b5b164038654f5d77f4c352827463
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84726815"
---
# <a name="about-the-query-language-for-azure-digital-twins"></a>Azure dijital TWINS için sorgu dili hakkında

[!INCLUDE [Azure Digital Twins current preview status](../../includes/digital-twins-preview-status.md)]

Azure dijital TWINS merkezinin, **dijital TWINS** ve **ilişkilerden**oluşturulan [**ikizi Graf**](concepts-twins-graph.md)olduğunu unutmayın. Bu grafik, içerdiği dijital TWINS ve ilişkiler hakkında bilgi almak için sorgulanabilir. Bu sorgular, **Azure Digital TWINS sorgu deposu dili**adlı özel bir SQL benzeri sorgu dilinde yazılır.

Bir istemci uygulamasından hizmete bir sorgu göndermek için Azure Digital TWINS **sorgu API**'sini kullanırsınız. Bu, geliştiricilerin ikizi grafiğindeki dijital TWINS kümelerini ve Azure Digital TWINS senaryosuyla ilgili diğer bilgileri bulmasını ve filtre uygulamasını sağlar.

## <a name="query-language-features"></a>Sorgu dili özellikleri

Azure dijital TWINS, ikizi grafiğine karşı kapsamlı sorgu özellikleri sağlar. Sorgular, [IoT Hub sorgu dilinin](../iot-hub/iot-hub-devguide-query-language.md)yetilerinin bir üst KÜMESI olarak SQL benzeri sözdizimi kullanılarak açıklanmıştır.

Azure Digital TWINS sorgu deposu dilinde kullanılabilen işlemler şunlardır:
* Dijital TWINS 'in özelliklerine göre TWINS alın.
* Dijital TWINS 'e göre TWINS ' arabirimlerini alın.
* İlişki özelliklerine göre TWINS 'i alın.
* Birden çok ilişki türü (sorgular) üzerinden TWINS Al `JOIN` . `JOIN`İzin verilen (genel önizleme için bir düzey) sayısında sınırlamalar vardır.
* `IS_OF_MODEL(twinCollection, twinTypeName)`İkizi 'in [modeline](concepts-models.md)göre filtrelemeye izin veren özel işlevini kullanın. Devralmayı destekler.
* `AND`Yukarıdaki (, `OR` , `NOT` işleç) birleşimini kullanın.
* Skaler işlevleri kullanın: `IS_BOOL` , `IS_DEFINED` , `IS_NULL` , `IS_NUMBER` , `IS_OBJECT` , `IS_PRIMITIVE` , `IS_STRING` , `STARTS_WITH` , `ENDS_WITH` .
* Sorgu karşılaştırma işleçlerini kullanın: `AND` / `OR` / `NOT` , `IN` / `NOT IN` ,, `STARTSWITH` / `ENDSWITH` `=` , `!=` , `<` , `>` , `<=` , `>=` .
* Devamlılık kullan: sorgu nesnesi, bir sayfa boyutu (100 'e kadar) ile birlikte oluşturulur. Tek seferde dijital TWINS 'i, yönteme yapılan çağrıları yineleyerek alabilirsiniz `nextAsTwin` .

## <a name="next-steps"></a>Sonraki adımlar

Sorguları yazmayı ve bkz. [nasıl yapılır: ikizi grafiğini sorgulama](how-to-query-graph.md)hakkında istemci kodu örnekleri.