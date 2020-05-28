---
title: 'Hızlı başlangıç: Soru-Cevap Oluşturma portalına soru ve yanıt ekleme'
description: Bu hızlı başlangıçta, kullanıcılarınızın sorularınızın doğru yanıtını bulabilmesi için meta verilerle soru ve yanıt çiftleri ekleme gösterilmektedir.
ms.topic: quickstart
ms.date: 05/26/2020
ms.openlocfilehash: fc40c6a6e0543d4d558da2c8a17c155fbb7cbf1f
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83994672"
---
# <a name="quickstart-add-questions-and-answer-with-qna-maker-portal"></a>Hızlı başlangıç: Soru-Cevap Oluşturma portalı ile soru ve yanıt ekleme

Bilgi Bankası oluşturulduktan sonra, yanıtı filtrelemek için meta verilerle soru ve cevap (QnA) çiftleri ekleyin. Aşağıdaki tablodaki sorular Azure hizmet limitleri ile ilgilidir, ancak her birinin farklı bir Azure hizmeti ile yapması gerekir.

<a name="qna-table"></a>

|Çifti|Sorular|Yanıt|Meta Veriler|
|--|--|--|--|
|1|`How large a knowledge base can I create?`<br><br>`What is the max size of a knowledge base?`<br><br>`How many GB of data can a knowledge base hold?` |`The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment) for more details.`|`service=qna_maker`<br>`link_in_answer=true`|
|2|`How many knowledge bases can I have for my QnA Maker service?`<br><br>`I selected a Azure Cognitive Search tier that holds 15 knowledge bases, but I can only create 14 - what is going on?`<br><br>`What is the connection between the number of knowledge bases in my QnA Maker service and the Azure Cognitive Search service size?` |`Each knowledge base uses 1 index, and all the knowledge bases share a test index. You can have N-1 knowledge bases where N is the number of indexes your Azure Cognitive Search tier supports.`|`service=search`<br>`link_in_answer=false`|

Bir QnA çiftine meta veriler eklendikten sonra istemci uygulama şunları yapabilir:

* Yalnızca belirli meta verilerle eşleşen yanıt isteyin.
* Tüm yanıtları al, ancak her yanıt için meta verilere bağlı olarak yanıtları son işle.


## <a name="prerequisites"></a>Ön koşullar

* [Önceki hızlı](./create-publish-knowledge-base.md) başlangıcı doldurun

## <a name="sign-in-to-the-qna-maker-portal"></a>Soru-Cevap Oluşturma portalında oturum açın

1. [Soru-cevap oluşturma portalında](https://www.qnamaker.ai)oturum açın.

1. [Önceki hızlı](../how-to/create-knowledge-base.md)başlangıçta mevcut bilgi bankasını seçin.

## <a name="add-additional-alternatively-phrased-questions"></a>Ek alternatif olarak phrased soruları ekleyin

Geçerli Bilgi Bankası 'nda Soru-Cevap Oluşturma sorun giderme QnA çiftleri vardır. Bu çiftler, oluşturma işlemi sırasında URL Bilgi Bankası 'na eklendiğinde oluşturulmuştur.

Bu URL içeri aktarıldığında, yalnızca bir yanıtla birlikte tek bir soru oluşturulur. Bu yordamda, ek sorular ekleyin.

1. **Düzenleme** sayfasında, soruyu bulmak için soru ve yanıt çiftlerinin üzerindeki arama metin kutusunu kullanın`How large a knowledge base can I create?`

1. **Soru** sütununda **+ Alternatif ifade Ekle** ' yi seçin ve aşağıdaki tabloda belirtilen her yeni bir ifade ekleyin.

    |Alternatif ifade|
    |--|
    |`What is the max size of a knowledge base?`|
    |`How many GB of data can a knowledge base hold?`|

1. Bilgi Bankası 'nı yeniden eğitmek için **Kaydet ve eğitme '** yi seçin.

1. **Test**' i seçin, ardından yeni alternatif Phen ' dan birine yakın bir soru girin, ancak tamamen aynı şekilde değildir:

    `What GB size can a knowledge base be?`

    Doğru yanıt markaşağı biçiminde döndürülür:

    `The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment) for more details.`

    Döndürülen yanıtın altında **İncele** ' yi seçerseniz, soruyu karşılayan daha fazla yanıt görebilirsiniz, ancak aynı yüksek düzeyde güvenle bilgi alabilirsiniz.

    Alternatif özelliklerin her olası birleşimini eklemeyin. Soru-Cevap Oluşturma [etkin öğrenimini](../how-to/improve-knowledge-base.md)açtığınızda, bu, bilgi tabanınızın kullanıcılarınızın ihtiyaçlarını karşılayacak en iyi şekilde yardımcı olacak alternatif duymalları bulur.

1. Test penceresini kapatmak için yeniden **Test** ' i seçin.

## <a name="add-metadata-to-filter-the-answers"></a>Yanıtları filtrelemek için meta veriler ekleyin

Bir soru ve yanıt çiftinin meta verilerini eklemek, istemci uygulamanızın filtrelenmiş yanıtlar istemesine izin verir. Bu filtre, [ilk ve ikinci ranranlar](../concepts/query-knowledge-base.md#ranker-process) uygulanmadan önce uygulanır.

1. İkinci soruyu ve yanıt çiftini, [Bu hızlı başlangıçtaki ilk tablodan](#qna-table)meta veriler olmadan ekleyin, ardından aşağıdaki adımlarla devam edin.

1. **Görünüm seçeneklerini**belirleyin ve **meta verileri göster**' i seçin.

1. Az önce eklediğiniz QnA çifti için, **meta veri etiketleri Ekle**' yi seçin, sonra adını `service` ve değerini ekleyin `search` . Şöyle görünür: `service:search` .

1. Adı ve değeri olan başka bir meta veri etiketi ekleyin `link_in_answer` `false` . Şöyle görünür: `link_in_answer:false` .

1. Tablodaki ilk yanıtı arayın, `How large a knowledge base can I create?` .

1. Aynı iki meta veri etiketi için meta veri çiftleri ekleyin:

    `link_in_answer` : `true`<br>
    `server`: `qna_maker`

    Artık, farklı değerlerle aynı meta veri etiketlerine sahip iki sorunuz vardır.

1. Bilgi Bankası 'nı yeniden eğitmek için **Kaydet ve eğitme '** yi seçin.

1. Yayımla sayfasına gitmek için üstteki menüden **Yayımla** ' yı seçin.
1. Geçerli bilgi bankasını uç noktaya yayımlamak için **Yayımla** düğmesini seçin.
1. Bilgi Bankası yayımlandıktan sonra, bilgi tabanınızdan nasıl yanıt oluşturacağınızı öğrenmek için sonraki hızlı başlangıca geçin.

## <a name="what-did-you-accomplish"></a>Ne başardınız?

Bilgi bankanızı, daha fazla soru ve verilen ad/değer çiftlerini, yanıt veya Yanıt döndürülbaşladıktan sonra, en üstteki yanıt veya geri yükleme sırasında filtrelemeyi destekleyecek şekilde düzenlediniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki hızlı başlangıca devam değilseniz, Azure portal Soru-Cevap Oluşturma ve bot Framework kaynaklarını silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Postman veya cURL ile yanıtlar alma](get-answer-from-knowledge-base-using-url-tool.md)