---
title: Güven puanı - QnA Maker
titleSuffix: Azure Cognitive Services
description: Bir bilgi tabanı yayınlanmalıdır. Yayımlandıktan sonra, bilgi tabanı, generateAnswer API kullanılarak çalışma zamanı tahmin bitiş noktasında sorgulanır.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76843461"
---
# <a name="the-confidence-score-of-an-answer"></a>Bir yanıtın güven puanı
Bir kullanıcı sorgusu bir bilgi tabanıyla eşleştiğinde, QnA Maker güven puanıyla birlikte ilgili yanıtları döndürür. Bu puan, yanıtın verilen kullanıcı sorgusu için doğru eşleşme olduğuna olan güveni gösterir.

Güven puanı 0 ile 100 arasında bir sayıdır. 100'lük bir skor büyük olasılıkla tam bir eşleşmedir, 0'lık bir skor ise eşleşen bir yanıt bulunmadığı anlamına gelir. Skor ne kadar yüksekse, cevaba olan güven de o kadar büyük tür. Belirli bir sorgu için birden çok yanıt döndürülebilir. Bu durumda, yanıtlar güven puanı azalan sırayla döndürülür.

Aşağıdaki örnekte, 2 soruile bir QnA varlığı görebilirsiniz.


![Örnek QnA çifti](../media/qnamaker-concepts-confidencescore/ranker-example-qna.png)

Yukarıdaki örnek için, farklı kullanıcı sorguları için aşağıdaki örnek puan aralığı gibi puanlar bekleyebilirsiniz:


![Ranker puan aralığı](../media/qnamaker-concepts-confidencescore/ranker-score-range.png)


Aşağıdaki tablo, belirli bir puan için ilişkili tipik güveni gösterir.

|Puan Değeri|Puan Anlamı|Örnek Sorgu|
|--|--|--|
|90 - 100|Kullanıcı sorgusunun ve KB sorusunun neredeyse tam eşleşmesi|"Değişikliklerim yayımlandıktan sonra KB'de güncelleştirilemiyor"|
|> 70|Yüksek güven - genellikle tamamen kullanıcının sorgusu yanıtlar iyi bir cevap|"KB'mi yayınladım ama güncellenmedi"|
|50 - 70|Orta güven - genellikle kullanıcı sorgusunun ana amacı cevap vermelidir oldukça iyi bir cevap|"KB'mi yayımlamadan önce güncellemelerimi kaydetmeli miyim?"|
|30 - 50|Düşük güven - genellikle kullanıcının niyetini kısmen yanıtlayan ilgili bir yanıt|" Kurtarma ve tren ne işe yarar?"|
|< 30|Çok düşük güven - genellikle kullanıcının sorgusuna cevap vermez, ancak bazı eşleşen sözcükler veya ifadeler vardır |" KB'me nerede eş anlamlı lar ekleyebilirim"|
|0|Eşleşme yok, bu yüzden cevap döndürülmez.|"Hizmet in maliyeti nedir"|

## <a name="choose-a-score-threshold"></a>Puan eşiği seçin
Yukarıdaki tablo, çoğu KB'de beklenen puanları gösterir. Ancak, her KB farklı olduğundan ve farklı kelime, niyet ve hedef türleri olduğundan, sizin için en uygun eşiği test etmenizi ve seçmenizi öneririz. Varsayılan olarak eşik 0 olarak ayarlanır, böylece tüm olası yanıtlar döndürülür. Çoğu KBs için çalışması gereken önerilen **eşik, 50'dir.**

Eşiğinizi seçerken, Doğruluk ve Kapsam arasındaki dengeyi aklınızda bulundurun ve gereksinimlerinize göre eşiğinizi değiştirin.

- **Doğruluk** (veya kesinlik) senaryonuz için daha önemliyse, eşiğinizi artırın. Bu şekilde, bir cevap dönmek her zaman, çok daha CONFIDENT durumda olacak ve çok daha fazla cevap kullanıcıların arıyoruz olması muhtemeldir. Bu durumda, daha fazla soruyu yanıtsız bırakabilirsiniz. *Örneğin:* eşik **70**yaparsanız, "kaydet ve eğitme nedir?" gibi bazı belirsiz örnekleri kaçırabilirsiniz.

- **Kapsam** (veya geri çağırma) daha önemliyse ve kullanıcının sorusuyla sadece kısmi bir ilişki olsa bile mümkün olduğunca çok soruyu yanıtlamak istiyorsanız, eşiği düşürün. Bu, yanıtın kullanıcının gerçek sorgusunu yanıtlamadığı, ancak başka bir şekilde ilgili başka bir yanıt verdiği daha fazla servis durumu olabileceği anlamına gelir. *Örneğin:* **30**barajını ederseniz, "KB'mi nerede edinebilirim?" gibi soruların yanıtlarını verebilirsiniz.

