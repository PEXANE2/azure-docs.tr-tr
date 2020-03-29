---
title: Sözlük nedir? - Özel Çevirmen
titleSuffix: Azure Cognitive Services
description: Sözlük, Microsoft Translator'ın her zaman aynı şekilde çevirmesini istediğiniz tümceciklerin veya cümlelerin (ve çevirilerinin) listesini belirten hizalanmış bir belgedir. Sözlükler bazen sözlükler veya terim tabanları olarak da adlandırılır.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 36b449c4c4ca30eb658c9519ce8e870a4f1fab32
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "71970736"
---
# <a name="what-is-a-dictionary"></a>Sözlük nedir?

Sözlük, tümceciklerin veya tümcelerin listesini ve bunların karşılık gelen çevirilerini belirten hizalanmış bir belge çiftidir. Microsoft Translator'ın sözlükte sağladığınız çeviriyi kullanarak kaynak tümcecik veya tümcenin herhangi bir örneğini her zaman çevirmesini istediğinizde, eğitiminizde bir sözlük kullanın. Sözlükler bazen sözlükler veya terim tabanları olarak adlandırılır. Sözlüğü, listelediğiniz tüm terimler için kaba bir kuvvet "kopyala ve değiştir" olarak düşünebilirsiniz. Ayrıca, Microsoft Custom Translator hizmeti, çevirikalitesini artırmak için kendi genel amaçlı sözlüklerini oluşturur ve kullanır. Ancak, sağlanan bir müşteri sözlük emsal alır ve ilk arama kelimeleri veya cümleler aranacaktır.

Sözlükler yalnızca, arkalarında tam olarak desteklenen bir Microsoft genel sinir ağı modeline sahip dil çiftleri projelerinde çalışır. [Dillerin tam listesini görüntüleyin.](https://docs.microsoft.com/azure/cognitive-services/translator/language-support#customization)

## <a name="phrase-dictionary"></a>İfade sözlüğü
Modelinizi eğiten bir tümcecik sözlüğü eklediğinizde, listelenen herhangi bir sözcük veya tümcecik belirttiğiniz şekilde çevrilir. Cümlenin geri kalanı her zamanki gibi tercüme edilir. Sözlükteki kaynak ve hedef dosyada aynı çevrilmemiş tümcecik sağlayarak çevrilmemesi gereken tümcecikleri belirtmek için bir tümcecik sözlüğü kullanabilirsiniz.

## <a name="sentence-dictionary"></a>Tümce sözlüğü
Tümce sözlüğü, kaynak cümle için tam bir hedef çevirisi belirtmenizi sağlar. Bir tümce sözlüğü eşleşmesinin oluşması için gönderilen tümcenin kaynağı sözlük girişiyle eşleşmesi gerekir.  Cümlenin yalnızca bir bölümü eşleşirse, giriş eşleşmez.  Eşleşme algılandığında, cümle sözlüğünün hedef girişi döndürülür.

## <a name="dictionary-only-trainings"></a>Yalnızca sözlük eğitimleri
Yalnızca sözlük verilerini kullanarak bir modeli eğitebilirsiniz. Bunu yapmak için yalnızca eklemek istediğiniz sözlük belgesini (veya birden çok sözlük belgesini) seçin ve Model Oluştur'a dokunun. Bu yalnızca sözlük tesimanı olduğundan, en az sayıda eğitim cümlesi gerekmez. Modeliniz genellikle standart bir eğitimden çok daha hızlı bir şekilde eğitimi tamamlar.  Ortaya çıkan modeller, eklediğiniz sözlüklerin eklenmesiyle çeviri için Microsoft temel modellerini kullanır.  Test raporu alamazsınız.

>[!Note]
>Özel Çevirmen sözlük dosyalarını hizalamaz, bu nedenle sözlük belgelerinizde eşit sayıda kaynak ve hedef tümcecik/cümle olması ve bunların tam olarak hizalanmış olması önemlidir.

## <a name="recommendations"></a>Öneriler

- Sözlükler, eğitim verilerini kullanarak bir modeli eğitimin yerini tutamaz. Bunlardan kaçınmanız ve sistemin eğitim verilerinizden öğrenmesi önerilir. Ancak, cümleler veya bileşik adlar olduğu gibi işlenmeli, bir sözlük kullanın.
- İfade sözlüğü dikkatli kullanılmalıdır. Bu nedenle, bir cümle içindeki bir tümcecik değiştirildiğinde, cümlenin içindeki bağlamın kaybolduğunu veya cümlenin geri kalanını çevirmek için sınırlı olduğunu unutmayın. Sonuç olarak, cümle içindeki tümcecik veya sözcük sağlanan sözlüğe göre tercüme edilirken, cümlenin genel çeviri kalitesi genellikle zarar görecektir.
- İfade sözlüğü, ürün adları ("Microsoft SQL Server"), uygun adlar ("Hamburg Şehri") veya ürünün özellikleri ("pivot table") gibi bileşik adlar için iyi çalışır. Fiiller veya sıfatlar için eşit derecede iyi çalışmaz, çünkü bunlar genellikle kaynakta veya hedef dilde yüksek oranda çekimlidir. En iyi uygulamalar, bileşik adlar hariç her şey için ifade sözlük girişlerinden kaçınmaktır.
- Bir tümcecik sözlüğü kullanırken, büyük harf ve noktalama önemlidir. Sözlük girişleri yalnızca giriş cümlesindeki, kaynak sözlük dosyasında belirtildiği gibi tam olarak aynı büyük harf ve noktalama işaretlerini kullanan sözcük ve tümceciklerle eşleşir. Ayrıca çeviriler, hedef sözlük dosyasında sağlanan büyük harf ve noktalama işaretlerini yansıtacaktır. Örneğin, kaynak dosyada "US" ve "EE" belirten bir ifade sözlüğü kullanan bir İngilizce'yi İspanyolca sistemine eğittiyseniz. UU." hedef dosyasında. "Biz" sözcüğünün (büyük harfle değil) sözcüğünün çevirisini istediğinizde, bu sözlüğe uymaz. Ancak , "US" (büyük harfle) sözcüğünün bulunduğu bir cümlenin çevirisini isterseniz, sözlükle eşleşir ve çeviri "EE" içerir. UU." Çevirideki büyük harf ve noktalama işaretlerinin sözlük hedef dosyasında belirtilenden farklı olabileceğini ve kaynaktaki büyük harf ve noktalama işaretlerinden farklı olabileceğini unutmayın. Hedef dilin kurallarına uyar.
- Bir sözcük sözlük dosyasında birden fazla kez görünürse, sistem her zaman sağlanan son girişi kullanır. Bu nedenle, sözlüğünüz aynı sözcüğün birden çok çevirisini içermemelidir.

## <a name="next-steps"></a>Sonraki adımlar

- Belge [biçimleriyle ilgili yönergeler](document-formats-naming-convention.md)hakkında bilgi edinin.
