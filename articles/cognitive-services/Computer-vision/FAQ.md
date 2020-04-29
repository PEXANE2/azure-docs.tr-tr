---
title: Sık sorulan sorular-Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: Azure bilişsel hizmetler 'deki Görüntü İşleme API'si hakkında sık sorulan soruların yanıtlarını alın.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: e8ab205a43e5cb1e8e2b96dbd9600e1fceb29403
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "68564595"
---
# <a name="computer-vision-api-frequently-asked-questions"></a>Görüntü İşleme API'si sık sorulan sorular

> [!TIP]
> Bu SSS 'de sorularınıza yanıt bulamazsanız, [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) 'de görüntü işleme API'si Community 'yi sormayı deneyin veya [UserVoice üzerinde yardım ve desteğe](https://cognitive.uservoice.com/) başvurun

---

**Soru**: *görüntü işleme API'si özel etiketleri kullanmak için eğitebilir miyim?  Örneğin, kedi breeds resimlerinde AI ' eğitme ' ' i ' eğitmek ' ve ardından bir AI isteğinde iyisi değerini almak istiyorum.*

**Cevap**: Bu işlev şu anda kullanılamıyor. Ancak, mühendislerimiz bu işlevselliği Görüntü İşleme getirmek için çalışmaktadır.

---

**Soru**: *İnternet bağlantısı olmadan yerel olarak görüntü işleme kullanılabilir mi?*

**Cevap**: Şu anda şirket içi veya yerel bir çözüm sunmuyoruz.

---

**Soru**: *Lisans levhalarını okumak için görüntü işleme kullanılabilir mi?*

**Cevap**: Vision API 'SI, OCR ile iyi metin algılama olanağı sunar, ancak şu anda lisans levhaları için iyileştirilmez. Hizmetlerimizi sürekli olarak iyileştirmeye çalışıyoruz ve özellik istekleri listemize otomatik lisans levha tanıma için OCR ekledik.

---

**Soru**: *el yazısı tanıma için hangi tür yazma yüzeyleri desteklenir?*

**Cevap**: teknoloji, beyaz tahta, Teknik İnceleme ve sarı yapışkan notlar gibi farklı türlerde yüzeyler ile birlikte kullanılır.

---

**Soru**: *el yazısı tanıma işlemi ne kadar sürer?*

**Cevap**: aldığı sürenin miktarı metnin uzunluğuna bağlıdır. Uzun metinler için birkaç saniye sürebilir. Bu nedenle, El Ile yazılmış metin işlemi tamamlandıktan sonra, el yazısı metin Işlem sonucunu Al işlemini kullanarak sonuçları alabilmek için beklemeniz gerekebilir.

---

**Soru**: *el yazısı tanıma teknolojisi, bir satır ortasında bir giriş işareti kullanılarak eklenen metni nasıl işler?*

**Cevap**: Bu tür metinler, el yazısı tanıma işlemi tarafından ayrı bir satır olarak döndürülür.

---

**Soru**: *el yazısı tanıma teknolojisi, çapraz veya satırları nasıl işler?*

**Cevap**: sözcüklerin tanınabilmesi için birden çok satır varsa, el yazısı tanıma işlemi bunları kullanıma almaz. Ancak, sözcükler tek bir çizgi kullanılarak ele alınalıyorsa, bu geçiş gürültü olarak kabul edilir ve kelimeler yine de el yazısı tanıma işlemi tarafından alınır.

---

**Soru**: *el yazısı tanıma teknolojisi için hangi metin yönleri desteklenir?*

**Cevap**: en fazla 30 derece 40 dereceye kadar olan metin yönelimli metinler, el yazısı tanıma işlemi tarafından alınmayabilir.

---
