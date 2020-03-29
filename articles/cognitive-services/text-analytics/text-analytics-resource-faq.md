---
title: Metin Analizi API'si hakkında sık sorulan sorular
titleSuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmetler için Metin Analizi API'si ile ilgili kavramlar, kodlar ve senaryolarla ilgili sık sorulan soruların yanıtlarını bulun.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: aahi
ms.openlocfilehash: aca3b0bea65923cee1bae25020058666c973922c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73837131"
---
# <a name="frequently-asked-questions-faq-about-the-text-analytics-cognitive-service"></a>Metin Analizi Bilişsel Servisi Hakkında Sık Sorulan Sorular (SSS)

 Azure'da Microsoft Bilişsel Hizmetler için Metin Analizi API'si ile ilgili kavramlar, kodlar ve senaryolarla ilgili sık sorulan soruların yanıtlarını bulun.

## <a name="can-text-analytics-identify-sarcasm"></a>Text Analytics iğnelemeyi tanımlayabilir mi?

Analiz, ruh hali algılamadan çok olumlu-negatif duygular içindir.

Duyarlılık analizinde her zaman bir dereceye kadar yanlışlık vardır, ancak içerikte gizli bir anlam veya alt metin olmadığında model en kullanışlıdır. İroni, alaycılık, mizah ve benzer nüanslı içerik kültürel bağlam ve normlar niyet iletmek için güveniyor. Bu tür içerikanaliz etmek için en zor arasındadır. Tipik olarak, analizör tarafından üretilen belirli bir puan ile bir insan tarafından öznel bir değerlendirme arasındaki en büyük tutarsızlık nüanslı anlamı olan içerik içindir.

## <a name="can-i-add-my-own-training-data-or-models"></a>Kendi eğitim verilerimi veya modellerimi ekleyebilir miyim?

Hayır, modeller önceden eğitilmiş. Yüklenen verilerde kullanılabilen tek işlem puanlama, anahtar cümle çıkarma ve dil algılamadır. Özel modeller barındırmıyoruz. Özel makine öğrenme modelleri oluşturmak ve barındırmak istiyorsanız, [Microsoft R Server'daki makine öğrenimi özelliklerini](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package)göz önünde bulundurun.

## <a name="can-i-request-additional-languages"></a>Ek dil isteyebilir miyim?

Duygu analizi ve anahtar tümcecik çıkarma belirli sayıda [dil](text-analytics-supported-languages.md)için kullanılabilir. Doğal dil işleme karmaşıktır ve yeni işlevsellik yayımlanmadan önce önemli testler gerektirir. Bu nedenle, kimsenin olgunlaşmak için daha fazla zamana ihtiyaç darayan işlevselliğe bağımlı olmaması için önceden duyurulmasını önleyen destekten kaçınırız. 

Bir sonraki üzerinde çalışacağımız dillere öncelik vermemize yardımcı olmak için, [Kullanıcı Sesi'nde](https://cognitive.uservoice.com/forums/555922-text-analytics)belirli dilleriçin oy verin. 

## <a name="why-does-key-phrase-extraction-return-some-words-but-not-others"></a>Neden anahtar tümcecik çıkarma bazı sözcükleri döndürmez, diğerlerini döndürmez?

Anahtar tümcecik çıkarma, gerekli olmayan sözcükleri ve bağımsız sıfatları ortadan kaldırır. "Muhteşem görünümler" veya "sisli hava" gibi sıfat-isim kombinasyonları birlikte döndürülür.

Genellikle, çıktı cümlenin isimleri ve nesnelerinden oluşur. Çıktı önem sırasına göre listelenir ve ilk tümcecik en önemli olandır. Önem, belirli bir kavramın kaç kez bahsedildiği veya bu öğenin metindeki diğer öğelerle ilişkisi ile ölçülür.

## <a name="why-does-output-vary-given-identical-inputs"></a>Aynı girdiler göz önüne alındığında çıktı neden değişir?

Değişiklik büyükse veya güncelleştirme küçükse sessizce hizmete aktarılıyorsa, modellerde ve algoritmalarda yapılan iyileştirmeler duyurulur. Zamanla, aynı metin girişinin farklı bir duyarlılık puanı veya anahtar tümcecik çıktısı ile sonuçverdiğini görebilirsiniz. Bu, bulutta yönetilen makine öğrenimi kaynaklarını kullanmanın normal ve kasıtlı bir sonucudur.

## <a name="next-steps"></a>Sonraki adımlar

Sorunuz eksik bir özellik veya işlevsellik hakkında mı? [UserVoice web sitemizde](https://cognitive.uservoice.com/forums/555922-text-analytics)talep etmeyi veya oy vermeyi düşünün.

## <a name="see-also"></a>Ayrıca bkz.

 [StackOverflow: Metin Analizi API](https://stackoverflow.com/questions/tagged/text-analytics-api)   
 [StackOverflow: Bilişsel Hizmetler](https://stackoverflow.com/questions/tagged/microsoft-cognitive)
