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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "76843461"
---
# <a name="the-confidence-score-of-an-answer"></a>Bir yanıtın Güvenirlik puanı
Bir Kullanıcı sorgusu bir Bilgi Bankası ile eşleştiğinde, Soru-Cevap Oluşturma ilgili yanıtları, Güvenirlik puanı ile birlikte döndürür. Bu puan, yanıtın verilen kullanıcı sorgusuyla doğru eşleşme olduğunu belirtir.

Güvenirlik puanı 0 ile 100 arasında bir sayıdır. 100 puanı büyük olasılıkla tam bir eşleşmedir, 0 puanı, hiçbir eşleşen yanıt bulunamamıştır. Puan arttıkça, yanıtın güveni daha yüksektir. Belirli bir sorgu için birden fazla yanıt döndürüldü. Bu durumda, yanıtlar daha fazla güvenilirlik puanı sırasına göre döndürülür.

Aşağıdaki örnekte, 2 sorudan bir QnA varlığı görebilirsiniz.


![Örnek QnA çifti](../media/qnamaker-concepts-confidencescore/ranker-example-qna.png)

Yukarıdaki örnek için-aşağıdaki örnek puan aralığı gibi puanları, farklı Kullanıcı sorgu türleri için de bekleyebilir:


![Ranker puan aralığı](../media/qnamaker-concepts-confidencescore/ranker-score-range.png)


Aşağıdaki tablo, belirli bir puan için ilişkili olan tipik güvenilirliği gösterir.

|Puan değeri|Puan anlamı|Örnek sorgu|
|--|--|--|
|90-100|Kullanıcı sorgusuyla tam olarak eşleşme ve bir KB sorusu|"Değişiklikler yayımladıktan sonra KB olarak güncelleştirilmiyor"|
|> 70|Yüksek güvenilirlik-genellikle kullanıcının sorgusuna tamamen cevap veren iyi bir yanıt|"KB 'mi yayımladım ancak güncelleştirilmedi"|
|50-70|Orta güvenilirlik-genellikle Kullanıcı sorgusunun ana hedefini yanıtlaması gereken oldukça iyi bir yanıt|"KB 'umu yayımlamadan önce güncelleştirmelerimi kaydetmem gerekir mi?"|
|30 - 50|Düşük güvenilirlik-genellikle kullanıcının hedefini kısmen cevapladığı ilgili bir yanıt|"Kaydet ve eğitme ne yapar?"|
|< 30|Çok düşük güvenilirlik-genellikle kullanıcının sorgusuna yanıt vermez, ancak bazı eşleşen sözcüklere veya tümceciklere sahiptir |"KB 'ime eş anlamlılar ekleyebilirim?"|
|0|Eşleşme yok, bu nedenle yanıt döndürülmüyor.|"Hizmet maliyeti ne kadar sürer"|

## <a name="choose-a-score-threshold"></a>Bir puan eşiği seçin
Yukarıdaki tabloda, en fazla KBs üzerinde beklenen puanlar gösterilmektedir. Ancak, her KB farklı olduğundan ve farklı türlerde sözcüklere, amaçlara ve hedeflere sahip olduğundan, test etmenizi ve sizin için en iyi işe yarar olan eşiği seçmenizi öneririz. Varsayılan olarak, eşik 0 olarak ayarlanır, böylece tüm olası yanıtlar döndürülür. En fazla KBs için çalışması gereken önerilen eşik **50**' dir.

Eşikinizi seçerken doğruluk ve kapsam arasındaki dengeyi göz önünde bulundurun ve gereksinimlerinize göre eşikinizi ince ayar edin.

- Eğer senaryonuz için **doğruluk** (veya duyarlık) daha önemliyse, daha sonra eşiği artırın. Bu şekilde, bir cevap her geri döndüğünüzde, çok daha duyarlı bir durumdur ve kullanıcıların aradıklarından çok daha büyük bir durum olacaktır. Bu durumda, yanıtlanmayan daha fazla soru bırakabilir. *Örneğin:* **70**eşiğini yaparsanız, "Kaydet ve eğitme nedir?" gibi bazı belirsiz örnekleri kaçırırdınız.

- **Kapsam** (veya geri çekme) daha önemliyse ve kullanıcının sorusuna yalnızca kısmi bir ilişki olsa bıle eşiği düşürmek için mümkün olduğunca fazla soru yanıtlamak istiyorsanız. Bu, yanıtın kullanıcının gerçek sorgusuna yanıt içermediği ancak biraz ilgili başka bir yanıt verdiği durumlarda daha fazla durum olabileceği anlamına gelir. *Örneğin:* **30**EŞIĞINI yaparsanız, "KB 'umu düzenleyebilirim?" gibi sorgular için yanıt verebilirsiniz.

