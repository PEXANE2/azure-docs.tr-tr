---
title: İyi örnek söz - LUIS
description: İfadeler kullanıcının yaptığı ve uygulamanızın yorumlaması gereken girişlerdir. Kullanıcıların gireceğini düşündüğünüz ifadeleri toplayın. Aynı anlama gelen ancak sözcük uzunluğu ve sözcük yerleştirmede farklı olarak oluşturulmuş sözcük leri ekleyin.
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: d851082a4ec4a003619826eeffd4f4b856a67824
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81382283"
---
# <a name="understand-what-good-utterances-are-for-your-luis-app"></a>LUIS uygulamanız için iyi sözlerle ne olduğunu anlayın

**Söyleyişler,** uygulamanızın yorumlanması gereken kullanıcıdan gelen girişlerdir. LUIS'i, niyet ve varlıkları onlardan ayıklamak için eğitmek için, her amaç için çeşitli örnek söylentiler yakalamak önemlidir. Aktif öğrenme, ya da yeni söyleyişler üzerinde eğitmeye devam süreci, LUIS'in sağladığı makinede öğrenilen zeka için gereklidir.

Kullanıcıların gireceğini düşündüğünüz söyleyiyi toplayın. Aynı anlama gelen ancak çeşitli şekillerde inşa edilen söyleyişleri ekleyin:

* Söyleyiş uzunluğu - kısa, orta ve uzun istemci-uygulama için
* Sözcük ve tümcecik uzunluğu
* Sözcük yerleştirme - sözcük başında, orta ve sözcük sonunda varlık
* Dilbilgisi
* Çoğullaştırma
* Dallanma
* Isim ve fiil seçimi
* [Noktalama](luis-reference-application-settings.md#punctuation-normalization) - doğru, yanlış ve dilbilgisi kullanarak iyi bir çeşitlilik

## <a name="how-to-choose-varied-utterances"></a>Nasıl çeşitli söyleyinmeler seçmek için

Luis modelinize [örnek sözlerle](luis-how-to-add-example-utterances.md) ilk başladığınızda, aklınızda bulundurmanız gereken bazı ilkeler şunlardır.

### <a name="utterances-arent-always-well-formed"></a>Söylenmeler her zaman iyi şekilli değildir.

"Benim için Paris'e bilet ayırt" gibi bir cümle ya da "Rezervasyon" ya da "Paris uçuşu" gibi bir cümlenin bir parçası olabilir.  Kullanıcılar genellikle yazım hataları yapar. Uygulamanızı planlarken, kullanıcı girişini LUIS'e geçirmeden önce düzeltmek için [Bing Yazım Denetimi'ni](luis-tutorial-bing-spellcheck.md) kullanıp kullanmadığınızı göz önünde bulundurun.

Kullanıcı sözcülerini hecelemiyorsanız, LUIS'i yazım hataları ve yazım hataları içeren söyleyişler konusunda eğitmelisiniz.

### <a name="use-the-representative-language-of-the-user"></a>Kullanıcının temsil dilini kullanma

Söyleyiş leri seçerken, ortak bir terim veya tümcecik olduğunu düşündüğünüz şeyin istemci uygulamanızın tipik kullanıcısı için doğru olmayabileceğini unutmayın. Etki alanı deneyimi olmayabilir. Bir kullanıcının yalnızca bir uzman olsaydı söyleyeceği terimleri veya tümcecikleri kullanırken dikkatli olun.

### <a name="choose-varied-terminology-as-well-as-phrasing"></a>Çeşitli terminolojinin yanı sıra tümceleri seçin

Çeşitli cümle kalıpları oluşturmak için çaba gösterseniz bile, yine de bazı kelime tekraredecektir bulacaksınız.

Şu örnek sözlerle ele alalım:

|Örnek konuşmalar|
|--|
|Nasıl bir bilgisayar alabilirim?|
|Bilgisayarı nereden bulabilirim?|
|Bilgisayar almak istiyorum, nasıl yapabilirim?|
|Ne zaman bir bilgisayarım olabilir?|

