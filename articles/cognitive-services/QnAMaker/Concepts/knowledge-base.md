---
title: Veri kaynaklarından alma - QnA Maker
description: QnA Maker bilgi tabanı, her QnA çiftiyle ilişkili bir dizi soru-cevap (QnA) kümesi ve isteğe bağlı meta verilerden oluşur.
ms.topic: conceptual
ms.date: 03/16/2020
ms.openlocfilehash: f648e15be803159dadb3f8bd047b2f46885eec91
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80804291"
---
# <a name="importing-from-data-sources"></a>Veri kaynaklarından içeri aktarma

Bilgi tabanı, ortak URL'ler ve dosyalar tarafından getirilen soru ve yanıt kümelerinden oluşur.

## <a name="data-source-locations"></a>Veri kaynağı konumları

İçerik bir veri kaynağından bir bilgi tabanına getirilir. Veri kaynağı konumları, kimlik doğrulaması gerektirmeyen **ortak URL'lerdir veya dosyalardır.**

Kimlik doğrulamayla güvenli olan [SharePoint dosyaları](../how-to/add-sharepoint-datasources.md)özel durumdur. SharePoint kaynakları web sayfaları değil, dosya olmalıdır. URL, '. gibi bir web uzantısı ile biterse. ASPX, SharePoint'ten QnA Maker'a almayacaktır.

## <a name="chit-chat-content"></a>Chit sohbet içeriği

Chit sohbet QnA içerik seti çeşitli dillerde ve konuşma stilleritam bir içerik veri kaynağı olarak sunulmaktadır. Bu bot kişiliği için bir başlangıç noktası olabilir, ve size zaman ve sıfırdan bunları yazma maliyeti kazandıracak. Bu içerik kümesini bilgi tabanınıza [nasıl ekleyeceğinizi](../how-to/chit-chat-knowledge-base.md) öğrenin.

## <a name="structured-data-format-through-import"></a>Alma yoluyla yapılandırılmış veri biçimi

Bilgi tabanının içe aktarılışı, varolan bilgi tabanının içeriğinin yerini alır. İçe aktarma, sorular ve yanıtlar içeren yapılandırılmış `.tsv` bir dosya gerektirir. Bu bilgiler, QnA Maker'ın soru-yanıt kümelerini gruplandırmasına ve bunları belirli bir veri kaynağına bağlamasına yardımcı olur.

| Soru  | Yanıt  | Kaynak| Meta veriler (1 anahtar: 1 değer) |
|-----------|---------|----|---------------------|
| Soru1 | Cevap1 | Url1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Soru2 | Cevap2 | Editoryal|    `Key:Value`       |

## <a name="structured-multi-turn-format-through-import"></a>Alma yoluyla yapılandırılmış çok dönüşlü biçim

Çok döndürme konuşmalarını `.tsv` dosya biçiminde oluşturabilirsiniz. Biçim, önceki sohbet günlüklerini (QnA Maker'ı kullanmayan diğer işlemlerle) analiz ederek çok döndürülme konuşmalarını oluşturmanızı ve ardından dosyayı otomasyon yoluyla oluşturmanızı `.tsv` sağlar. Varolan bilgi tabanını değiştirmek için dosyayı içe aktarın.

> [!div class="mx-imgBorder"]
> ![Çok döndürüredayalı soru 3 düzeyinin kavramsal modeli](../media/qnamaker-concepts-knowledgebase/nested-multi-turn.png)

Çok dönüşlü sütun, `.tsv`çoklu dönüşe özgü **Istemler'dir.** Excel'de gösterilen bir örnek, `.tsv`çok döndürülen alt çocukları tanımlamak için içerecek bilgileri gösterir:

```JSON
[
    {"displayOrder":0,"qnaId":2,"displayText":"Level 2 Question A"},
    {"displayOrder":0,"qnaId":3,"displayText":"Level 2 - Question B"}
]
```

**displayOrder** sayısaldır ve **displayText** işaretleme içermemesi gereken metindir.

> [!div class="mx-imgBorder"]
> ![Excel'de gösterildiği gibi çok döndürme soru örneği](../media/qnamaker-concepts-knowledgebase/multi-turn-tsv-columns-excel-example.png)

## <a name="export-as-example"></a>Örnek olarak dışa aktarma

`.tsv` QnA çiftinizi dosyada nasıl temsil edeceğinden emin değilseniz:
* [GitHub'dan bu indirilebilir örneği](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Structured-multi-turn-format.xlsx?raw=true) kullanma
* Veya QnA Maker portalında seti oluşturun, kaydedin ve ardından seti nasıl temsil edilecin bir örneği için bilgi tabanını dışa aktarın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası geliştirme yaşam döngüsü](./development-lifecycle-knowledge-base.md)

## <a name="see-also"></a>Ayrıca bkz.

Yanıtlarınızı biçimlendirmenize yardımcı olması için QnA Maker [Markdown başvurularını](../reference-markdown-format.md) kullanın.

[Soru-Cevap Oluşturma’ya genel bakış](../Overview/overview.md)

Bir bilgi tabanı oluşturun ve şularla edinin:
* [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase)
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebase?view=azure-dotnet)

Aşağıdakilerle bir yanıt oluşturun:
* [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer)
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.runtime?view=azure-dotnet)
