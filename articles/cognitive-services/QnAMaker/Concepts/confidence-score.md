---
title: Güvenirlik puanı-Soru-Cevap Oluşturma
titleSuffix: Azure Cognitive Services
description: Bilgi Bankası 'nın yayımlanması gerekir. Bilgi Bankası, yayımlandıktan sonra, generateAnswer API kullanılarak çalışma zamanı tahmin uç noktasında sorgulanır.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 01/27/2020
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: d901a803311805825c22503af6098e805a67e8f6
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76843461"
---
# <a name="the-confidence-score-of-an-answer"></a>Bir yanıtın Güvenirlik puanı
Kullanıcı sorgusu karşı Bilgi Bankası eşleştiğinde, soru-cevap Oluşturucu bir güven puanı yanı sıra ilgili yanıt verir. Bu puanı güvenle yanıt verilen kullanıcı sorgusu için doğru eşleşme olduğunu gösterir.

Güvenilirlik puanı, 0 ile 100 arasında bir sayıdır. Bir puan 100 olasılıkla eşleşen hiç yanıt bulunamadı 0 anlamına gelir, bir puan sırasında tam bir eşleşme var. Yüksek puan - yanıtında kendilerinden daha emin. Belirli bir sorgu için birden çok yanıt döndürdü olabilir. Bu durumda, yanıtları güvenilirlik puanı azalan sırayla döndürülür.

Aşağıdaki örnekte, 2 sorularla bir soru-cevap varlık görebilirsiniz.


![Soru-cevap çifti örneği](../media/qnamaker-concepts-confidencescore/ranker-example-qna.png)

Yukarıdaki örnek - için puanları örnek puanı aralık aşağıdaki-kullanıcı sorgularına farklı türleri için gibi bekleyebilirsiniz:


![Derecelendiricisini uygulama puanı aralığı](../media/qnamaker-concepts-confidencescore/ranker-score-range.png)


Aşağıdaki tabloda tipik güvenilirlik için belirli bir puan ilişkili gösterir.

|Puanı değeri|Puan anlama|Örnek sorgu|
|--|--|--|
|90 - 100|Bir kullanıcı sorgu KB soru ve tam eşleşme yakın|"Değişiklikler yayımladıktan sonra KB olarak güncelleştirilmiyor"|
|70 >|Yüksek güvenle - tamamen kullanıcının sorgu yanıt veren genellikle iyi bir cevap|"BB'mi yayımladım ancak değil güncelleştirildi"|
|50 - 70|Orta güven - genellikle bir kullanıcı sorgusu ana amacı yanıt oldukça iyi yanıt|"I BB'mi yayımlamadan önce güncelleştirmelerim kaydetmeliyim?"|
|30 - 50|Düşük güven - kullanıcının amacı kısmen yanıtladığı genellikle bir ilgili yanıt|"Kaydet ve eğitme ne yapar?"|
|< 30|Çok düşük güven - genellikle kullanıcının sorgu yanıt vermezse, ancak bazı eşleşen sözcük ve tümcecikleri sahiptir |"Nerede eş anlamlılar için BB'mi ekleyebilirim"|
|0|Yanıt alınmadı için hiç eşleşme.|"Hizmet maliyeti"|

## <a name="choose-a-score-threshold"></a>Bir puan eşiği seçin
Yukarıdaki tabloda, çoğu KB'leri üzerinde beklenen puanları gösterilmektedir. Ancak, her KB farklı olduğundan ve farklı türlerde sözcüklere, amaçlara ve hedeflere sahip olduğundan, test etmenizi ve sizin için en iyi işe yarar olan eşiği seçmenizi öneririz. Varsayılan olarak, eşik 0 olarak ayarlanır, böylece tüm olası yanıtlar döndürülür. En fazla KBs için çalışması gereken önerilen eşik **50**' dir.

Eşiğine seçerken, doğruluk ve kapsamı arasındaki dengeyi göz önünde bulundurun ve gereksinimlerinize göre eşiğine ince ayar.

- Varsa **doğruluğu** (veya duyarlık) senaryonuz için daha önemlidir ve ardından, eşiğini yükseltin. Bu şekilde bir yanıt döndürür her zaman büyük/küçük harf ve yanıt kullanıcılar aradığınız olması olası çok daha fazlasını bir kişiye olacaktır. Bu durumda, daha fazla yanıtlanmamış soruları bırakarak yukarı bitiş. *Örneğin:* eşiği yaparsanız **70**, "nedir kaydedin ve eğitme?" bazı belirsiz örnekler beğenilerin kaçırabilirsiniz.

- Varsa **kapsamı** (veya geri çağırma) daha önemli olduğu ve yalnızca kısmi bir ilişkisi için kullanıcının soru - olsa bile kadar fazla soruyu mümkün olduğunca ardından alt olarak eşiği yanıt istiyorsanız. Bu gösterir, burada yanıt kullanıcının gerçek sorgu yanıt vermezse, ancak bazı diğer biraz ilgili yanıt verir daha fazla durumda olabilir. *Örneğin:* **30**EŞIĞINI yaparsanız, "KB 'umu düzenleyebilirim?" gibi sorgular için yanıt verebilirsiniz.

