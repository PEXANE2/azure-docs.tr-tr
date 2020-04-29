---
title: Tasarım Bilgi Bankası-Soru-Cevap Oluşturma
description: Soru-Cevap Oluşturma Bilgi Bankası, her QnA çiftiyle ilişkili bir soru-cevap (QnA) kümesi ve isteğe bağlı meta veri kümesinden oluşur.
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: 1d24434d1343f4174cfbfeb3a30c36737b213168
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80756792"
---
# <a name="question-and-answer-set-concepts"></a>Soru ve yanıt kümesi kavramları

Bilgi Bankası, soru ve yanıt (QnA) kümelerinden oluşur.  Her küme bir yanıta sahiptir ve bir küme, bu _Yanıt_ile ilişkili tüm bilgileri içerir. Bir yanıt, bir veritabanı satırına veya bir veri yapısı örneğine benzeyebilirler.

## <a name="question-and-answer-sets"></a>Soru ve yanıt kümeleri

Bir soru-cevap (QnA) kümesindeki **gerekli** ayarlar şunlardır:

* farklı bir metinle, ancak aynı Yanıt ile Kullanıcı sorusunun metniyle hizalamak için makine öğrenimine Soru-Cevap Oluşturma için kullanılan Kullanıcı sorgusunun **soru** metni.
* **Yanıt** -kümenin yanıtı, bir Kullanıcı sorgusu ilişkili soru ile eşleştiğinde döndürülen yanıttır

Her küme bir **kimlik**ile temsil edilir.

Bir küme için **isteğe bağlı** ayarlar şunlardır:

* **Sorunun alternatif biçimleri** -bu, soru-cevap oluşturma çok çeşitli soru açısından doğru yanıtı döndürmenize yardımcı olur
* **Meta**veri: meta veriler bir QNA çiftiyle ilişkili etiketlerdir ve anahtar-değer çiftleri olarak temsil edilir. Meta veri etiketleri, QnA çiftlerini filtrelemek ve sorgu eşleştirmesinin gerçekleştirileceği kümeyi sınırlamak için kullanılır.
* Çoklu çift **istemler**, çok yönlü bir konuşmaya devam etmek için kullanılır

![Bilgi temellerini Soru-Cevap Oluşturma](../media/qnamaker-concepts-knowledgebase/knowledgebase.png)

## <a name="editorially-add-to-knowledge-base"></a>Bilgi tabanına genel olarak ekle

Bilgi bankasını doldurmak için önceden mevcut içeriğiniz yoksa, Soru-Cevap Oluşturma portalına genel olarak QnA çiftleri ekleyebilirsiniz. Bilgi bankaınızı [buradan](../How-To/edit-knowledge-base.md)güncelleştirmeyi öğrenin.

## <a name="editing-your-knowledge-base-locally"></a>Bilgi bankaınızı yerel olarak Düzenle

Bilgi Bankası oluşturulduktan sonra, yerel dosyalar aracılığıyla dışarı aktarmak ve yeniden içeri aktarmak yerine [soru-cevap oluşturma portalındaki](https://qnamaker.ai)Bilgi Bankası metninde düzenleme yapmanız önerilir. Ancak, bir bilgi bankasını yerel olarak düzenlemeniz gerektiği zamanlar olabilir.

Bilgi Bankası 'nı **Ayarlar** sayfasından dışarı aktarın ve Bilgi Bankası 'Nı Microsoft Excel ile düzenleyin. Verdiğiniz dosyayı düzenlemek için başka bir uygulama kullanmayı seçerseniz, uygulama tam olarak uyumlu olmadığından sözdizimi hataları verebilir. Microsoft Excel 'in TSV dosyaları genellikle biçimlendirme hataları sunmaz.

Düzenlemelerinizi tamamladıktan sonra, **Ayarlar** sayfasından TSV dosyasını yeniden içeri aktarın. Bu işlem, geçerli bilgi bankasını içeri aktarılan Bilgi Bankası ile tamamen değiştirir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma Bilgi Bankası yaşam döngüsü](./development-lifecycle-knowledge-base.md)