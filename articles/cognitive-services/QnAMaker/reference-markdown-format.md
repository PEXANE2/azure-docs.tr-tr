---
title: Markdown formatı - QnA Maker
description: QnA Maker'ın yanıt metninde kullanabileceğiniz işaretleme biçimlerinin listesi aşağıdadır.
ms.topic: reference
ms.date: 01/09/2020
ms.openlocfilehash: 3fb7d73afdfd5ab7f1fb56a685b21538b97c8ea4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77045406"
---
# <a name="markdown-format-supported-in-qna-maker-answer-text"></a>QnA Maker yanıt metninde desteklenen Markdown biçimi

QnA Maker, yanıt metnini işaretleme olarak saklar. İşaretlenin birçok tadı vardır. Yanıt metninin döndürülüp doğru görüntülendiğinden emin olmak için bu başvuruyu kullanın.

Markdown'ınızı doğrulamak için **[CommonMark](https://commonmark.org/help/tutorial/index.html)** öğreticisini kullanın. Öğretici, hızlı kopyalama/yapıştır ma doğrulama için **bir Try it** özelliğine sahiptir.

## <a name="supported-markdown-format"></a>Desteklenen işaretleme biçimi

QnA Maker'ın yanıt metninde kullanabileceğiniz işaretleme biçimlerinin listesi aşağıdadır.

|Amaç|Biçimlendir|Örnek markdown|İşleme<br>Chat bot'ta görüntülendiği gibi|
|--|--|--|--|
2 cümle arasında yeni bir satır.|`\n\n`|`How can I create a bot with \n\n QnA Maker?`|![iki cümle arasında biçimlendirme yeni bir çizgi](./media/qnamaker-concepts-datasources/format-newline.png)|
|H1'den h6'ya kadar `#` olan üstbilgi, hangi üstbilginin sayısını gösterir. 1 `#` h1'dir.|`\n# text \n## text \n### text \n####text \n#####text` |`## Creating a bot \n ...text.... \n### Important news\n ...text... \n### Related Information\n ....text...`<br><br>`\n# my h1 \n## my h2\n### my h3 \n#### my h4 \n##### my h5`|![işaretleme üstbilgileriyle biçimlendirme](./media/qnamaker-concepts-datasources/format-headers.png)<br>![işaretleme üstbilgisi H1'den H5'e biçimlendirme](./media/qnamaker-concepts-datasources/format-h1-h5.png)|
|İtalik |`*text*`|`How do I create a bot with *QnA Maker*?`|![italik ile biçimlendirme](./media/qnamaker-concepts-datasources/format-italics.png)|
|Güçlü (kalın)|`**text**`|`How do I create a bot with **QnA Maker**?`|![kalın için güçlü işaretleme ile biçim](./media/qnamaker-concepts-datasources/format-strong.png)|
|Bağlantı için URL|`[text](https://www.my.com)`|`How do I create a bot with [QnA Maker](https://www.qnamaker.ai)?`|![URL için biçim (köprü)](./media/qnamaker-concepts-datasources/format-url.png)|
|*Genel resim için URL|`![text](https://www.my.com/image.png)`|`How can I create a bot with ![QnAMaker](https://review.docs.microsoft.com/azure/cognitive-services/qnamaker/media/qnamaker-how-to-key-management/qnamaker-resource-list.png)`|![genel resim URL'si için biçim ](./media/qnamaker-concepts-datasources/format-image-url.png)|
|Üstü çizili|`~~text~~`|`some ~~questoins~~ questions need to be asked`|![strikethrough için biçim](./media/qnamaker-concepts-datasources/format-strikethrough.png)|
|Kalın ve italik|`***text***`|`How can I create a ***QnA Maker*** bot?`|![kalın ve italik için biçim](./media/qnamaker-concepts-datasources/format-bold-italics.png)|
|Bağlantı için kalın URL|`[**text**](https://www.my.com)`|`How do I create a bot with [**QnA Maker**](https://www.qnamaker.ai)?`|![kalın URL için biçim](./media/qnamaker-concepts-datasources/format-bold-url.png)|
|Bağlantı için ITalics URL|`[*text*](https://www.my.com)`|`How do I create a bot with [*QnA Maker*](https://www.qnamaker.ai)?`|![italik URL için biçim](./media/qnamaker-concepts-datasources/format-url-italics.png)|
|Kaçış işaretleme sembolleri|`\*text\*`|`How do I create a bot with \*QnA Maker\*?`|![italik URL için biçim](./media/qnamaker-concepts-datasources/format-escape-markdown-symbols.png)|
|Sıralı liste|`\n 1. item1 \n 1. item2`|`This is an ordered list: \n 1. List item 1 \n 1. List item 2`<br>Önceki örnekte, işaretlemede yerleşik otomatik numaralandırma kullanır.<br>`This is an ordered list: \n 1. List item 1 \n 2. List item 2`<br>Önceki örnekaçık numaralandırma kullanır.|![sipariş edilen liste için biçim](./media/qnamaker-concepts-datasources/format-ordered-list.png)|
|Sırasız liste|`\n * item1 \n * item2`<br>or<br>`\n - item1 \n - item2`|`This is an unordered list: \n * List item 1 \n * List item 2`|![sıralanmamış liste için biçim](./media/qnamaker-concepts-datasources/format-unordered-list.png)|
|İç içe listeler|`\n * Parent1 \n\t * Child1 \n\t * Child2 \n * Parent2`<br><br>`\n * Parent1 \n\t 1. Child1 \n\t * Child2 \n 1. Parent2`<br><br>Sıralı ve sıralanmamış listeleri bir araya getirebilirsiniz. Sekme, `\t`alt öğenin girintisi düzeyini gösterir.|`This is an unordered list: \n * List item 1 \n\t * Child1 \n\t * Child2 \n * List item 2`<br><br>`This is an ordered nested list: \n 1. Parent1 \n\t 1. Child1 \n\t 1. Child2 \n 1. Parent2`|![iç içe sıralanmamış liste için biçim](./media/qnamaker-concepts-datasources/format-nested-unordered-list.png)<br>![iç içe sıralı liste için biçim](./media/qnamaker-concepts-datasources/format-nested-ordered-list.png)|

*QnA Maker görüntüyü hiçbir şekilde işlemez. Görüntüyü işlemek istemci uygulamasının görevidir.

Bilgi bankası API'lerini kullanarak içerik eklemek/değiştirmek istiyorsanız ve içerik/dosya html etiketleri içeriyorsa, etiketlerin açılıp kapanmasının kodlanmış biçimde dönüştürülmesini sağlayarak DOSYANıZDAKI HTML'yi koruyabilirsiniz.

| HTML'yi Koru  | API isteğinde gösterim  | KB'de Temsil |
|-----------|---------|-------------------------|
| Evet | \&lt;br\&gt; | &lt;Br&gt; |
| Evet | \&lt;h3\&gt;başlık\&lt;/h3\&gt; | &lt;h3&gt;üstbilgi&lt;/h3&gt; |

Ayrıca, CR LF(\r\n) KB'de \n'ye dönüştürülür. LF(\n) olduğu gibi tutulur. \t veya \n gibi herhangi bir kaçış dizisinden kaçmak istiyorsanız, örneğin\\\\\\\\ters eğik\\\\çizgi kullanabilirsiniz: ' r n' ve ' t'

## <a name="next-steps"></a>Sonraki adımlar

Toplu iş [testi dosya biçimlerini](reference-tsv-format-batch-testing.md)gözden geçirin.