> [!NOTE]
> QnA Maker'ın yeni sürümleri puanlama mantığında iyileştirmeler içerir ve eşiğinizi etkileyebilir. Hizmeti her güncellediğinizde, gerekirse eşiği test ettiğinizden ve değiştirip ayarladığınızdan emin olun. QnA Service sürümünüzü [buradan](https://www.qnamaker.ai/UserSettings)kontrol edebilir ve en son güncellemeleri buradan nasıl [alacağınızı](../How-To/set-up-qnamaker-service-azure.md#get-the-latest-runtime-updates)görebilirsiniz.

## <a name="set-threshold"></a>Eşik ayarlama

Eşik puanını [GenerateAnswer API JSON gövdesinin](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration)bir özelliği olarak ayarlayın. Bu, GenerateAnswer için her arama için ayarladığınız anlamına gelir.

Bot çerçevesinden, [C#](../how-to/metadata-generateanswer-usage.md?#use-qna-maker-with-a-bot-in-c) veya [Node.js](../how-to/metadata-generateanswer-usage.md?#use-qna-maker-with-a-bot-in-nodejs)ile seçenekler nesnesinin bir parçası olarak skoru ayarlayın.

## <a name="improve-confidence-scores"></a>Güven puanlarını artırın
Bir kullanıcı sorgusuna verilen belirli bir yanıtın güven puanını artırmak için, kullanıcı sorgusunu bilgi tabanına bu yanıtta alternatif bir soru olarak ekleyebilirsiniz. KB'nizdeki anahtar kelimelere eş anlamlı lar eklemek için büyük/küçük harf duyarlı [sözcük değişikliklerini](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/alterations/replace) de kullanabilirsiniz.


## <a name="similar-confidence-scores"></a>Benzer güven puanları
Birden çok yanıt benzer bir güven puanına sahipse, sorgunun çok genel olması ve bu nedenle birden çok yanıtla eşit olasılıkla eşleştirilmiş olması olasıdır. Her QnA varlığının farklı bir amacı olacak şekilde QnA'larınızı daha iyi yapılandırmaya çalışın.


## <a name="confidence-score-differences-between-test-and-production"></a>Test ve üretim arasındaki güven puanı farkları
Bir yanıtın güven puanı, içerik aynı olsa bile, test ve bilgi tabanının yayımlanmış sürümü arasında önemsiz bir şekilde değişebilir. Bunun nedeni, testin içeriğinin ve yayınlanan bilgi tabanının farklı Azure Bilişsel Arama dizinlerinde bulunmasıdır.

Test dizini, bilgi üslerinizin tüm QnA çiftlerini tutar. Test dizini sorgulanırken, sorgu tüm dizin için geçerlidir, sonra sonuçlar belirli bir bilgi tabanı için bölüm ile sınırlıdır. Test sorgusu sonuçları bilgi tabanını doğrulama yeteneğinizi olumsuz etkiliyorsa, şunları yapabilirsiniz:
* aşağıdakilerden birini kullanarak bilgi tabanınızı düzenleyin:
    * 1 KB ile sınırlandırılmış 1 kaynak: Tek QnA kaynağınızı (ve ortaya çıkan Azure Bilişsel Arama test dizinini) tek bir bilgi tabanıyla sınırlandırın.
    * 2 kaynak - test için 1, üretim için 1: iki QnA Maker kaynakları var, test için bir (kendi test ve üretim dizinleri ile) ve ürün için bir (ayrıca kendi test ve üretim dizinleri olan) kullanarak
* ve hem test hem de üretim bilgi tabanınızı sorgularken **[üst](../how-to/improve-knowledge-base.md#use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers)** teki gibi her zaman aynı parametreleri kullanın

Bir bilgi tabanı yayımladığınızda, bilgi tabanınızın soru ve yanıt içeriği test dizininden Azure aramasında üretim dizinine taşınır. [Yayımlama](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base) işleminin nasıl çalıştığını görün.

Farklı bölgelerde bir bilgi tabanınız varsa, her bölge kendi Azure Bilişsel Arama dizinini kullanır. Farklı dizinler kullanıldığından, puanlar tam olarak aynı olmayacaktır.


## <a name="no-match-found"></a>Eşleşme bulunamadı
Ranker tarafından iyi bir eşleşme bulunmadığında, 0,0 veya "Yok" güven puanı döndürülür ve varsayılan yanıt "KB'de iyi eşleşme bulunmaz" olur. Bu [varsayılan yanıtı](../How-To/metadata-generateanswer-usage.md) bitiş noktasını çağıran bot veya uygulama kodunda geçersiz kılabilirsiniz. Alternatif olarak, Azure'da geçersiz kılma yanıtını da ayarlayabilirsiniz ve bu, belirli bir QnA Maker hizmetinde dağıtılan tüm bilgi tabanları için varsayılan değişikliği sağlar.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [En iyi uygulamalar](./best-practices.md)

