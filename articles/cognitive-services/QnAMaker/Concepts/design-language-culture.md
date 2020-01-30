---
title: Dil Soru-Cevap Oluşturma için tasarım
description: Soru-Cevap Oluşturma kaynağı ve bu kaynaktaki tüm bilgi tabanları, tek bir dili destekler. Tek dil, bir sorgunun en iyi yanıt sonuçlarını sağlamak için gereklidir.
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: 5cb1dcd35649debbafd2e234606ad4c9d6906ea6
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76843429"
---
# <a name="design-knowledge-base-for-content-language"></a>İçerik dili için Bilgi Bankası tasarlama

Soru-Cevap Oluşturma kaynağı ve bu kaynaktaki tüm bilgi tabanları, tek bir dili destekler. Tek dil, bir sorgunun en iyi yanıt sonuçlarını sağlamak için gereklidir.

## <a name="single-language-per-resource"></a>Kaynak başına tek dil

Dil desteğiyle ilgili Soru-Cevap Oluşturma dikkat edilecek noktalar:

* Bir Soru-Cevap Oluşturma Hizmeti ve tüm bilgi tabanları, yalnızca bir dili destekler.
* Hizmetin ilk bilgi tabanı oluşturulduğunda dil açıkça ayarlanır
* Dil, Bilgi Bankası oluşturulduğunda eklenen dosyalardan ve URL 'lerden belirlenir
* Hizmet, hizmette diğer bilgi tabanları için değiştirilemez
* Dil, bir sorguya en iyi yanıtı oluşturmak için Bilişsel Arama hizmeti (Ranker #1) ve Soru-Cevap Oluşturma Hizmeti (Ranker #2) tarafından kullanılır

## <a name="supporting-multiple-languages"></a>Birden çok dili destekleme

Birkaç dil içeren bir Bilgi Bankası sistemini desteklemeniz gerekiyorsa, aşağıdaki yöntemlerden birini seçebilirsiniz:

* Soruyu bilgi tabanınızı göndermeden önce bir soruyu tek bir dile çevirmek için [çeviri metin hizmetini](../../translator/translator-info-overview.md) kullanın. Bu, tek bir dilin kalitesine ve diğer soruların ve yanıtların kalitesine odaklanabilmenizi sağlar.
* Her dil için bu kaynak içindeki bir Soru-Cevap Oluşturma kaynağı ve Bilgi Bankası oluşturun. Bu, ayrı alternatif soruları yönetmenize ve her bir dil için daha fazla nuılmış metin elde etmenizi sağlar. Bu, çok daha fazla esneklik sağlar ancak sorular veya yanıtlar tüm dillerde değiştiğinde çok daha yüksek bir bakım maliyeti gerektirir.

Soru-Cevap Oluşturma için [desteklenen dilleri](../overview/language-support.md) gözden geçirin.

### <a name="support-each-language-with-a-qna-maker-resource"></a>Soru-Cevap Oluşturma kaynağıyla her dili destekleme

* Her dil için bir Soru-Cevap Oluşturma kaynağı oluşturma
* Yalnızca bu dilin dosyalarını ve URL 'Lerini ekleyin
* Dili tanımlamak için kaynak için bir adlandırma kuralı kullanın. Fransız belgelerinin tüm Bilgi Bankası bilgileri için bir örnek `qna-maker-fr`

## <a name="next-steps"></a>Sonraki adımlar

Bilgi bankasını bir yanıt için sorgulama hakkında [Kavramlar](query-knowledge-base.md) öğrenin.