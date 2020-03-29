---
title: Tasarım bilgi tabanı - QnA Maker
description: QnA Maker bilgi tabanı, her QnA çiftiyle ilişkili bir dizi soru-cevap (QnA) kümesi ve isteğe bağlı meta verilerden oluşur.
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: cb71de0f6a3e372d8c1c1fede67ba0c0354532ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76844334"
---
# <a name="question-and-answer-set-concepts"></a>Soru-cevap seti kavramları

Bir bilgi tabanı soru-cevap (QnA) kümelerinden oluşur.  Her kümenin bir yanıtı vardır ve bir küme bu _yanıtla_ilişkili tüm bilgileri içerir. Bir yanıt gevşek bir veritabanı satırı veya veri yapısı örneği benzer olabilir.

## <a name="question-and-answer-sets"></a>Soru ve yanıt kümeleri

Soru-cevap (QnA) kümesinde **gerekli** ayarlar şunlardır:

* bir **soru** - qnA Maker'ın makine öğreniminde kullanılan kullanıcı sorgu metni, kullanıcının sorusunun metniyle farklı ifadelerle hizalamak için ama aynı cevap
* **cevap** - setin yanıtı, kullanıcı sorgusu ilişkili soruyla eşleştiğinde döndürülen yanıttır

Her küme bir **kimlikle**temsil edilir.

Bir küme için **isteğe bağlı** ayarlar şunlardır:

* **Sorunun alternatif formları** - Bu QnA Maker soru tümceleri daha geniş bir yelpazede için doğru yanıtı dönmek yardımcı olur
* **Meta veriler**: Meta veriler, QnA çiftiyle ilişkili etiketlerdir ve anahtar değer çiftleri olarak temsil edilir. Meta veri etiketleri QnA çiftlerini filtrelemek ve sorgu eşleştirmesinin gerçekleştirildiği kümeyi sınırlamak için kullanılır.
* **Çok dönüşlü istemler,** çok döndürülme devam etmek için kullanılır

![QnA Maker bilgi üsleri](../media/qnamaker-concepts-knowledgebase/knowledgebase.png)

## <a name="editorially-add-to-knowledge-base"></a>Editoryal bilgi tabanına eklemek

Bilgi tabanını doldurmak için önceden varolan içeriğiniz yoksa, QnA Maker portalına editoryal olarak QnA kümeleri ekleyebilirsiniz. Bilgi tabanınızı nasıl güncelleştirteceklerinizi [buradan](../How-To/edit-knowledge-base.md)öğrenin.

## <a name="editing-your-knowledge-base-locally"></a>Bilgi tabanınızı yerel olarak düzenleme

Bir bilgi tabanı oluşturulduktan sonra, yerel dosyalar üzerinden dışa aktarma ve yeniden alma yerine [QnA Maker portalındaki](https://qnamaker.ai)bilgi bankası metninde yapılan ları yapmanız önerilir. Ancak, yerel olarak bir bilgi tabanını düzenlilmeye ihtiyaç duyduğunuz zamanlar olabilir.

Bilgi tabanını **Ayarlar** sayfasından dışa aktarın ve ardından bilgi tabanını Microsoft Excel ile birlikte edin. Dışa aktarılan dosyanızı yeniden kullanmak için başka bir uygulama kullanmayı seçerseniz, uygulama tam TSV uyumlu olmadığından sözdizimi hatalarına neden olabilir. Microsoft Excel'in TSV dosyaları genellikle herhangi bir biçimlendirme hatası getirmez.

Yaptığınız da, tsv dosyanızı **Ayarlar** sayfasından yeniden içe aktarın. Bu tamamen ithal bilgi tabanı ile geçerli bilgi tabanı yerini alacaktır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [QnA Maker'da bilgi tabanı yaşam döngüsü](./development-lifecycle-knowledge-base.md)