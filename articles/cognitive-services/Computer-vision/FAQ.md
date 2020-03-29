---
title: Sık sorulan sorular - BilgisayarLı Vizyon
titleSuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmetler'de BilgisayarLı Vizyon API'si hakkında sık sorulan soruların yanıtlarını alın.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68564595"
---
# <a name="computer-vision-api-frequently-asked-questions"></a>Bilgisayarlı Vizyon API Sık Sorulan Sorular

> [!TIP]
> Bu SSS'de sorularınıza yanıt bulamıyorsanız, [StackOverflow'daki](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) Computer Vision API topluluğuna sormayı deneyin veya [UserVoice'ta Yardım ve Destek'e](https://cognitive.uservoice.com/) başvurun

---

**Soru**: *Computer Vision API'yi özel etiketleri kullanmak üzere eğitebilir miyim?  Örneğin, AI'yi 'eğitmek' için kedi ırklarının resimleriyle beslenmek, sonra bir AI isteği üzerine cins değerini almak istiyorum.*

**Cevap**: Bu işlev şu anda kullanılamıyor. Ancak mühendislerimiz bu işlevselliği Computer Vision'a getirmek için çalışmaktadır.

---

**Soru**: *Bilgisayar Lıgdato internet bağlantısı olmadan yerel olarak kullanılabilir mi?*

**Cevap**: Şu anda şirket içi veya yerel bir çözüm sunmuyoruz.

---

**Soru**: *Bilgisayarlı Görme plakaları okumak için kullanılabilir mi?*

**Cevap**: Vision API, OCR ile iyi metin algılama sunar, ancak şu anda plakalar için optimize edilmedi. Hizmetlerimizi sürekli olarak geliştirmeye çalışıyoruz ve otomatik plaka tanıma için OCR'yi özellik istekleri listemize ekledik.

---

**Soru**: *El yazısı tanıma için ne tür yazı yüzeyleri desteklenir?*

**Cevap**: Teknoloji, beyaz tahtalar, beyaz kağıt ve sarı yapışkan notlar da dahil olmak üzere farklı yüzeylerle çalışır.

---

**Soru**: *El yazısı tanıma işlemi ne kadar sürer?*

**Cevap**: Aldığı süre metnin uzunluğuna bağlıdır. Daha uzun metinler için birkaç saniye kadar sürebilir. Bu nedenle, El Yazısı Metni Tanı işlemi tamamlandıktan sonra, El YazısıYla Metin İşlemi Sonucu Al işlemini kullanarak sonuçları alabilmeniz için beklemeniz gerekebilir.

---

**Soru**: *El yazısı tanıma teknolojisi, satırın ortasına bir basamak kullanılarak eklenen metni nasıl işler?*

**Cevap**: Bu tür metin el yazısı tanıma işlemi ile ayrı bir satır olarak döndürülür.

---

**Soru**: *El yazısı tanıma teknolojisi, çapraz lanmış kelimeleri veya çizgileri nasıl işler?*

**Cevap**: Sözcüklertanınmaz hale getirmek için birden fazla satırla çizilirse, el yazısı tanıma işlemi onları almaz. Ancak, sözcükler tek bir satır kullanılarak çizilirse, bu geçiş gürültü olarak kabul edilir ve sözcükler yine de el yazısı tanıma işlemi tarafından alınır.

---

**Soru**: *El yazısı tanıma teknolojisi için hangi metin oryantasyonları desteklenir?*

**Cevap**: El yazısı tanıma işlemi ile yaklaşık 30 derece ile 40 dereceye kadar açılarla yönlendirilen metin alınabilir.

---
