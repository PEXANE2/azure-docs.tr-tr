---
title: Tasarım Bilgi Bankası-Soru-Cevap Oluşturma
description: Soru-Cevap Oluşturma Bilgi Bankası, her QnA çiftiyle ilişkili bir soru-cevap (QnA) çiftleri ve isteğe bağlı meta veri kümesi içerir.
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: e75a9313f02a6b5cc4235cdcccb381585d2192ae
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83993006"
---
# <a name="question-and-answer-pair-concepts"></a>Soru ve yanıt çifti kavramları

Bilgi Bankası, soru ve yanıt (QnA) çiftlerinden oluşur.  Her çiftin bir yanıtı vardır ve bir çift, bu _Yanıt_ile ilişkili tüm bilgileri içerir. Bir yanıt, bir veritabanı satırına veya bir veri yapısı örneğine benzeyebilirler.

## <a name="question-and-answer-pairs"></a>Soru ve yanıt çiftleri

Soru-cevap (QnA) çiftinde **gerekli** ayarlar şunlardır:

* farklı bir metinle, ancak aynı Yanıt ile Kullanıcı sorusunun metniyle hizalamak için makine öğrenimine Soru-Cevap Oluşturma için kullanılan Kullanıcı sorgusunun **soru** metni.
* **Yanıt** -çiftin yanıtı, bir Kullanıcı sorgusu ilişkili soru ile eşleştiğinde döndürülen yanıttır

Her çift bir **kimlik**ile temsil edilir.

Bir çiftin **isteğe bağlı** ayarları şunlardır:

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