Buradaki temel terim, "bilgisayar", çeşitli değildir. Masaüstü bilgisayar, dizüstü bilgisayar, iş istasyonu ve hatta sadece makine gibi alternatifleri kullanın. LUIS, eş anlamlıları bağlamından akıllıca çıkartabilir, ancak eğitim için söyleşiler oluşturduğunuzda, bunları değiştirmek her zaman daha iyidir.

## <a name="example-utterances-in-each-intent"></a>Her niyette örnek söz

Her niyetin en az 15 örnek söz emesi gerekir. Herhangi bir örnek söyleyiş olmayan bir niyet varsa, LUIS eğitmek mümkün olmayacaktır. Eğer bir veya çok az örnek söyleyiş ile bir niyet varsa, LUIS doğru niyet tahmin olmayabilir.

## <a name="add-small-groups-of-15-utterances-for-each-authoring-iteration"></a>Her yazma yinelemesi için 15 kelimeden oluşan küçük gruplar ekleyin

Modelin her yinelemesinde, büyük miktarda söyleyiş eklemeyin. 15 miktarda söyleyinmeekleyin. [Tren](luis-how-to-train.md), [yayınlamak](luis-how-to-publish-app.md), ve tekrar [test](luis-interactive-test.md) edin.

LUIS, LUIS modeli yazarı tarafından özenle seçilen söyleyerek etkili modeller oluşturur. Çok fazla söyleyiş eklemek değerli değildir çünkü kafa karışıklığına yol açar.

Birkaç söyleyiş ile başlamak için daha iyidir, sonra doğru niyet tahmini ve varlık çıkarma için [uç nokta söyleyiş gözden geçirin.](luis-how-to-review-endpoint-utterances.md)

## <a name="utterance-normalization"></a>Söyleyiş normalleştirme

Söyleyerek normalleştirme, eğitim ve tahmin sırasında noktalama işaretlerinin ve aksamaların etkilerini göz ardı etme işlemidir. Söyleyiş normalleştirmenin söyleyiş tahminlerini nasıl etkilediğini denetlemek için [uygulama ayarlarını](luis-reference-application-settings.md) kullanın.

## <a name="utterance-normalization-for-diacritics-and-punctuation"></a>Aksatçlar ve noktalama işaretleri için söyleyiş normalleştirmesi

Uygulama JSON dosyasında bir ayar olduğu için uygulamayı oluşturduğunuzda veya içe aktardığınızda söyleyiş normalleştirmesi tanımlanır. Söyleyiş normalleştirme ayarları varsayılan olarak kapatılır.

Aksenler, metin deki işaretler veya işaretlerdir:

```
İ ı Ş Ğ ş ğ ö ü
```

Uygulamanız normale dönerse, **Test** bölmesindeki puanlar, toplu testler ve uç nokta sorguları aksan veya noktalama işareti kullanarak tüm tüm tüm sözcükler için değişir.

Parametredeki LUIS JSON uygulama dosyanıza aksama veya noktalama `settings` işaretleri için söyleyiş normalleştirmesini açın.

```JSON
"settings": [
    {"name": "NormalizePunctuation", "value": "true"},
    {"name": "NormalizeDiacritics", "value": "true"}
]
```

**Noktalama işaretlerini** normalleştirmek, modelleriniz eğitilmeden ve bitiş noktası sorgularınız tahmin edilmeden önce noktalama işaretlerinin dile gelenlerden kaldırılacağı anlamına gelir.

**Aksiyi** normalleştirmek, karakterlerin yerine, düzenli karakterlerle birlikte, söyleyişdeki aksitlerle değiştirilir. Örneğin: `Je parle français` olur `Je parle francais`.

Normalleştirme, örnek söyleyişlerinizde veya tahmin yanıtlarınızda noktalama işaretleri ve aksanlar görmeyeceğiniz anlamına gelmez, yalnızca eğitim ve tahmin sırasında göz ardı edilecekleri anlamına gelir.

### <a name="punctuation-marks"></a>Noktalama işaretleri

Noktalama, LUIS'te ayrı bir belirteçtir. Sonunda bir dönem içeren bir söyleyiş sonunda bir dönem içermeyen bir söyleyiş iki ayrı sözcük ve iki farklı tahminler alabilirsiniz.

