---
title: Sorgu dili
titleSuffix: Azure Digital Twins
description: Azure Digital TWINS sorgu deposu dilinin temellerini anlayın.
author: baanders
ms.author: baanders
ms.date: 3/26/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: f7e9a76309b4d9dcd010b85d1b55f340374be5c4
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87337934"
---
# <a name="about-the-query-language-for-azure-digital-twins"></a>Azure dijital TWINS için sorgu dili hakkında

Azure dijital TWINS merkezinin, **dijital TWINS** ve **ilişkilerden**oluşturulan [**ikizi Graf**](concepts-twins-graph.md)olduğunu unutmayın. Bu grafik, içerdiği dijital TWINS ve ilişkiler hakkında bilgi almak için sorgulanabilir. Bu sorgular, **Azure Digital TWINS sorgu deposu dili**adlı özel bir SQL benzeri sorgu dilinde yazılır.

Bir istemci uygulamasından hizmete bir sorgu göndermek için Azure Digital TWINS [**sorgu API**](https://docs.microsoft.com/dotnet/api/azure.digitaltwins.core.digitaltwinsclient.query?view=azure-dotnet-preview)'sini kullanırsınız. Bu, geliştiricilerin ikizi grafiğindeki dijital TWINS kümelerini ve Azure Digital TWINS senaryosuyla ilgili diğer bilgileri bulmasını ve filtre uygulamasını sağlar.

## <a name="query-language-features"></a>Sorgu dili özellikleri

Azure dijital TWINS, ikizi grafiğine karşı kapsamlı sorgu özellikleri sağlar. Sorgular, benzer bir sorgu dili olan SQL benzeri sözdizimi kullanılarak açıklanmış ve birçok karşılaştırılabilir özelliği [IoT Hub sorgu diline](../iot-hub/iot-hub-devguide-query-language.md) benzer.

> [!NOTE]
> Tüm Azure dijital TWINS sorgu işlemleri büyük/küçük harfe duyarlıdır.

Azure Digital TWINS sorgu deposu dilinde kullanılabilen işlemler şunlardır:
* Dijital TWINS 'in özelliklerine göre TWINS 'leri ( [Etiketler](how-to-use-tags.md)dahil) alın.
* Dijital TWINS 'e göre TWINS ' arabirimlerini alın.
* İlişki özelliklerine göre TWINS 'i alın.
* Birden çok ilişki türü (sorgular) üzerinden TWINS Al `JOIN` . `JOIN`İzin verilen (genel önizleme için bir düzey) sayısında sınırlamalar vardır.
* `IS_OF_MODEL(twinCollection, twinTypeName)`İkizi 'in [modeline](concepts-models.md)göre filtrelemeye izin veren özel işlevini kullanın. Devralmayı destekler.
* Skaler işlevleri kullanın: `IS_BOOL` , `IS_DEFINED` , `IS_NULL` , `IS_NUMBER` , `IS_OBJECT` , `IS_PRIMITIVE` , `IS_STRING` , `STARTS_WITH` , `ENDS_WITH` .
* Sorgu karşılaştırma işleçlerini kullanın: `IN` / `NIN` , `=` , `!=` , `<` , `>` , `<=` , `>=` .
* `AND`Yukarıdaki (, `OR` , `NOT` işleç) birleşimini kullanın.
* Devamlılık kullan: sorgu nesnesi, bir sayfa boyutu (100 'e kadar) ile birlikte oluşturulur. Tek seferde dijital TWINS 'i, API 'ye sonraki çağrılarda devamlılık belirteci sağlayarak alabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Sorguları yazmayı ve bkz. [*nasıl yapılır: ikizi grafiğini sorgulama*](how-to-query-graph.md)hakkında istemci kodu örnekleri.