> [!NOTE]
> Soru-cevap Oluşturucu daha yeni sürümlerini Puanlama mantığı için geliştirmeler içerir ve eşiğine etkileyebilir. İstediğiniz zaman hizmet güncelleştirmesi, test edin ve gerekiyorsa eşik ince emin olun. Soru-cevap hizmet sürümü denetleyebilirsiniz [burada](https://www.qnamaker.ai/UserSettings)ve son gelişmeleri öğrenin [burada](../How-To/set-up-qnamaker-service-azure.md#get-the-latest-runtime-updates).

## <a name="set-threshold"></a>Eşiği ayarla

Eşik Puanını [Generateanswer API JSON gövdesinin](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration)bir özelliği olarak ayarlayın. Bu, her GenerateAnswer çağrısı için ayarladığınız anlamına gelir.

Bot çerçevesinden, veya [C#](../how-to/metadata-generateanswer-usage.md?#use-qna-maker-with-a-bot-in-c) [Node. js](../how-to/metadata-generateanswer-usage.md?#use-qna-maker-with-a-bot-in-nodejs)ile Options nesnesinin bir parçası olarak puanı ayarlayın.

## <a name="improve-confidence-scores"></a>Güven puanlarını geliştirmeleri
Belirli bir kullanıcı sorgu yanıt güvenilirlik puanı geliştirmek için alternatif bir soru, yanıt olarak Bilgi Bankası'na kullanıcı sorgusu ekleyebilirsiniz. Büyük/küçük harf duyarsız [sözcük değişikliklerini](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/alterations/replace) , KB 'inizdeki anahtar sözcüklere eşanlamlı eklemek için de kullanabilirsiniz.


## <a name="similar-confidence-scores"></a>Benzer güven puanları
Birden çok yanıtı benzer bir güvenilirlik puanı varsa, sorgu çok geneldir ve bu nedenle, birden çok yanıtlarla eşit olasılığını ile eşleşen olasıdır. Soru-cevap her varlık, farklı bir hedefi sahip olacak şekilde, Bankalarıyla daha iyi yapısı deneyin.


## <a name="confidence-score-differences-between-test-and-production"></a>Test ve üretim arasındaki güven puanı farkları
İçeriği aynı olsa bile bir yanıt güvenilirlik puanı negligibly test ve Bilgi Bankası yayımlanmış sürümü arasında değişebilir. Bunun nedeni, test ve yayımlanan bilgi tabanı içeriğinin farklı Azure Bilişsel Arama dizinlerinde konumlandırıldı.

Test dizini, bilgi tabanlarınızın tüm QnA çiftlerini barındırır. Test dizini sorgulanırken, sorgu tüm dizine uygulanır, bundan sonra sonuçlar ilgili Bilgi Bankası için bölüm ile kısıtlanır. Test sorgu sonuçları, bilgi bankasını doğrulama yeteneğinizi olumsuz şekilde etkiledikleri takdirde şunları yapabilirsiniz:
* bilgi Bankalarınızı aşağıdakilerden birini kullanarak düzenleyin:
    * 1 KB ile kısıtlanan kaynak: tek bir QnA kaynağını (ve elde edilen Azure Bilişsel Arama test dizinini) tek bir Bilgi Bankası ile sınırlandırın.
    * 2 kaynak-test için 1, üretim için 1: bir test için (kendi test ve üretim dizinleriyle) bir tane olmak üzere bir diğeri de bir ürüne (kendi test ve üretim dizinlerine sahip olması) yönelik iki Soru-Cevap Oluşturma kaynağı vardır
* hem test hem de üretim bilgi tabanınızı sorgularken **[üst](../how-to/improve-knowledge-base.md#use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers)** gibi her zaman aynı parametreleri kullanın

Bir Bilgi Bankası yayımladığınızda, bilgi Bankalarınızın sorusu ve yanıt içerikleri, test dizininden Azure Search 'teki bir üretim dizinine gider. [Yayımla](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base) işleminin nasıl çalıştığını görün.

Farklı bölgelerde bilgi tabanınız varsa, her bölge kendi Azure Bilişsel Arama dizinini kullanır. Farklı dizinler kullanıldığından, puanlar tam olarak aynı olmayacaktır.


## <a name="no-match-found"></a>Eşleşme bulunamadı
Derecelendiricisini tarafından iyi bir eşleşme bulunduğunda 0.0 ya da "None" güven puanı döndürülür ve "iyi eşleşme KB bulunamadı" varsayılan yanıttır. Uç noktayı çağıran bot veya Application kodundaki bu [varsayılan yanıtı](../How-To/metadata-generateanswer-usage.md) geçersiz kılabilirsiniz. Alternatif olarak, geçersiz kılma yanıt Azure'da ayarlayabilirsiniz ve bu belirli bir soru-cevap Oluşturucu hizmeti dağıtılan tüm bilgi bankaları için varsayılan değiştirir.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [En iyi uygulamalar](./best-practices.md)