Noktalama işaretleri normalleştirilemezse, bazı istemci uygulamaları bu işaretlerüzerinde önem ifade edebileceğinden, LUIS varsayılan olarak noktalama işaretlerini göz ardı etmez. Her iki stilin de aynı göreli puanları döndürebilmesi için örnek söyleyişlerinizin hem noktalama işaretlerini hem de noktalama işaretlerini kullanmadığından emin olun.

Modelin noktalama işaretlerini örnek sözdiziminde (noktalama işaretlerine sahip olma ve sahip olmamak) veya özel sözdizimi yle noktalama işaretlerini yok saymanın daha kolay olduğu [desenlerde](luis-concept-patterns.md) işlediğinden emin olun:`I am applying for the {Job} position[.]`

Noktalama işaretleri istemci uygulamanızda belirli bir anlam ifade etmemişse, noktalama işaretlerini normalleştirerek [noktalama işaretlerini yok saymayı](#utterance-normalization) düşünün.

### <a name="ignoring-words-and-punctuation"></a>Sözcükleri ve noktalama işaretlerini yok sayma

Desenlerde belirli sözcükleri veya noktalama işaretlerini yoksaymak istiyorsanız, kare ayraçların `[]`sözdizimini yok _sayan_ bir [desen](luis-concept-patterns.md#pattern-syntax) kullanın.

## <a name="training-utterances"></a>Eğitim söyleyişleri

Eğitim genellikle belirleyici değildir: söyleyiş tahmini sürümler veya uygulamalar arasında biraz farklılık gösterebilir.
Tüm eğitim verilerini kullanmak için [sürüm ayarları](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) API'sini `UseAllTrainingData` ad/değer çiftiyle güncelleyerek deterministik olmayan eğitimi kaldırabilirsiniz.

## <a name="testing-utterances"></a>Testleri söyleyünmeler

Geliştiriciler, [öngörü bitiş noktası](luis-how-to-azure-subscription.md) URL'sine söz göndererek LUIS uygulamalarını gerçek trafikle test etmeye başlamalıdır. Bu söyleyişler, [İnceleme ile](luis-how-to-review-endpoint-utterances.md)niyet ve varlıkların performansını artırmak için kullanılır. LUIS web sitesi test bölmesi ile gönderilen testler bitiş noktası üzerinden gönderilmez ve bu nedenle aktif öğrenme için katkıda bulunmaz.

## <a name="review-utterances"></a>Sözlerle ilgili incelemeleri gözden geçirin

Modeliniz eğitildikten, yayımlandıktan ve [uç nokta](luis-glossary.md#endpoint) sorguları aldıktan sonra LUIS tarafından önerilen [söyleyileri gözden geçirin.](luis-how-to-review-endpoint-utterances.md) LUIS, amaç veya varlık için düşük puanları olan uç nokta lı söyleyişleri seçer.

## <a name="best-practices"></a>En iyi uygulamalar

En iyi uygulamaları gözden [geçirin](luis-concept-best-practices.md) ve bunları normal yazma döngünüzün bir parçası olarak uygulayın.

## <a name="label-for-word-meaning"></a>Sözcük anlamı için etiket

Sözcük seçimi veya sözcük düzenlemesi aynıysa, ancak aynı anlama gelmiyorsa, varlıkla etiketlemeyin.

Aşağıdaki ifadeler, kelime `fair` bir homograf olduğunu. Aynı yazıldığından ancak farklı bir anlamı vardır:

|İfade|
|--|
|Bu yaz Seattle bölgesinde ne tür ilçe fuarları yapılıyor?|
|Seattle inceleme için geçerli derecelendirme adil mi?|

Bir olay varlığının tüm olay verilerini bulmasını `fair` istiyorsanız, sözcüğü ilk sözcükte etiketle, ancak ikincisinde etiketlemeyin.


## <a name="next-steps"></a>Sonraki adımlar
Bkz. Kullanıcı sözcüklerini anlamak için bir LUIS uygulamasının eğitimi hakkında bilgi almak için [örnek sözcük ekle.](luis-how-to-add-example-utterances.md)

