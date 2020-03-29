---
title: Dil için tasarım - QnA Maker
description: QnA Maker kaynağı ve bu kaynağın içindeki tüm bilgi tabanları tek bir dili destekler. Tek dil, bir sorgu için en iyi yanıt sonuçlarını sağlamak için gereklidir.
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: 5cb1dcd35649debbafd2e234606ad4c9d6906ea6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76843429"
---
# <a name="design-knowledge-base-for-content-language"></a>İçerik dili için tasarım bilgi tabanı

QnA Maker kaynağı ve bu kaynağın içindeki tüm bilgi tabanları tek bir dili destekler. Tek dil, bir sorgu için en iyi yanıt sonuçlarını sağlamak için gereklidir.

## <a name="single-language-per-resource"></a>Kaynak başına tek dil

Dil desteği için QnA Maker hususlar:

* Bir QnA Maker hizmeti ve tüm bilgi tabanları yalnızca bir dili destekler.
* Hizmetin ilk bilgi tabanı oluşturulduğunda dil açıkça ayarlanır
* Dil, bilgi tabanı oluşturulduğunda eklenen dosyalardan ve URL'lerden belirlenir
* Dil, hizmetteki diğer bilgi üsleri için değiştirilemez
* Dil, bir sorguya en iyi yanıtı oluşturmak için Bilişsel Arama hizmeti (#1 ranker) ve QnA Maker hizmeti (ranker #2) tarafından kullanılır

## <a name="supporting-multiple-languages"></a>Birden çok dili destekleme

Birkaç dil içeren bir bilgi tabanı sistemini desteklemeniz gerekiyorsa, aşağıdaki yöntemlerden birini seçebilirsiniz:

* Soruyu bilgi tabanınıza göndermeden önce soruyu tek bir dile çevirmek için [Çeviri Metni hizmetini](../../translator/translator-info-overview.md) kullanın. Bu, tek bir dilin kalitesine ve alternatif soru ve yanıtların kalitesine odaklanmanızı sağlar.
* Her dil için bir QnA Maker kaynağı ve bu kaynağın içinde bir bilgi tabanı oluşturun. Bu, ayrı ayrı alternatif soruları yönetmenize ve her dil için daha nüanslı metni yanıtlamanıza olanak tanır. Bu size çok daha fazla esneklik sağlar, ancak sorular veya yanıtlar tüm dillerde değiştiğinde çok daha yüksek bakım maliyeti gerektirir.

QnA Maker için [desteklenen dilleri](../overview/language-support.md) gözden geçirin.

### <a name="support-each-language-with-a-qna-maker-resource"></a>Her dili bir QnA Maker kaynağıyla destekleyin

* Her dil için bir QnA Maker kaynağı oluşturma
* Yalnızca o dil için dosya ve URL ekleme
* Dili tanımlamak için kaynak için bir adlandırma kuralı kullanın. Bir örnek `qna-maker-fr` Fransızca belgeler için tüm bilgi üsleri için

## <a name="next-steps"></a>Sonraki adımlar

Bir yanıt için bilgi tabanını sorgulama hakkında [kavramlar](query-knowledge-base.md) öğrenin.