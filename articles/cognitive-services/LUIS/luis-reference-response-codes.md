---
title: API HTTP yanıt kodları - LUIS
titleSuffix: Azure Cognitive Services
description: HTTP yanıt kodlarının LUIS Yazma ve Bitiş Noktası API'lerinden ne döndürülür olduğunu anlama
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 03/04/2019
ms.author: diberry
ms.openlocfilehash: 405381a2d80ee6469b2b90bcb25923004fe55882
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78270241"
---
# <a name="common-api-response-codes-and-their-meaning"></a>Ortak API yanıt kodları ve anlamları

Yazma [authoring](https://go.microsoft.com/fwlink/?linkid=2092087) ve [bitiş noktası](https://go.microsoft.com/fwlink/?linkid=2092356) API'leri HTTP yanıt kodlarını döndürer. Yanıt iletileri isteğe özgü bilgileri içerse de, HTTP yanıt durum kodu geneldir.

## <a name="common-status-codes"></a>Ortak durum kodları
Aşağıdaki [tabloda, yazma](https://go.microsoft.com/fwlink/?linkid=2092087) ve [bitiş noktası](https://go.microsoft.com/fwlink/?linkid=2092356) API'leri için en yaygın HTTP yanıt durum kodlarından bazıları listelenir:

|Kod|API|Açıklama|
|:--|--|--|
|400|Yazma, Bitiş Noktası|isteğin parametreleri yanlış, yani gerekli parametreler eksik, biçimsiz veya çok büyük|
|400|Yazma, Bitiş Noktası|isteğin gövdesi yanlış, yani JSON eksik, biçimsiz veya çok büyük|
|401|Yazma|kullanılan uç nokta abonelik anahtarı, anahtar yazma yerine|
|401|Yazma, Bitiş Noktası|geçersiz, biçimsiz veya boş anahtar|
|401|Yazma, Bitiş Noktası| anahtar bölgeyle eşleşmiyor|
|401|Yazma|sahibi veya işbirlikçisi değilseniz|
|401|Yazma|API çağrılarının geçersiz sırası|
|403|Yazma, Bitiş Noktası|toplam aylık anahtar kota sınırı aşıldı|
|409|Uç Nokta|uygulama hala yükleniyor|
|410|Uç Nokta|uygulamanın yeniden eğitilmesi ve yeniden yayınlanması gerekiyor|
|414|Uç Nokta|sorgu maksimum karakter sınırını aşıyor|
|429|Yazma, Bitiş Noktası|Oran sınırı aşılır (istekler/saniye)|

## <a name="next-steps"></a>Sonraki adımlar

* REST API [yazma](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) ve [bitiş noktası](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78) belgeleri
