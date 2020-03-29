---
title: Pano - Dil Anlama - LUIS
titleSuffix: Azure Cognitive Services
description: Eğitimli uygulamanızın panosuyla niyetleri ve varlıkları düzeltin. Pano, düzeltilmesi gereken niyet vurgularıyla birlikte genel uygulama bilgilerini görüntüler.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/08/2019
ms.author: diberry
ms.openlocfilehash: d9ae126753f55349f9bf3eefd20bc4d222866af1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73888210"
---
# <a name="how-to-use-the-dashboard-to-improve-your-app"></a>Uygulamanızı geliştirmek için Pano'yu kullanma

Örnek sözlerinizi kullanırken, eğitimli uygulamanızın amaçlarıyla ilgili sorunları bulun ve düzeltin. Pano, düzeltilmesi gereken niyet vurgularıyla birlikte genel uygulama bilgilerini görüntüler. 

Pano çözümlemesi yinelemeli bir işlemdir, modelinizi değiştirip geliştirirken tekrarlayın.

Bu sayfada, yalnızca desenli uygulamalar olarak bilinen, kasıtlı olarak örnek bir söz lemesi olmayan uygulamalar için ilgili analizler _olmayacaktır._ 

## <a name="what-issues-can-be-fixed-from-dashboard"></a>Panodan hangi sorunlar giderilebilir?

Panoda ele alınan üç sorun:

