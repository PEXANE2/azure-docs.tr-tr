---
title: Amaç Ekle-LUSıS
titleSuffix: Azure Cognitive Services
description: Aynı amaçları içeren soruların veya komutların gruplarını belirlemek için LUSıS uygulamanıza amaçlar ekleyin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.service: cognitive-services
ms.openlocfilehash: 1f2f001489552203f0157dd24356341eb3184c81
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73467554"
---
# <a name="add-intents-to-determine-user-intention-of-utterances"></a>Kullanıcı amaçlarını belirleme amacını ekleyin

Aynı amaca sahip olan soruların veya komutların gruplarını belirlemek için LUSıS uygulamanıza [amaçlar](luis-concept-intent.md) ekleyin. 

Amaçlar, üst gezinti çubuğunun **Build** bölümünden ve ardından sol bölmenin amaçlarından **yönetilir.** 

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

## <a name="add-intent"></a>Amaç ekleme

1. **Intents** (Amaçlar) sayfasında **Create new intent** (Yeni amaç oluştur) öğesini seçin.

1. **Yeni amaç oluştur** iletişim kutusunda, `GetEmployeeInformation`amaç adını girin ve **bitti**' ye tıklayın.

    ![Amaç Ekle](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

## <a name="add-an-example-utterance"></a>Örnek bir mekan ekleyin

Örnek söyleyler, Kullanıcı sorularının veya komutlarının metin örnekleridir. Language Understanding (LUSıS) öğretmek için bir amaca yönelik örnek ekleme yapmanız gerekir.

1. **GetEmployeeInformation** amaç ayrıntıları sayfasında, kullanıcılarınızın altındaki metin kutusunda `Does John Smith work in Seattle?` gibi, kullanıcılarınız için beklediğinizi ilgili bir yer girin ve ardından ENTER tuşuna basın.
 
    ![Ara vurgulanan, amaç Ayrıntıları sayfasının ekran görüntüsü](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png) 

    LUO, tüm aralıkları küçük harfe dönüştürür ve kısa çizgiler gibi belirteçlerin etrafına boşluk ekler.

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

## <a name="add-a-custom-entity"></a>Özel varlık ekleyin

Bir hedefe bir söylenişi eklendikten sonra, özel bir varlık oluşturmak için, söylenişi içinden metin seçebilirsiniz. Özel bir varlık, doğru amaç ile birlikte ayıklanmak üzere metin etiketlemek için bir yoldur. 

Daha fazla bilgi için bkz. [naterance 'e varlık ekleme](luis-how-to-add-example-utterances.md) .

## <a name="entity-prediction-discrepancy-errors"></a>Varlık tahmini tutarsızlık hataları 

Varlık [tahmin tutarsızlığı](luis-how-to-add-example-utterances.md#entity-status-predictions)göstermek için varlığın kırmızıyla altı çizilir. Bu, bir varlığın ilk oluşumu olduğundan, bu metnin doğru varlıkla etiketlenmesi için LUSıS 'nin yüksek güvenilirliğe sahip olması için yeterli örnek yoktur. Bu tutarsızlık, uygulama eğitilirken kaldırılır. 

![Amaç Ayrıntıları sayfasının ekran görüntüsü, mavi renkle vurgulanmış özel varlık adı](./media/luis-how-to-add-intents/create-custom-entity-name-blue-highlight.png) 

Metin, bir varlığı belirten mavi renkle vurgulanır.  

## <a name="add-a-prebuilt-entity"></a>Önceden oluşturulmuş bir varlık ekleyin

Daha fazla bilgi için bkz. [önceden oluşturulmuş varlık](luis-how-to-add-entities.md#add-a-prebuilt-entity-to-your-app).

## <a name="using-the-contextual-toolbar"></a>Bağlamsal araç çubuğunu kullanma

Listede bir veya daha fazla örnek utde seçildiğinde, söylenişi 'in solundaki kutuyu işaretleyerek, söylenişi listesinin üzerindeki araç çubuğu aşağıdaki eylemleri gerçekleştirmenize olanak tanır:

* Amacı yeniden atama: utterleri farklı bir amaca taşıyın
* Utbotları Sil
* Varlık filtreleri: yalnızca filtrelenmiş varlıkları içeren utbotları göster
* Yalnızca tüm/hataları göster: tahmin hatalarıyla birlikte bulunan ve tüm söyleyeni gösteren utbotları göster
* Varlıklar/belirteçler görünümü: varlık adlarıyla varlıkları görünümü gösterme veya söylenişi ham metnini gösterme
* Büyüteç: belirli bir metni içeren söyleylerini arama

## <a name="working-with-an-individual-utterance"></a>Tek bir söylenişi ile çalışma

Aşağıdaki eylemler, üç nokta menüsünden söylenişi 'in sağına doğru bir şekilde gerçekleştirilebilir:

* Düzenle: söylenişi 'in metnini değiştirme
* Sil: amacınızı amaç 'dan kaldırın. Yine de isterseniz, daha iyi bir yöntem onu **none** amacına taşımaktır. 
* Desen ekleme: bir desen, yaygın bir şekilde bir zaman ve değiştirilebilir metin ve yoksayılabilir metin işaretlemenizi sağlar ve bu sayede, amaç açısından daha fazla duyun gereksinimini azaltır. 

**Etiketli amaç** sütunu, utterance 'in amacını değiştirmenize olanak sağlar.

## <a name="train-your-app-after-changing-model-with-intents"></a>Bir modeli amaçlar ile değiştirdikten sonra uygulamanızı eğitme

Hedefleri ekledikten, düzenledikten veya kaldırdıktan sonra, değişikliklerinizin uç nokta sorgularına uygulanması için uygulamanızı [eğitin](luis-how-to-train.md) ve [yayımlayın](luis-how-to-publish-app.md) . 

## <a name="next-steps"></a>Sonraki adımlar

Varlıklarla [örnek](luis-how-to-add-example-utterances.md) ekleme hakkında daha fazla bilgi edinin. 
