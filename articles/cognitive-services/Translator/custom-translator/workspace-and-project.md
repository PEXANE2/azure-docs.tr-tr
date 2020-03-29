---
title: Çalışma alanı ve proje nedir? - Özel Çevirmen
titleSuffix: Azure Cognitive Services
description: Bu makalede, bir çalışma alanı ve proje arasındaki farkların yanı sıra Özel Çevirmen hizmeti için proje kategorileri ve etiketleri açıklanacaktır.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: d2f7903fa85c645357e46a753d1cb043e0893254
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219439"
---
# <a name="what-is-a-custom-translator-workspace"></a>Özel Çevirmen çalışma alanı nedir?

Çalışma alanı, özel çeviri sisteminizi oluşturmak ve oluşturmak için bir çalışma alanıdır. Çalışma alanı birden çok proje, model ve belge içerebilir. Custom Translator'da yaptığınız tüm işler belirli bir çalışma alanının içindedir.

Çalışma alanı sizin ve çalışma alanınıza davet ettiğiniz kişiler için özeldir. Davetsiz kişilerin çalışma alanınızın içeriğine erişimi yoktur. İstediğiniz kadar kişiyi çalışma alanınıza davet edebilir ve istediğiniz zaman bu kişileri değiştirebilir veya kaldırabilirsiniz. Ayrıca yeni bir çalışma alanı da oluşturabilirsiniz. Varsayılan olarak, bir çalışma alanı diğer çalışma alanlarınızın içinde bulunan herhangi bir proje veya belge içermez.

## <a name="what-is-a-custom-translator-project"></a>Özel Çevirmen projesi nedir?

Proje, bir model, belge ve testler için bir paketleyicidir. Her proje, doğru dil çiftine sahip olan çalışma alanına yüklenen tüm belgeleri otomatik olarak içerir. Örneğin, hem İngilizce'den İspanyolca'ya hem de İspanyolca'dan İngilizce'ye bir projeniz varsa, her iki projeye de aynı belgeler eklenecektir. Her projenin, çeviriler için [V3 API'sini](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl) sorgularken kullanılan bir CategoryID'si vardır. CategoryID, Özel Çevirmen ile oluşturulmuş özelleştirilmiş bir sistemden çeviri almak için kullanılan parametredir.

## <a name="project-categories"></a>Proje kategorileri

Kategori, projeniz için etki alanını (terminoloji alanı ve kullanmak istediğiniz stil) tanımlar. Belgelerinizle en alakalı kategoriyi seçin. Bazı durumlarda, kategori seçiminiz Özel Çevirmenin davranışını doğrudan etkiler.

İki ana çizgi modelimiz var. Onlar Genel ve Teknoloji vardır. **Teknoloji** kategorisi seçilirse, Teknoloji temel modelleri kullanılır. Diğer kategori seçimleri için Genel taban çizgisi modelleri kullanılır. Teknoloji temel modeli teknoloji etki alanında iyi yapar, ancak çeviri için kullanılan cümleler teknoloji etki alanına girmiyorsa, daha düşük kalite gösterir. Müşterilerin yalnızca cümleler yalnızca teknoloji etki alanı içinde tamamen yer alırsa, kategori Teknolojisi'ni seçmelerini öneririz.

Aynı çalışma alanında, farklı kategorilerde aynı dil çifti için projeler oluşturabilirsiniz. Custom Translator, aynı dil çifti ve kategorisine sahip yinelenen bir projenin oluşturulmasını engeller. Projenize etiket uygulamak, bu kısıtlamayı önlemenize olanak tanır. Projenize benzersiz bir etiket eklenmesi projectCATEGORYID'e yansıyacağı için, birden çok istemci için çeviri sistemleri oluşturmadığınız sürece etiketleri kullanmayın.

## <a name="project-labels"></a>Proje etiketleri

Özel Çevirmen, projenize bir proje etiketi atamanızı sağlar. Proje etiketi, aynı dil çifti ve kategorisine sahip birden çok projeyi birbirinden ayırır. En iyi uygulama olarak, gerekli olmadıkça proje etiketlerini kullanmaktan kaçının.

Proje etiketi CategoryID'nin bir parçası olarak kullanılır. Proje etiketi ayarlanmamış sayılsa veya projeler arasında aynı şekilde ayarlanırsa, aynı kategoriye ve *farklı* dil çiftlerine sahip projeler aynı CategoryID'yi paylaşır. Bu yaklaşım avantajlıdır, çünkü sizin veya müşterinizin Metin Çevirmeni API'sini kullanırken her projeye özgü bir CategoryID endişesi duymadan diller arasında geçiş yapmasına olanak tanır.

Örneğin, Teknoloji alanında İngilizce'den Fransızca'ya ve Fransızca'dan İngilizce'ye çevirileri etkinleştirmek isteseydim,\> biri İngilizce -\> Fransızca, diğeri Fransızca - İngilizce olmak üzere iki proje oluştururdum. Her ikisi için de aynı kategoriyi (Teknoloji) belirtir ve proje etiketini boş bırakırım. Her iki proje için CategoryID eşleşir, bu nedenle CategoryID'mi değiştirmek zorunda kalmadan hem İngilizce hem de Fransızca çeviriler için API sorgulayabilirim.

Bir dil hizmeti sağlayıcısıysanız ve aynı kategoriyi ve dil çiftini koruyan farklı modellerle birden fazla müşteriye hizmet vermek istiyorsanız, müşteriler arasında ayrım yapmak için bir proje etiketi kullanmak akıllıca bir karar olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- Bir çeviri modelinin verimli bir şekilde nasıl oluşturacağını bilmek için [Eğitim ve model](training-and-model.md) hakkında bilgi edinin.