|Sorun|Grafik rengi|Açıklama|
|--|--|--|
|Veri dengesizliği|-|Bu, örnek sözleşmelerin miktarı önemli ölçüde değiştiğinde oluşur. Tüm niyetler, Yok niyeti _dışında, kabaca_ aynı sayıda örnek söze sahip olmak zorunda. Uygulamadaki toplam söyleyiş miktarının yalnızca %10-15'ine sahip olmalıdır.<br><br> Veriler dengesiz, ancak niyet doğruluğu belirli eşiğin üzerindeyse, bu dengesizlik bir sorun olarak bildirilmemiştir.<br><br>**Bu sorunla başlayın - diğer sorunların temel nedeni olabilir.**|
|Belirsiz tahminler|Orange|Bu, en yüksek niyet ve bir sonraki niyetin puanları, [olumsuz örnekleme](luis-how-to-train.md#train-with-all-data) veya daha fazla örnek sözleme nedeniyle bir sonraki eğitimde takla atabilecek kadar yakın olduğunda ortaya çıkar. |
|Yanlış tahminler|Red|Bu, etiketli amaç (içinde olduğu amaç) için bir örnek söyleyiş öngörülmediğinde oluşur.|

Doğru tahminler mavi renkle temsil edilir.

Pano bu sorunları gösterir ve hangi amaçların etkilendiğini söyler ve uygulamayı geliştirmek için ne yapmanız gerektiğini önerir. 

## <a name="before-app-is-trained"></a>Uygulama eğitilmeden önce 

Uygulamayı eğitmeden önce, pano düzeltmeler için herhangi bir öneri içermez. Bu önerileri görmek için uygulamanızı eğitin.  

## <a name="check-your-publishing-status"></a>Yayımlama durumunuzu kontrol edin

**Yayımlama durum** kartı, etkin sürümün son yayımı hakkında bilgi içerir. 

Etkin sürümün düzeltmek istediğiniz sürüm olup olmadığını denetleyin. 

![Pano, uygulamanın dış hizmetlerini, yayımlanmış bölgelerini ve toplu bitiş noktası isabetlerini gösterir.](./media/luis-how-to-use-dashboard/analytics-card-1-shows-app-summary-and-endpoint-hits.png)

Bu ayrıca herhangi bir dış hizmetleri, yayımlanmış bölgeleri ve toplu bitiş noktası isabetlerini gösterir. 

## <a name="review-training-evaluation"></a>Eğitim değerlendirmesini gözden geçirme

**Eğitim değerlendirme** kartı, uygulamanızın alana göre genel doğruluğunun toplu özetini içerir. Puan niyet kalitesini gösterir. 

![Eğitim değerlendirme kartı, uygulamanızın genel doğruluğu hakkında ilk bilgi alanını içerir.](./media/luis-how-to-use-dashboard/analytics-card-2-shows-app-overall-accuracy.png)

Grafik, doğru tahmin edilen amaçları ve farklı renklere sahip sorunlu alanları gösterir. Önerilerle uygulamayı geliştirin, bu puan artar. 

Önerilen düzeltmeler sorun türüne göre ayrılır ve uygulamanız için en önemli düzeltmelerdir. Amaç başına sorunları gözden geçirmek ve gidermek isterseniz, sayfanın alt kısmında hata kartı **[olan Intents'i](#intents-with-errors)** kullanın. 

Her sorun alanının düzeltilmesi gereken amaçları vardır. Niyet adını seçtiğinizde, **Niyet** sayfası dile gelenlere uygulanan bir filtreyle açılır. Bu filtre, soruna neden olan söyleyişlere odaklanmanızı sağlar.

### <a name="compare-changes-across-versions"></a>Sürümler arasında değişiklikleri karşılaştırma

Uygulamada değişiklik yapmadan önce yeni bir sürüm oluşturun. Yeni sürümde, niyetin örnek söyleyişlerinde önerilen değişiklikleri yapın, ardından tekrar eğitin. Pano sayfasının **Eğitim değerlendirme** kartında, değişiklikleri karşılaştırmak için **eğitilmiş sürümden göster değişikliğini** kullanın. 

![Sürümler arasında değişiklikleri karşılaştırma](./media/luis-how-to-use-dashboard/compare-improvement-across-versions.png)

### <a name="fix-version-by-adding-or-editing-example-utterances-and-retraining"></a>Örnek söz ve yeniden eğitim ekleyerek veya düzenleyerek sürümü düzeltme

Uygulamanızı düzeltmenin birincil yöntemi, örnek söz lerinizi eklemek veya yeniden eğitmek tir. Yeni veya değiştirilmiş söyleyinçler [çeşitli söyleymeler](luis-concept-utterance.md)için yönergeleri takip etmek gerekir.

Örnek söz ekleme, şu kişiler tarafından yapılmalıdır:

* farklı niyetlerde ne söylenmeleri olduğunu yüksek derecede anlayışa sahiptir.
* bir niyetle söylenmelerin başka bir niyetle nasıl karıştırılabileceğini bilir.
* sık sık birbiriyle karıştırılan iki niyetin tek bir niyete çöküp çökmeyeceğine karar verebilir. Bu durumda, farklı veriler varlıklarla birlikte çekilmesi gerekir.

### <a name="patterns-and-phrase-lists"></a>Desenler ve tümcecik listeleri

Analiz sayfası [desen](luis-concept-patterns.md) veya [ifade listelerinin](luis-concept-feature.md)ne zaman kullanılacağını belirtmez. Bunları eklerseniz, yanlış veya belirsiz tahminlerde yardımcı olabilir, ancak veri dengesizliği ile yardımcı olmaz. 

### <a name="review-data-imbalance"></a>Veri dengesizliğini gözden geçirme

Bu sorunla başlayın - diğer sorunların temel nedeni olabilir.

**Veri dengesizliği** niyet listesi, veri dengesizliğini düzeltmek için daha fazla söyleyiş gerektiren niyetleri gösterir. 

**Bu sorunu gidermek için:**

* Niyete daha fazla söz ekleyin ve sonra tekrar eğitin. 

Panoda önerilmedikçe Yok amacına söz eklemeyin.

> [!Tip]
> Sayfadaki üçüncü bölümü kullanın, **Utterances (sayı)** ayarı ile **niyet başına Söyleyerek,** hangi niyet daha fazla söyleyiş gerekir hızlı bir görsel kılavuz olarak.  
    ![Veri dengesizliği olan niyetleri bulmak için 'Söyleyişler (sayı)' kullanın.](./media/luis-how-to-use-dashboard/predictions-per-intent-number-of-utterances.png)

### <a name="review-incorrect-predictions"></a>Yanlış tahminleri gözden geçirme

**Yanlış tahmin** niyet listesi, belirli bir amaç için örnek olarak kullanılan, ancak farklı niyetler için öngörülen, söyleyiyi olan niyetleri gösterir. 

**Bu sorunu gidermek için:**

* Niyete daha açık olması için söyleyişleri edin ve tekrar eğitin.
* Eğer söyleyişçok yakından hizalanmış ve tekrar eğitmek ise niyetleri birleştirin.

### <a name="review-unclear-predictions"></a>Belirsiz tahminleri gözden geçirin

**Belirsiz tahmin** niyet listesi, en yakın rakibinden yeterince uzak olmayan tahmin puanları ile, bu söyleyiş için üst niyet [olumsuz örnekleme](luis-how-to-train.md#train-with-all-data)nedeniyle, bir sonraki eğitimde değişebilir tahmin puanları ile niyetleri gösterir .

**Bu sorunu gidermek için;**

* Niyete daha açık olması için söyleyişleri edin ve tekrar eğitin.
* Eğer söyleyişçok yakından hizalanmış ve tekrar eğitmek ise niyetleri birleştirin.

## <a name="utterances-per-intent"></a>Niyet başına söylenmeler

Bu kart, tüm amaçlı uygulamalarda genel uygulama durumunu gösterir. Eğer niyetleri düzeltmek ve yeniden eğitmek gibi, sorunlar için bu karta göz atmaya devam edin.

Aşağıdaki grafikte, düzeltilmesi gereken neredeyse hiçbir sorun olmayan dengeli bir uygulama gösterilmektedir.

![Aşağıdaki grafikte, düzeltilmesi gereken neredeyse hiçbir sorun olmayan dengeli bir uygulama gösterilmektedir.](./media/luis-how-to-use-dashboard/utterance-per-intent-shows-data-balance.png)

Aşağıdaki grafik, düzeltilmesi gereken birçok sorunla birlikte düşük dengeli bir uygulamayı gösterir.

![Aşağıdaki grafikte, düzeltilmesi gereken neredeyse hiçbir sorun olmayan dengeli bir uygulama gösterilmektedir.](./media/luis-how-to-use-dashboard/utterance-per-intent-shows-data-imbalance.png)

Niyet hakkında bilgi almak için her niyetin çubuğunun üzerine geçin. 

![Aşağıdaki grafikte, düzeltilmesi gereken neredeyse hiçbir sorun olmayan dengeli bir uygulama gösterilmektedir.](./media/luis-how-to-use-dashboard/utterances-per-intent-with-details-of-errors.png)

Bu sorunla ilgili en sorunlu amaclara odaklanabilmeniz için, amaçları sorun türüne göre düzenlemek için **Sıralama** özelliğini kullanın. 

## <a name="intents-with-errors"></a>Hataları olan niyetler

Bu kart, belirli bir amaç için sorunları gözden geçirmenize olanak tanır. Bu kartın varsayılan görünümü, çabalarınızı nereye odakladığınızı bilmeniz için en sorunlu amaçtır.

![Hata kartı olan Intents, belirli bir amaç için sorunları gözden geçirmenize olanak tanır. Kart varsayılan olarak en sorunlu niyetlere filtrelenir, böylece çabalarınızı nereye odakladığınızı bilirsiniz.](./media/luis-how-to-use-dashboard/most-problematic-intents-with-errors.png)

Üst donut grafik üç sorun türleri arasında niyet ile sorunları gösterir. Üç sorun türünde sorunlar varsa, her türün rakip niyetlerle birlikte aşağıda kendi grafiği vardır. 

### <a name="filter-intents-by-issue-and-percentage"></a>Amaçları soruna ve yüzdeye göre filtreleme

Kartın bu bölümü, hata eşiğinizden düşen örnek söyleyişleri bulmanızı sağlar. İdeal olarak doğru tahminlerin önemli olmasını istersiniz. Bu yüzde iş ve müşteri odaklı. 

İşletmeniz için rahat olduğunuz eşik yüzdelerini belirleyin. 

Filtre, belirli bir sorunla ilgili amaçları bulmanızı sağlar:

|Filtre|Önerilen yüzde|Amaç|
|--|--|--|
|En sorunlu niyetler|-|**Buradan başlayın** - Bu niyetle gelen belaları düzeltmek, uygulamayı diğer düzeltmelerden daha iyi hale getirecektir.|
|Aşağıdaki tahminleri düzeltin|%60|Bu, seçilen niyetteki doğru olan ancak eşiğin altında bir güven puanına sahip olan söyleyiş yüzdesidir. |
|Yukarıdaki belirsiz tahminler|%15|Bu, seçilen niyetteki ve en yakın rakip niyetle karıştırılan söyleyiş yüzdesidir.|
|Yukarıdaki yanlış tahminler|%15|Bu, seçili niyetteki yanlış tahmin edilen söyleyiş yüzdesidir. |

### <a name="correct-prediction-threshold"></a>Doğru tahmin eşiği

Size güvenli bir tahmin güven puanı nedir? Uygulama geliştirmenin başlangıcında hedefinizin %60'ı olabilir. Düzeltilmesi gereken seçili niyetle herhangi bir söyleyiş bulmak için aşağıdaki **doğru tahminleri** %60 oranında kullanın.

### <a name="unclear-or-incorrect-prediction-threshold"></a>Belirsiz veya yanlış tahmin eşiği

Bu iki filtre, eşiğinizin ötesinde seçili niyette söz bulmanızı sağlar. Bu iki yüzdeyi hata yüzdeleri olarak düşünebilirsiniz. Tahminler için %10-15 hata oranıyla rahatsanız, bu değerin üzerindeki tüm tüm sözcükleri bulmak için filtre eşiğini %15'e ayarlayın. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure kaynaklarınızı yönetme](luis-how-to-azure-subscription.md)
