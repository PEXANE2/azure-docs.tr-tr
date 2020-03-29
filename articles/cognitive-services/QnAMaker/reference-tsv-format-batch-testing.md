---
title: Toplu test TSV formatı - QnA Maker
titleSuffix: Azure Cognitive Services
description: Toplu iş testi için TSV biçimini anlama
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 10/24/2019
ms.author: diberry
ms.openlocfilehash: dccf034b6497651db70b6cc09fff3f1b1d645a1b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73507830"
---
# <a name="batch-testing-tsv-format"></a>Toplu test TSV formatı

Toplu işlem [kaynak kodundan](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/batchtesting) veya indirilebilir bir [çalıştırılabilir sıkıştırılmış](https://aka.ms/qna_btzip)olarak kullanılabilir. Toplu iş testini çalıştırmak için komutun biçimi:

```console
batchtesting.exe input.tsv https://YOUR-HOST.azurewebsites.net ENDPOINT-KEY out.tsv
```

|Param|Beklenen Değer|
|--|--|
|1|[TSV giriş alanları](#tsv-input-fields) ile biçimlendirilmiş tsv dosyasının adı|
|2|Son nokta için URI, QnA Maker portalının Yayın sayfasından YOUR-HOST ile.|
|3|ENDPOINT-KEY, QnA Maker portalının Yayımlama sayfasında bulunur.|
|4|sonuçlar için toplu test tarafından oluşturulan tsv dosyasının adı.|

Toplu iş testi için TSV biçimini anlamak ve uygulamak için aşağıdaki bilgileri kullanın. 

## <a name="tsv-input-fields"></a>TSV giriş alanları

|TSV giriş dosya alanları|Notlar|
|--|--|
|KBID|KB Kimliğiniz Yayımlama sayfasında bulunur.|
|Soru|Kullanıcının gireceği soru.|
|Meta veri etiketleri|isteğe bağlı|
|Üst parametre|isteğe bağlı| 
|Beklenen yanıt kimliği|isteğe bağlı|

![Toplu işlem için TSV dosyası için giriş biçimi.](media/batch-test/input-tsv-format-batch-test.png)

## <a name="tsv-output-fields"></a>TSV çıkış alanları 

|TSV Çıkış dosya parametreleri|Notlar|
|--|--|
|KBID|KB Kimliğiniz Yayımlama sayfasında bulunur.|
|Soru|Giriş dosyasından girilen soru.|
|Yanıt|Bilgi tabanınızdan en iyi cevap.|
|Yanıt Kimliği|Yanıt Kimliği|
|Puan|Cevap için tahmin puanı. |
|Meta veri etiketleri|döndürülen yanıtla ilişkili|
|Beklenen yanıt kimliği|isteğe bağlı (yalnızca beklenen yanıt kimliği verildiğinde)|
|Yargı etiketi|isteğe bağlı, değerler olabilir: doğru veya yanlış (sadece beklenen yanıt verildiğinde)|
