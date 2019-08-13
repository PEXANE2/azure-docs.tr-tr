---
title: Güvenirlik puanı-Soru-Cevap Oluşturma
titleSuffix: Azure Cognitive Services
description: Güvenilirlik puanı güvenle yanıt verilen kullanıcı sorgusu için doğru eşleşme olduğunu gösterir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 06/17/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: afc50a5adb591550f6e988a572d1ac9a8c4439cb
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68955195"
---
# <a name="confidence-score-of-a-qna-maker-knowledge-base"></a>Soru-cevap Oluşturucu Bilgi Bankası güvenilirlik puanı
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
> Soru-cevap Oluşturucu daha yeni sürümlerini Puanlama mantığı için geliştirmeler içerir ve eşiğine etkileyebilir. İstediğiniz zaman hizmet güncelleştirmesi, test edin ve gerekiyorsa eşik ince emin olun. Soru-cevap hizmet sürümü denetleyebilirsiniz [burada](https://www.qnamaker.ai/UserSettings)ve son gelişmeleri öğrenin [burada](../How-To/troubleshooting-runtime.md).

## <a name="set-threshold"></a>Eşiği ayarla 

Eşik Puanını [Generateanswer API JSON gövdesinin](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration)bir özelliği olarak ayarlayın. Bu, her GenerateAnswer çağrısı için ayarladığınız anlamına gelir. 

Bot çerçevesinden, veya [C#](../how-to/metadata-generateanswer-usage.md?#use-qna-maker-with-a-bot-in-c) [Node. js](../how-to/metadata-generateanswer-usage.md?#use-qna-maker-with-a-bot-in-nodejs)ile Options nesnesinin bir parçası olarak puanı ayarlayın.

## <a name="improve-confidence-scores"></a>Güven puanlarını geliştirmeleri
Belirli bir kullanıcı sorgu yanıt güvenilirlik puanı geliştirmek için alternatif bir soru, yanıt olarak Bilgi Bankası'na kullanıcı sorgusu ekleyebilirsiniz. Büyük/küçük harf duyarsız [sözcük değişikliklerini](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/alterations/replace) , KB 'inizdeki anahtar sözcüklere eşanlamlı eklemek için de kullanabilirsiniz.


## <a name="similar-confidence-scores"></a>Benzer güven puanları
Birden çok yanıtı benzer bir güvenilirlik puanı varsa, sorgu çok geneldir ve bu nedenle, birden çok yanıtlarla eşit olasılığını ile eşleşen olasıdır. Soru-cevap her varlık, farklı bir hedefi sahip olacak şekilde, Bankalarıyla daha iyi yapısı deneyin.


## <a name="confidence-score-differences"></a>Güvenilirlik puanı farkları
İçeriği aynı olsa bile bir yanıt güvenilirlik puanı negligibly test ve Bilgi Bankası yayımlanmış sürümü arasında değişebilir. Test ve yayımlanan Bilgi Bankası içeriğini bulunur farklı Azure Search dizinlerini olmasıdır. Bir Bilgi Bankası yayımladığınızda, bilgi Bankalarınızın sorusu ve yanıt içerikleri, test dizininden Azure Search 'teki bir üretim dizinine gider. [Yayımla](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base) işleminin nasıl çalıştığını görün.

Farklı bölgelerde bilgi tabanınız varsa, her bölge kendi Azure Search dizinini kullanır. Farklı dizinler kullanıldığından, puanlar tam olarak aynı olmayacaktır. 


## <a name="no-match-found"></a>Eşleşme bulunamadı
Derecelendiricisini tarafından iyi bir eşleşme bulunduğunda 0.0 ya da "None" güven puanı döndürülür ve "iyi eşleşme KB bulunamadı" varsayılan yanıttır. Uç noktayı çağıran bot veya Application kodundaki bu [varsayılan yanıtı](#change-default-answer) geçersiz kılabilirsiniz. Alternatif olarak, geçersiz kılma yanıt Azure'da ayarlayabilirsiniz ve bu belirli bir soru-cevap Oluşturucu hizmeti dağıtılan tüm bilgi bankaları için varsayılan değiştirir.

## <a name="change-default-answer"></a>Varsayılan yanıt değiştirme

1. Git [Azure portalında](https://portal.azure.com) ve oluşturduğunuz soru-cevap Oluşturucu hizmetini temsil eder kaynak grubuna gidin.

2. Açmak için tıklayın **App Service**.

    ![Azure portalında App service için soru-cevap Oluşturucu erişim](../media/qnamaker-concepts-confidencescore/set-default-response.png)

3. Tıklayarak **uygulama ayarları** ve düzenleme **DefaultAnswer** istenen varsayılan yanıt alanı. **Kaydet**’e tıklayın.

    ![Uygulama ayarlarını seçin ve ardından DefaultAnswer için soru-cevap Oluşturucu düzenleyin](../media/qnamaker-concepts-confidencescore/change-response.png)

4. App service'ı yeniden başlatın

    ![Soru-cevap Oluşturucu appservice DefaultAnswer değiştirdikten sonra yeniden başlatın](../media/qnamaker-faq/qnamaker-appservice-restart.png)


## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Desteklenen veri kaynakları](./data-sources-supported.md)

