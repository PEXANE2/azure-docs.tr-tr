---
title: Veri kaynaklarından içeri aktarma-Soru-Cevap Oluşturma
description: Soru-Cevap Oluşturma Bilgi Bankası, her QnA çiftiyle ilişkili bir soru-cevap (QnA) kümesi ve isteğe bağlı meta veri kümesinden oluşur.
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: d47d994366a8057521c1cc2ab1ab8a7ec3393965
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78389162"
---
# <a name="importing-from-data-sources"></a>Veri kaynaklarından içeri aktarma

Bilgi Bankası, genel URL 'Ler ve dosyalar tarafından getirilen soru ve yanıt kümelerinden oluşur.

## <a name="data-source-locations"></a>Veri kaynağı konumları

İçerik bir veri kaynağından bilgi tabanına getirilir. Veri kaynağı konumları, kimlik doğrulaması gerektirmeyen **Genel URL 'ler veya dosyalardır**.

Kimlik doğrulamasıyla güvenli hale getirilen [SharePoint dosyaları](../how-to/add-sharepoint-datasources.md)özel durumdur. SharePoint kaynakları, Web sayfaları değil, dosyalar olmalıdır. URL, gibi bir Web uzantısıyla sonlanıyorsa. ASPX, SharePoint 'ten Soru-Cevap Oluşturma içine aktarılmaz.

## <a name="chit-chat-content"></a>CHIT sohbet içeriği

CHIT sohbeti QnA içerik kümesi, çeşitli dillerde ve konuşma stillerinde tüm içerik veri kaynakları olarak sunulur. Bu botunuzun ait kişilik için bir başlangıç noktası olabilir ve bu saat ve bunları sıfırdan yazmak maliyeti kaydedecek. Bilgi tabanınızı bu içerik kümesinin [nasıl ekleneceğini](../how-to/chit-chat-knowledge-base.md) öğrenin.

## <a name="structured-data-format-through-import"></a>İçeri aktarma ile yapılandırılmış veri biçimi

Bilgi Bankası içeri aktarma, var olan bir Bilgi Bankası içeriğini değiştirir. İçeri aktarma, sorular ve yanıt içeren bir yapılandırılmış `.tsv` dosyası gerektirir. Bu bilgiler, soru-cevap kümelerini ve özniteliklerini belirli bir veri kaynağına göre gruplamak Soru-Cevap Oluşturma yardımcı olur.

| Soru  | Yanıt  | Kaynak| Meta veriler (1 anahtar: 1 değer) |
|-----------|---------|----|---------------------|
| Question1 | Answer1 | Url1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Question2 | Answer2 | Düzenleme|    `Key:Value`       |

## <a name="structured-multi-turn-format-through-import"></a>İçeri aktarma aracılığıyla yapılandırılmış çok yönlü biçim

Çoklu açma konuşmalarını bir `.tsv` dosya biçiminde oluşturabilirsiniz. Biçim, önceki sohbet günlüklerini çözümleyerek (Soru-Cevap Oluşturma kullanmayan diğer işlemlerle) birden çok yönlü konuşmaları oluşturma olanağı sağlar ve ardından Otomasyon aracılığıyla `.tsv` dosyası oluşturabilir. Mevcut bilgi bankasını değiştirmek için dosyayı içeri aktarın.

> [!div class="mx-imgBorder"]
> 3 düzeyden çok yönlü soruda kavramsal model ![](../media/qnamaker-concepts-knowledgebase/nested-multi-turn.png)

Birden çok yönlü `.tsv`için olan sütun, çoklu-açma **Istemlerine**özgüdür. Excel 'de gösterilen örnek `.tsv`, çoklu çift alt öğeleri tanımlamak için dahil edilecek bilgileri gösterir:

```JSON
[
    {"displayOrder":0,"qnaId":2,"displayText":"Level 2 Question A"},
    {"displayOrder":0,"qnaId":3,"displayText":"Level 2 - Question B"}
]
```

**Displayorder** sayısal ve **metinmetni** markaşağı içermemelidir.

> [!div class="mx-imgBorder"]
> Excel 'de gösterildiği gibi çok yönlü soru örneğini ![](../media/qnamaker-concepts-knowledgebase/multi-turn-tsv-columns-excel-example.png)

## <a name="export-as-example"></a>Örnek olarak dışarı aktar

`.tsv` dosyasında QnA kümesini nasıl temsil ettiğini bilmiyorsanız, kümeyi temsil eden bir örnek için Soru-Cevap Oluşturma portalında kümeyi oluşturun, kaydedin ve Bilgi Bankası 'nı dışarı aktarın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankasındaki geliştirme yaşam döngüsü](./development-lifecycle-knowledge-base.md)

## <a name="see-also"></a>Ayrıca bkz.

Yanıtlarınızı biçimlendirmeye yardımcı olması için Soru-Cevap Oluşturma [markın başvurusunu](../reference-markdown-format.md) kullanın.

[Soru-Cevap Oluşturma’ya genel bakış](../Overview/overview.md)

İle bir Bilgi Bankası oluşturun ve düzenleyin:
* [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase)
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebase?view=azure-dotnet)

Şunu kullanarak bir yanıt oluşturun:
* [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer)
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.runtime?view=azure-dotnet)
