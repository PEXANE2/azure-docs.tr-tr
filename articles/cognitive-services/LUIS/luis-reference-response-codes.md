---
title: API HTTP yanıt kodları-LUSıS
titleSuffix: Azure Cognitive Services
description: LUıS yazma ve uç nokta API 'Lerinden hangi HTTP yanıt kodlarının döndürüldüğünü anlayın
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 03/04/2019
ms.openlocfilehash: 46a7fff86d3a8c4539b77a0f271179d68e7a26d2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91541688"
---
# <a name="common-api-response-codes-and-their-meaning"></a>Ortak API yanıt kodları ve anlamları

[Yazma](https://go.microsoft.com/fwlink/?linkid=2092087) ve [uç nokta](https://go.microsoft.com/fwlink/?linkid=2092356) API 'leri http yanıt kodları döndürür. Yanıt iletileri bir isteğe özgü bilgileri içerirken, HTTP yanıt durum kodu genel olur.

## <a name="common-status-codes"></a>Ortak durum kodları
Aşağıdaki tabloda, [yazma](https://go.microsoft.com/fwlink/?linkid=2092087) ve [uç nokta](https://go.microsoft.com/fwlink/?linkid=2092356) API 'LERI için en yaygın http yanıtı durum kodlarından bazıları listelenmektedir:

|Kod|API|Açıklama|
|:--|--|--|
|400|Yazma, uç nokta|isteğin parametreleri yanlış, gerekli parametreler eksik, hatalı biçimlendirilmiş veya çok büyük|
|400|Yazma, uç nokta|isteğin gövdesi yanlış anlamı, JSON eksik, hatalı biçimlendirilmiş veya çok büyük|
|401|Yazma|anahtar yazmak yerine, kullanılan uç nokta abonelik anahtarı|
|401|Yazma, uç nokta|geçersiz, hatalı biçimlendirilmiş veya boş anahtar|
|401|Yazma, uç nokta| anahtar bölge ile eşleşmiyor|
|401|Yazma|sahibi veya ortak çalışan değilsiniz|
|401|Yazma|geçersiz API çağrıları sırası|
|403|Yazma, uç nokta|toplam aylık anahtar kota sınırı aşıldı|
|409|Uç Noktası|uygulama hala yükleniyor|
|410|Uç Noktası|uygulamanın geri çekilmesi ve yeniden yayımlanması gerekir|
|414|Uç Noktası|sorgu maksimum karakter sınırını aşıyor|
|429|Yazma, uç nokta|Hız sınırı aşıldı (istek/saniye)|

## <a name="next-steps"></a>Sonraki adımlar

* REST API [yazma](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) ve [uç nokta](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78) belgeleri
