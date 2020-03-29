---
title: Cümle eşleştirme ve hizalama - Özel Çevirmen
titleSuffix: Azure Cognitive Services
description: Eğitim infazı sırasında, paralel belgelerde bulunan cümleler eşlenir veya hizalanır. Custom Translator, çevirileri bir cümleyi okuyarak, bu cümlenin çevirisini öğrenir. Daha sonra bu iki cümledeki sözcükleri ve tümcecikleri birbirine hizalar.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: cf5b2b84142c9104ea5b3afa3ad179fd0ec07449
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80370130"
---
# <a name="sentence-pairing-and-alignment-in-parallel-documents"></a>Paralel belgelerde cümle eşleştirme ve hizalama

Eğitim sırasında, paralel belgelerde bulunan cümleler eşlenir veya hizalanır. Özel Çevirmen, veri kümelerinin her birinde Hizalanmış Cümleler olarak eşleştirebildiği cümle sayısını bildirir.

## <a name="pairing-and-alignment-process"></a>Eşleştirme ve hizalama işlemi

Custom Translator cümlelerin çevirilerini her seferinde bir cümle öğrenir. Bir cümleyi kaynaktan okur, sonra da bu cümlenin hedeften çevirisini. Daha sonra bu iki cümledeki sözcükleri ve tümcecikleri birbirine hizalar. Bu işlem, bu cümlenin çevirisindeki eşdeğer sözcük ve tümceciklere bir cümle ve tümceciklerin bir haritasını oluşturmasını sağlar. Hizalama, sistemin birbirinin çevirisi olan cümleler üzerinde eğitilmesini sağlamaya çalışır.

## <a name="pre-aligned-documents"></a>Önceden hizalanmış belgeler

Paralel belgeleriniz olduğunu biliyorsanız, önceden hizalanmış metin dosyaları sağlayarak cümle hizalamasını geçersiz kılabilirsiniz. Her iki belgedeki tüm cümleleri metin dosyasına ayıklayabilir, satır `.align` başına bir cümle düzenleyebilir ve uzantıyla yükleyebilirsiniz. Uzantı, Özel Çevirmen'e `.align` cümle hizalamasını atlaması gerektiğini bildirir.

En iyi sonuçlar için, dosyalarınızda satır başına bir cümle olduğundan emin olun.Bu kötü hizalamalara neden olacak gibi bir cümle içinde yeni satır karakterleri yok.

## <a name="suggested-minimum-number-of-sentences"></a>Önerilen en az cümle sayısı

Bir eğitimin başarılı olabilmek için, aşağıdaki tabloda her belge türünde gereken en az cümle sayısı gösterilmektedir.Bu sınırlama, paralel cümlelerinizin bir çeviri modelini başarılı bir şekilde eğitecek kadar benzersiz kelime bilgisi içerdiğinden emin olmak için bir güvenlik ağıdır. Genel kılavuz insan çeviri kalitesinde daha fazla etki alanı paralel cümleler daha kaliteli modeller üretmek olmalıdır yaşıyor.

| Belge türü   | Önerilen minimum cümle sayısı | Maksimum cümle sayısı |
|------------|--------------------------------------------|--------------------------------|
| Eğitim   | 10,000                                     | Üst sınır yok                 |
| Ayarlama     | 500                                      | 2,500       |
| Sınama    | 500                                      | 2,500  |
| Sözlük | 0                                          | Üst sınır yok                 |

> [!NOTE]
> - Eğitim için en az 10.000 ceza sayısı yerine getirilmezse eğitim başlamayacak ve başarısız olacaktır. 
> - Tuning ve Test isteğe bağlıdır. Bunları sağlamazsanız, sistem doğrulama ve sınama için kullanılmak üzere Eğitimden uygun bir yüzdeyi kaldırır. 
> - Yalnızca sözlük verilerini kullanarak bir modeli eğitebilirsiniz. Sözlük [Nedir'e](https://docs.microsoft.com/azure/cognitive-services/translator/custom-translator/what-is-dictionary)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Özel Çevirmen'de [sözlük](what-is-dictionary.md) kullanmayı öğrenin.
