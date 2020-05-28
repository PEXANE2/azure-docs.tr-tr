---
title: Veri kaynaklarından içeri aktarma-Soru-Cevap Oluşturma
description: Soru-Cevap Oluşturma Bilgi Bankası, her QnA çiftiyle ilişkili bir soru-cevap (QnA) çiftleri ve isteğe bağlı meta veri kümesi içerir.
ms.topic: conceptual
ms.date: 03/16/2020
ms.openlocfilehash: eaa19cb2abf84f31cda9d8894e91ec1540980b27
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83993108"
---
# <a name="importing-from-data-sources"></a>Veri kaynaklarından içeri aktarma

Bilgi Bankası, ortak URL 'Ler ve dosyalar tarafından getirilen sorulardan ve yanıt çiftlerinden oluşur.

## <a name="data-source-locations"></a>Veri kaynağı konumları

İçerik bir veri kaynağından bilgi tabanına getirilir. Veri kaynağı konumları, kimlik doğrulaması gerektirmeyen **Genel URL 'ler veya dosyalardır**.

Kimlik doğrulamasıyla güvenli hale getirilen [SharePoint dosyaları](../how-to/add-sharepoint-datasources.md)özel durumdur. SharePoint kaynakları, Web sayfaları değil, dosyalar olmalıdır. URL'nin .ASPX gibi bir web uzantısıyla bitmesi durumunda SharePoint'ten Soru-Cevap Oluşturma hizmetine aktarma gerçekleştirilmez.

## <a name="chit-chat-content"></a>CHIT sohbet içeriği

CHIT sohbeti QnA içerik kümesi, çeşitli dillerde ve konuşma stillerinde tüm içerik veri kaynakları olarak sunulur. Bu, bot 'ın kişiliğine yönelik bir başlangıç noktası olabilir ve bu, bunları sıfırdan yazmanın zaman ve maliyetini kaydeder. Bilgi tabanınızı bu içerik kümesinin [nasıl ekleneceğini](../how-to/chit-chat-knowledge-base.md) öğrenin.

## <a name="structured-data-format-through-import"></a>İçeri aktarma yoluyla yapılandırılmış veri biçimi

Bilgi bankasını içeri aktarmak, mevcut bilgi bankasındaki içeriğin yerini alır. İçeri aktarma `.tsv` , sorular ve yanıt içeren yapılandırılmış bir dosya gerektirir. Bu bilgiler, soru-cevap çiftlerini ve onları belirli bir veri kaynağına göre gruplamak Soru-Cevap Oluşturma yardımcı olur.

| Soru  | Yanıt  | Kaynak| Meta veriler (1 anahtar: 1 değer) |
|-----------|---------|----|---------------------|
| Question1 | Answer1 | Url1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Question2 | Answer2 | Düzenleme|    `Key:Value`       |

## <a name="structured-multi-turn-format-through-import"></a>İçeri aktarma aracılığıyla yapılandırılmış çok yönlü biçim

Çoklu açma konuşmalarını bir `.tsv` dosya biçiminde oluşturabilirsiniz. Biçim, önceki sohbet günlüklerini çözümleyerek (Soru-Cevap Oluşturma kullanmayan diğer işlemlerle) birden çok yönlü konuşmaları oluşturma olanağı sağlar ve ardından `.tsv` dosyayı Otomasyon aracılığıyla oluşturabilir. Mevcut bilgi bankasını değiştirmek için dosyayı içeri aktarın.

> [!div class="mx-imgBorder"]
> ![3 düzey çoklu çift soruda kavramsal model](../media/qnamaker-concepts-knowledgebase/nested-multi-turn.png)

Birden çok açma için özel, birden çok yönlü bir sütun, `.tsv` **ister**. `.tsv`Excel 'de gösterilen, çoklu çift alt öğeleri tanımlamak için dahil edilecek bilgileri gösteren bir örnek:

```JSON
[
    {"displayOrder":0,"qnaId":2,"displayText":"Level 2 Question A"},
    {"displayOrder":0,"qnaId":3,"displayText":"Level 2 - Question B"}
]
```

**Displayorder** sayısal ve **metinmetni** markaşağı içermemelidir.

> [!div class="mx-imgBorder"]
> ![Excel 'de gösterildiği gibi çok yönlü soru örneği](../media/qnamaker-concepts-knowledgebase/multi-turn-tsv-columns-excel-example.png)

## <a name="export-as-example"></a>Örnek olarak dışarı aktar

Dosyada QnA çiftini nasıl temsil ettiğini bilmiyorsanız `.tsv` :
* Bu [indirilebilir örneği GitHub 'dan](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Structured-multi-turn-format.xlsx?raw=true) kullan
* Veya Soru-Cevap Oluşturma portalında çift oluşturun, bu çiftin nasıl kullanılacağına ilişkin bir örnek için Bilgi Bankası 'nı kaydedin ve dışarı aktarın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası geliştirme yaşam döngüsü](./development-lifecycle-knowledge-base.md)

## <a name="see-also"></a>Ayrıca bkz.

Yanıtlarınızı biçimlendirmeye yardımcı olması için Soru-Cevap Oluşturma [markın başvurusunu](../reference-markdown-format.md) kullanın.

[Soru-Cevap Oluşturma’ya genel bakış](../Overview/overview.md)

İle bir Bilgi Bankası oluşturun ve düzenleyin:
* [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase)
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebase?view=azure-dotnet)

Şunu kullanarak bir yanıt oluşturun:
* [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer)
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.runtime?view=azure-dotnet)
