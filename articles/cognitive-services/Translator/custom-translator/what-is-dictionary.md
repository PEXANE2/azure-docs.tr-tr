---
title: Sözlük nedir? -Özel Translator
titleSuffix: Azure Cognitive Services
description: Sözlük, Microsoft Translator 'ın aynı şekilde çevirisini her zaman çevirebilmeniz için bir tümcecik veya tümce (ve bunların çevirileri) listesini belirten hizalanmış bir belgedir. Sözlüklerde aynı zamanda ışlar veya terim esaları da denir.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 5103526956b5041771a1d8e4abb5e8800b971059
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68595378"
---
# <a name="what-is-a-dictionary"></a>Sözlük nedir?

Sözlük, deyimlerin veya Tümcelerin bir listesini ve bunlara karşılık gelen çevirileri belirten, hizalanmış bir belge çiftidir. Microsoft Translator 'ın, sözlükte sağladığınız çeviriyi kullanarak her zaman kaynak tümceciği veya cümle örneklerini çevirmesini istediğinizde, öğreticinizdeki bir sözlüğü kullanın. Sözlüklerde bazen ışlar veya terim tabanı denir. Sözlüğü, listenizin tüm terimleri için "Kopyala ve Değiştir" zorlaması olarak düşünebilirsiniz.

Sözlükler yalnızca kendi arkasındaki tam olarak desteklenen bir Microsoft sinir makine çevirisi (NMT) sistemine sahip olan dil çiftlerindeki projeler için geçerlidir. [Dillerin tüm listesini görüntüleyin](https://docs.microsoft.com/azure/cognitive-services/translator/language-support#customization).

## <a name="phrase-dictionary"></a>Tümcecik sözlüğü
Modelinize eğitim altına bir tümcecik sözlüğü eklediğinizde, listelenen herhangi bir sözcük veya tümcecik, belirttiğiniz şekilde çevrilir. Cümlenin geri kalanı her zamanki gibi çevrilir. Sözlükteki kaynak ve hedef dosyada aynı çevrilmemiş tümceciği sağlayarak çevrilmemesi gereken tümcecikleri belirtmek için bir tümcecik sözlüğü kullanabilirsiniz.

## <a name="sentence-dictionary"></a>Tümce sözlüğü
Tümce sözlüğü, kaynak tümce için tam bir hedef çevirisi belirtmenize olanak tanır. Bir cümle sözlüğü eşleşmesi gerçekleşmesi için, gönderilen tümcenin tamamının kaynak sözlük girdisiyle eşleşmesi gerekir.  Cümlenin yalnızca bir kısmı eşleşiyorsa giriş eşleşmez.  Bir eşleşme algılandığında, tümce sözlüğünün hedef girdisi döndürülür.

## <a name="dictionary-only-trainings"></a>Sözlük-yalnızca insanlar
Yalnızca Sözlük verilerini kullanarak bir modeli eğitebilirsiniz. Bunu yapmak için, dahil etmek istediğiniz Sözlük belgesi (veya birden fazla Sözlük belgesi) seçin ve model oluştur ' a dokunun. Bu yalnızca bir sözlük eğitimi olduğundan, en az sayıda eğitim cümleleri gerekli değildir. Modelinize, genellikle standart bir eğitimin daha hızlı bir şekilde eğitim tamamlanmıştır.  Elde edilen modeller, eklediğiniz sözlüklerin eklenmesiyle çeviri için Microsoft taban çizgisi modellerini kullanacaktır.  Test raporu alacaksınız.

>[!Note]
>Özel çevirici, sözlük dosyalarını tümce halinde hizalamaz, bu nedenle sözlük belgelerinizde eşit sayıda kaynak ve hedef tümcecik/cümle olması ve tam olarak hizalanmaları önemlidir.

## <a name="recommendations"></a>Öneriler

- Sözlükler eğitim verileri olan eğitilen bir modelin yerini değildir.  Sözlükler temelde kelimeleri veya tümceleri bulur ve değiştirir.  Sistemin eğitim malzemenizden tam tümcelerde öğrenmesine izin vermek, genellikle sözlük kullanmaktan daha iyi bir seçimdir.
- Tümcecik sözlüğü gelişigüzel kullanılmalıdır. Tümce içindeki bir ifade değiştirildiğinde, tümcenin geri kalanını çevirmek için bu tümcedeki Bağlam kaybolur veya sınırlıdır. Sonuç, tümce içindeki tümcecik veya sözcük, tümcecik sözlüğüne göre çevirirken, tümcenin genel çeviri kalitesi genellikle zarar görür.
- Tümcecik sözlüğü ürün adları ("Microsoft SQL Server"), uygun adlar ("şehir Hamburg") veya ürünün özellikleri ("Pivot tablosu") gibi bileşik adlarla iyi bir şekilde çalışacaktır. Bunlar genellikle kaynakta veya hedef dilde oldukça büyük olduklarından, fiiller veya sıfatlar için eşit bir şekilde çalışmaz. Tümcecik sözlüğü girişlerinin her şeyi, ancak bileşik isimleri önleyin.
- Bir sözlük kullanılırken, çevirinizdeki büyük harfler ve noktalama işaretleri hedef dosyanızda belirtilen büyük/küçük harfleri yansıtır. Giriş cümlesi ve sözlük dosyanızdaki kaynak cümleler arasındaki eşleşmeleri belirlemeye çalışırken büyük harfler ve noktalama işaretleri yoksayılır. Örneğin, kaynak dosyada "City of Hamburg" ve hedef dosyadaki "CIUDAD de Hamburg" belirtilmiş bir sözlük kullanan bir Ingilizce ve Ispanyolca sistem eğitiyoruz. "City of Hamburg" ifadesini içeren bir tümcenin çevirisini istediğimde, "City of Hamburg" girdisi "City of Hamburg" girişi için sözlük dosyası ile eşleşir ve son çevirimde "CIUDAD de Hamburg" ile eşlenir.
- Bir sözcük, sözlük dosyasında birden çok kez görünürse, sistem her zaman belirtilen son girişi kullanır. Sözlüğünüz aynı kelimenin birden fazla çevirisi içermemelidir.

## <a name="next-steps"></a>Sonraki adımlar

- [Belge biçimlerinde yönergeler](document-formats-naming-convention.md)hakkında bilgi edinin.
