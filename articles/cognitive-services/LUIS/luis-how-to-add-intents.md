---
title: Amaç Ekle-LUSıS
titleSuffix: Azure Cognitive Services
description: Intents sorularınız ya da aynı amaçları olan komutları gruplarını tanımlamak için LUIS uygulamanızı ekleyin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.service: cognitive-services
ms.openlocfilehash: eb90a902b8f7fe8b37b81c2825cbdfc25ef5dc0d
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68932886"
---
# <a name="add-intents-to-determine-user-intention-of-utterances"></a>Kullanıcı amaçlarını belirleme amacını ekleyin

Ekleme [hedefleri](luis-concept-intent.md) LUIS uygulamanızı sorularınız ya da bağdaştırıcılar aynı amaca sahip komutları gruplarını tanımlamak için. 

Hedefleri, üst gezinti çubuğundan 's yönetilir **derleme** bölümünden, ardından sol bölmenin **hedefleri**. 

## <a name="add-intent"></a>Amaç ekleme

1. **Intents** (Amaçlar) sayfasında **Create new intent** (Yeni amaç oluştur) öğesini seçin.

1. İçinde **yeni hedefi oluşturma** iletişim kutusunda, hedefi adı girin `GetEmployeeInformation`, tıklatıp **Bitti**.

    ![Hedefi ekleyin](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

## <a name="add-an-example-utterance"></a>Bir örnek utterance Ekle

Örnek konuşma metin kullanıcı sorularınız ya da komutları örnekleridir. Language Understanding (LUIS) öğretmeyi bir amaç için örnek konuşma eklemeniz gerekir.

1. Üzerinde **GetEmployeeInformation** hedefi Ayrıntıları sayfasında, beklediğiniz kullanıcılarınızdan gelen gibi ilgili bir utterance girin `Does John Smith work in Seattle?` metin kutusuna aşağıdaki hedefi adını ve Enter tuşuna basın.
 
    ![Vurgulanan utterance ile ekran görüntüsü, hedefleri Ayrıntıları sayfası](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png) 

    LUIS, tüm konuşma küçük harfe dönüştürür ve kısa çizgi gibi belirteçleri boşluk ekler.

<a name="#intent-prediction-discrepancy-errors"></a>

## <a name="intent-prediction-errors"></a>Amaç tahmin hataları 

Bir amaca göre örnek, bu örnekte yer aldığı amaç ve eğitim sırasında belirlenen tahmini amaç arasında bir amaç tahmin hatası olabilir. 

Söylenişi tahmin hatalarını bulmak ve bunları onarmak için, **filtre** seçeneğinin **değerlendirme** seçeneklerini yanlış ve **ayrıntılı görünümün** **Görünüm** seçeneğiyle birlikte belirsiz şekilde kullanın. 

![Söylenişi tahmin hatalarını bulmak ve bunları onarmak için filtre seçeneğini kullanın.](./media/luis-how-to-add-intents/find-intent-prediction-errors.png)

Filtreler ve görünüm uygulandığında ve hatalar ile ilgili örnek bir sorun oluşursa, örnek bir liste, söz konusu konuları ve sorunları gösterir.

![! [Filtreler ve görünüm uygulandığında ve hatalar ile ilgili örnekler varsa, örnek utterler listesi, bu noktalar ve sorunları gösterir.] (./Media/Luis-How-to-add-BITS/Find-Errors-ın-utterances.exe)](./media/luis-how-to-add-intents/find-errors-in-utterances.png#lightbox)

Her satır, bu iki puandaki fark olan en yakın Rival 'in puanı olan utterance için geçerli eğitimin tahmin Puanını gösterir. 

### <a name="fixing-intents"></a>Amaçlar düzeltiliyor

Amaç tahmin hatalarını nasıl düzelteceğinizi öğrenmek için [Özet panosunu](luis-how-to-use-dashboard.md)kullanın. Özet Panosu, etkin sürümün son eğitimi için analiz sağlar ve modelinizi gidermeye yönelik en önemli önerileri sunar.  

## <a name="add-a-custom-entity"></a>Özel bir varlık ekleme

Bir amaç için bir utterance eklendikten sonra bir özel varlık oluşturma utterance metni seçebilirsiniz. Özel bir varlık ayıklama, doğru amaç birlikte etiket metni bir yoludur. 

Daha fazla bilgi için bkz. [naterance 'e varlık ekleme](luis-how-to-add-example-utterances.md) .

## <a name="entity-prediction-discrepancy-errors"></a>Varlık tahmin tutarsızlık hataları 

Varlık kırmızı renkte göstermek için altı çizili olup bir [varlık tahmin tutarsızlık](luis-how-to-add-example-utterances.md#entity-status-predictions). Bu varlığın ilk geçtiği yeri olduğundan yok yeterli örnekler LUIS Yüksek Güvenilirlikli olması için bu metni doğru varlığı ile etiketlenir. Bu uyuşmazlık, uygulamayı eğitildi kaldırılır. 

![Ekran görüntüsü, hedefleri Ayrıntıları sayfası, mavi renkle vurgulandığı özel varlık adı](./media/luis-how-to-add-intents/create-custom-entity-name-blue-highlight.png) 

Metin gösteren bir varlık mavi renkle vurgulanır.  

## <a name="add-a-prebuilt-entity"></a>Önceden oluşturulmuş bir varlık ekleme

Bilgi için [önceden oluşturulmuş varlık](luis-how-to-add-entities.md#add-a-prebuilt-entity-to-your-app).

## <a name="using-the-contextual-toolbar"></a>Bağlamsal araç çubuğunu kullanma

Listede bir veya daha fazla örnek utde seçildiğinde, söylenişi 'in solundaki kutuyu işaretleyerek, söylenişi listesinin üzerindeki araç çubuğu aşağıdaki eylemleri gerçekleştirmenize olanak tanır:

* Yeniden atama hedefi: farklı eylemlerinize utterance(s) Taşı
* Utterance(s) Sil
* Varlık filtreleri: yalnızca filtrelenen varlık içeren konuşma Göster
* Tümünü Göster / yalnızca hataları: tahmin hatalı konuşma ya da tüm konuşma göstermek
* Varlıkları/belirteç görünümü: varlık adları ile varlıkları görünümünü göster veya utterance ham metni göster
* Büyüteç: belirli bir metni içeren konuşma arayın

## <a name="working-with-an-individual-utterance"></a>Tek bir utterance ile çalışma

Aşağıdaki eylemleri utterance sağındaki üç nokta menüsünden bir bireysel utterance gerçekleştirilebilir:

* Düzen: utterance metnini değiştirme
* Sil: utterance amacından kaldırın. Utterance hala istiyorsanız taşımak için daha iyi bir yöntem olan **hiçbiri** hedefi. 
* Bir model ekleyin: Bir desen, yaygın bir şekilde bir işlem yapmanıza ve değiştirilebilir metin ve yoksayılabilir metinlere işaret etmenize olanak tanır ve bu sayede, amaç açısından daha fazla yer gereksinimini azaltır. 

**Hedefi etiketli** sütun utterance amacı değiştirmenize olanak sağlar.

## <a name="train-your-app-after-changing-model-with-intents"></a>Intents modeliyle değiştirdikten sonra uygulamanızı eğitin

Ekleme, düzenleme veya kaldırma amacı, sonra [eğitme](luis-how-to-train.md) ve [yayımlama](luis-how-to-publish-app.md) uygulamanızı değişikliklerinizi uç nokta sorguları uygulanması. 

## <a name="next-steps"></a>Sonraki adımlar

Ekleme hakkında daha fazla bilgi edinin [örnek konuşma](luis-how-to-add-example-utterances.md) varlıklarla. 