> [!NOTE]
> Soru-Cevap Oluşturma yeni sürümleri, Puanlama mantığına yönelik iyileştirmeler içerir ve eşikinizi etkileyebilir. Hizmeti her güncelleştirdiğinizde, gerekirse eşiğin test ve ince ayar olduğundan emin olun. [Burada](https://www.qnamaker.ai/UserSettings)QNA hizmeti sürümünüzü denetleyebilir ve en son güncelleştirmeleri [buradan](../How-To/set-up-qnamaker-service-azure.md#get-the-latest-runtime-updates)nasıl alacağınız hakkında bilgi edinebilirsiniz.

## <a name="set-threshold"></a>Eşiği ayarla

Eşik Puanını [Generateanswer API JSON gövdesinin](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration)bir özelliği olarak ayarlayın. Bu, her GenerateAnswer çağrısı için ayarladığınız anlamına gelir.

Bot çerçevesinden, bu puanı [C#](../how-to/metadata-generateanswer-usage.md?#use-qna-maker-with-a-bot-in-c) veya [Node. js](../how-to/metadata-generateanswer-usage.md?#use-qna-maker-with-a-bot-in-nodejs)ile Options nesnesinin bir parçası olarak ayarlayın.

## <a name="improve-confidence-scores"></a>Güven puanlarını geliştirme
Bir Kullanıcı sorgusuna belirli bir yanıtın güvenilirlik Puanını artırmak için, bu yanıta alternatif bir soru olarak Kullanıcı sorgusunu Bilgi Bankası 'na ekleyebilirsiniz. Büyük/küçük harf duyarsız [sözcük değişikliklerini](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/alterations/replace) , KB 'inizdeki anahtar sözcüklere eşanlamlı eklemek için de kullanabilirsiniz.


## <a name="similar-confidence-scores"></a>Benzer güven puanları
Birden çok yanıtın benzer bir güven puanı olduğunda, bu, sorgunun çok genel olması ve bu nedenle birden fazla Yanıt ile eşit olasılıkla eşleştirildiği bir olasılıktır. Her QnA varlığının ayrı bir amacı olması için QnAs 'nizi daha iyi bir şekilde yapınızı deneyin.


## <a name="confidence-score-differences-between-test-and-production"></a>Test ve üretim arasındaki güven puanı farkları
Bir yanıtın Güvenirlik puanı, içerik aynı olsa bile, bilgi bankasındaki test ve yayımlanmış sürümü arasında ihmal edilebilir olarak değişebilir. Bunun nedeni, test ve yayımlanan bilgi tabanı içeriğinin farklı Azure Bilişsel Arama dizinlerinde konumlandırıldı.

Test dizini, bilgi tabanlarınızın tüm QnA çiftlerini barındırır. Test dizini sorgulanırken, sorgu tüm dizine uygulanır, bundan sonra sonuçlar ilgili Bilgi Bankası için bölüm ile kısıtlanır. Test sorgu sonuçları, bilgi bankasını doğrulama yeteneğinizi olumsuz şekilde etkiledikleri takdirde şunları yapabilirsiniz:
* bilgi Bankalarınızı aşağıdakilerden birini kullanarak düzenleyin:
    * 1 KB ile kısıtlanan kaynak: tek bir QnA kaynağını (ve elde edilen Azure Bilişsel Arama test dizinini) tek bir Bilgi Bankası ile sınırlandırın.
    * 2 kaynak-test için 1, üretim için 1: bir test için (kendi test ve üretim dizinleriyle) bir tane olmak üzere bir diğeri de bir ürüne (kendi test ve üretim dizinlerine sahip olması) yönelik iki Soru-Cevap Oluşturma kaynağı vardır
* hem test hem de üretim bilgi tabanınızı sorgularken **[üst](../how-to/improve-knowledge-base.md#use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers)** gibi her zaman aynı parametreleri kullanın

Bir Bilgi Bankası yayımladığınızda, bilgi Bankalarınızın sorusu ve yanıt içerikleri, test dizininden Azure Search 'teki bir üretim dizinine gider. [Yayımla](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base) işleminin nasıl çalıştığını görün.

Farklı bölgelerde bilgi tabanınız varsa, her bölge kendi Azure Bilişsel Arama dizinini kullanır. Farklı dizinler kullanıldığından, puanlar tam olarak aynı olmayacaktır.


## <a name="no-match-found"></a>Eşleşme bulunamadı
Ranker tarafından herhangi bir iyi eşleşme bulunmazsa, 0,0 veya "none" güvenilirlik puanı döndürülür ve varsayılan yanıt "KB 'de iyi eşleşme bulunamadı" olur. Uç noktayı çağıran bot veya Application kodundaki bu [varsayılan yanıtı](../How-To/metadata-generateanswer-usage.md) geçersiz kılabilirsiniz. Alternatif olarak, aynı zamanda Azure 'da geçersiz kılma yanıtı da ayarlayabilirsiniz ve bu, belirli bir Soru-Cevap Oluşturma hizmetinde dağıtılan tüm bilgi tabanları için varsayılan olarak değişiklik yapabilir.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [En iyi uygulamalar](./best-practices.md)

