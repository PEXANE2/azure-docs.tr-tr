---
title: 'Quickstart: QnA Maker portalında soru ve yanıt ekleme'
description: Bu hızlı başlangıç, kullanıcılarınızın sorularına doğru yanıtı bulabilmeleri için meta verilerle soru ve yanıt kümelerinin nasıl ekleyeceğini gösterir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 02/08/2020
ms.author: diberry
ms.openlocfilehash: f067bae55c38fc783c12bf9d0bc6fbcdf881e4e4
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80756692"
---
# <a name="quickstart-add-questions-and-answer-with-qna-maker-portal"></a>Quickstart: QnA Maker portalı ile soru ekleme ve yanıtla

Bir bilgi tabanı oluşturulduktan sonra, yanıtı filtrelemek için meta verilerle soru ve yanıt (QnA) kümeleri ekleyin. Aşağıdaki tablodaki sorular Azure hizmet sınırlarıyla ilgilidir, ancak her birinin farklı bir Azure hizmetiyle ilgisi vardır.

<a name="qna-table"></a>

|Ayarla|Sorular|Yanıt|Meta Veriler|
|--|--|--|--|
|1|`How large a knowledge base can I create?`<br><br>`What is the max size of a knowledge base?`<br><br>`How many GB of data can a knowledge base hold?` |`The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment) for more details.`|`service=qna_maker`<br>`link_in_answer=true`|
|2|`How many knowledge bases can I have for my QnA Maker service?`<br><br>`I selected a Azure Cognitive Search tier that holds 15 knowledge bases, but I can only create 14 - what is going on?`<br><br>`What is the connection between the number of knowledge bases in my QnA Maker service and the Azure Cognitive Search service size?` |`Each knowledge base uses 1 index, and all the knowledge bases share a test index. You can have N-1 knowledge bases where N is the number of indexes your Azure Cognitive Search tier supports.`|`service=search`<br>`link_in_answer=false`|

Bir QnA çiftine meta veriler eklendikten sonra, istemci uygulaması şunları yapabilir:

* Yalnızca belirli meta verilerle eşleşen yanıtlar isteyin.
* Her yanıt için meta verilere bağlı olarak tüm yanıtları alın, ancak işlem sonrası yanıtları alın.


## <a name="prerequisites"></a>Ön koşullar

* Önceki [hızlı başlatmayı](./create-publish-knowledge-base.md) tamamlayın

## <a name="sign-in-to-the-qna-maker-portal"></a>QnA Maker portalında oturum açın

1. [QnA Maker portalında](https://www.qnamaker.ai)oturum açın.

1. Önceki [hızlı başlangıçtan](../how-to/create-knowledge-base.md)mevcut bilgi tabanınızı seçin.

## <a name="add-additional-alternatively-phrased-questions"></a>Alternatif olarak ifade edilen ek sorular ekleme

Geçerli bilgi tabanı QnA Maker sorun giderme QnA çiftleri vardır. Bu kümeler, oluşturma işlemi sırasında URL bilgi tabanına eklendiğinde oluşturulmuştur.

Bu URL içe aktarıldığında, tek yanıtlı tek bir soru oluşturuldu. Bu yordamda, ek sorular ekleyin.

1. Soruyu **bulmak için, Sorguyu Ve** Yanıt kümelerinin üzerindeki arama textbox'ını kullan`How large a knowledge base can I create?`

1. **Soru** sütununda + **Alternatif tümce ekle'yi** seçin ve ardından aşağıdaki tabloda sağlanan her yeni tümceyi ekleyin.

    |Alternatif tümce|
    |--|
    |`What is the max size of a knowledge base?`|
    |`How many GB of data can a knowledge base hold?`|

1. **Kaydet'i** seçin ve bilgi tabanını yeniden eğitmek için eğitin.

1. **Test'i**seçin, ardından yeni alternatif tümcelerden birine yakın olan ancak tam olarak aynı ifadelere sahip olmayan bir soru girin:

    `What GB size can a knowledge base be?`

    Doğru yanıt işaretleme biçiminde döndürülür:

    `The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment) for more details.`

    İade edilen yanıtın altında **İncele'yi** seçerseniz, aynı yüksek güven düzeyine sahip olmamakla birlikte daha fazla yanıtın soruya yanıt verildiğini görebilirsiniz.

    Alternatif tümcelerin olası her kombinasyonunu eklemeyin. QnA Maker'ın aktif [öğrenimini](../how-to/improve-knowledge-base.md)açtığınızda, bu, bilgi tabanınızın kullanıcılarınızın ihtiyaçlarını karşılamaya en iyi şekilde yardımcı olacak alternatif tümceleri bulur.

1. Test penceresini kapatmak için Yeniden **Test'i** seçin.

## <a name="add-metadata-to-filter-the-answers"></a>Yanıtları filtrelemek için meta veri ekleme

Soru ve yanıt kümesine meta veri eklemek, istemci uygulamanızın filtreuygulanmış yanıtlar istemesine olanak tanır. Bu [filtre, birinci ve ikinci sıralayıcılar](../concepts/query-knowledge-base.md#ranker-process) uygulanmadan önce uygulanır.

1. [Bu hızlı başlatmadaki ilk tablodan](#qna-table)meta veriler olmadan ikinci soru ve yanıt kümesini ekleyin, ardından aşağıdaki adımlarla devam edin.

1. **Görünüm seçeneklerini**seçin, ardından meta **verileri göster'i**seçin.

1. Az önce eklediğiniz QnA çifti için **meta veri etiketleri ekle'yi**seçin, ardından adını `service` ve değerini `search`ekleyin. Bu gibi görünüyor: `service:search`.

1. Adı `link_in_answer` ve değeri ile başka bir `false`meta veri etiketi ekleyin. Bu gibi görünüyor: `link_in_answer:false`.

1. Tablodaki ilk yanıtı ara. `How large a knowledge base can I create?`

1. Aynı iki meta veri etiketi için meta veri çiftleri ekleyin:

    `link_in_answer` : `true`<br>
    `server`: `qna_maker`

    Şimdi farklı değerlere sahip aynı meta veri etiketleri ile iki soru var.

1. **Kaydet'i** seçin ve bilgi tabanını yeniden eğitmek için eğitin.

1. Yayımlama sayfasına gitmek için üst menüde **Yayımla'yı** seçin.
1. Geçerli bilgi tabanını bitiş noktasına yayınlamak için **Yayımla** düğmesini seçin.
1. Bilgi bankası yayınlandıktan sonra, bilgi tabanınızdan nasıl bir yanıt oluşturacağınızı öğrenmek için bir sonraki hızlı başlangıcıza devam edin.

## <a name="what-did-you-accomplish"></a>Ne başardın?

Daha fazla soruyu desteklemek için bilgi tabanınızı düzenlediniz ve yanıt veya yanıtlar döndürüldünden sonra en iyi yanıtı veya postprocessing'i arama sırasında filtrelemayı desteklemek için ad/değer çiftleri sağladınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki hızlı başlatmaya devam etmiyorsanız, Azure portalındaki QnA Maker ve Bot çerçeve kaynaklarını silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Postman veya cURL ile yanıtlar alma](get-answer-from-knowledge-base-using-url-tool.md)