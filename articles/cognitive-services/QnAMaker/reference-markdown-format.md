---
title: Markaşağı biçimi-Soru-Cevap Oluşturma
description: Soru-Cevap Oluşturma yanıt metninde kullanabileceğiniz markın biçimlerinin listesi aşağıda verilmiştir.
ms.topic: reference
ms.date: 01/09/2020
ms.openlocfilehash: 3fb7d73afdfd5ab7f1fb56a685b21538b97c8ea4
ms.sourcegitcommit: 57669c5ae1abdb6bac3b1e816ea822e3dbf5b3e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77045406"
---
# <a name="markdown-format-supported-in-qna-maker-answer-text"></a>Soru-Cevap Oluşturma yanıt metninde markın biçimi destekleniyor

Yanıt metnini markaşağı olarak depolar Soru-Cevap Oluşturma. Markı 'nin birçok türleri vardır. Yanıt metninin doğru bir şekilde döndürüldüğünden ve görüntülendiğinden emin olmak için bu başvuruyu kullanın.

Markın 'ı doğrulamak için **[Commonmark](https://commonmark.org/help/tutorial/index.html)** öğreticisini kullanın. Öğreticide hızlı kopyalama/yapıştırma doğrulaması için bir **deneyin** özelliği bulunur.

## <a name="supported-markdown-format"></a>Desteklenen markaşağı biçimi

Soru-Cevap Oluşturma yanıt metninde kullanabileceğiniz markın biçimlerinin listesi aşağıda verilmiştir.

|Amaç|Biçimlendir|Örnek markaşağı|İşleme<br>Sohbet bot 'ta gösterildiği gibi|
|--|--|--|--|
2 cümle arasında yeni bir satır.|`\n\n`|`How can I create a bot with \n\n QnA Maker?`|![iki cümle arasındaki yeni çizgiyi Biçimlendir](./media/qnamaker-concepts-datasources/format-newline.png)|
|H1 ile H6 arasındaki üstbilgiler, `#` sayısı hangi üstbilgiyi gösterir. 1 `#` H1 'dir.|`\n# text \n## text \n### text \n####text \n#####text` |`## Creating a bot \n ...text.... \n### Important news\n ...text... \n### Related Information\n ....text...`<br><br>`\n# my h1 \n## my h2\n### my h3 \n#### my h4 \n##### my h5`|![markaşağı başlıkları ile biçimlendirme](./media/qnamaker-concepts-datasources/format-headers.png)<br>![markaşağı üstbilgileri ile Biçimlendir H1 ile H5](./media/qnamaker-concepts-datasources/format-h1-h5.png)|
|İtalik |`*text*`|`How do I create a bot with *QnA Maker*?`|![italik ile biçimlendirme](./media/qnamaker-concepts-datasources/format-italics.png)|
|Güçlü (koyu)|`**text**`|`How do I create a bot with **QnA Maker**?`|![kalın için güçlü işaret ile biçimlendirme](./media/qnamaker-concepts-datasources/format-strong.png)|
|Bağlantı URL 'SI|`[text](https://www.my.com)`|`How do I create a bot with [QnA Maker](https://www.qnamaker.ai)?`|![URL için biçim (köprü)](./media/qnamaker-concepts-datasources/format-url.png)|
|\* Ortak görüntü URL 'SI|`![text](https://www.my.com/image.png)`|`How can I create a bot with ![QnAMaker](https://review.docs.microsoft.com/azure/cognitive-services/qnamaker/media/qnamaker-how-to-key-management/qnamaker-resource-list.png)`|![ortak görüntü URL 'SI için biçim ](./media/qnamaker-concepts-datasources/format-image-url.png)|
|Üstünü|`~~text~~`|`some ~~questoins~~ questions need to be asked`|![üstü çizili biçim](./media/qnamaker-concepts-datasources/format-strikethrough.png)|
|Kalın ve italik|`***text***`|`How can I create a ***QnA Maker*** bot?`|![kalın ve italik biçim](./media/qnamaker-concepts-datasources/format-bold-italics.png)|
|Bağlantı için kalın URL|`[**text**](https://www.my.com)`|`How do I create a bot with [**QnA Maker**](https://www.qnamaker.ai)?`|![kalın URL için biçim](./media/qnamaker-concepts-datasources/format-bold-url.png)|
|Bağlantı için italik URL|`[*text*](https://www.my.com)`|`How do I create a bot with [*QnA Maker*](https://www.qnamaker.ai)?`|![italik URL için biçim](./media/qnamaker-concepts-datasources/format-url-italics.png)|
|Kaçış markın sembolleri|`\*text\*`|`How do I create a bot with \*QnA Maker\*?`|![italik URL için biçim](./media/qnamaker-concepts-datasources/format-escape-markdown-symbols.png)|
|Sıralı liste|`\n 1. item1 \n 1. item2`|`This is an ordered list: \n 1. List item 1 \n 1. List item 2`<br>Yukarıdaki örnek, markaşağı içinde yerleşik olarak bulunan otomatik numaralandırmayı kullanır.<br>`This is an ordered list: \n 1. List item 1 \n 2. List item 2`<br>Yukarıdaki örnek, açık numaralandırma kullanır.|![sıralı liste için biçim](./media/qnamaker-concepts-datasources/format-ordered-list.png)|
|Sırasız liste|`\n * item1 \n * item2`<br>or<br>`\n - item1 \n - item2`|`This is an unordered list: \n * List item 1 \n * List item 2`|![Sırasız liste için biçim](./media/qnamaker-concepts-datasources/format-unordered-list.png)|
|İç içe listeler|`\n * Parent1 \n\t * Child1 \n\t * Child2 \n * Parent2`<br><br>`\n * Parent1 \n\t 1. Child1 \n\t * Child2 \n 1. Parent2`<br><br>Sıralı ve sıralanmamış listeleri birlikte iç içe yerleştirebilirsiniz. `\t`sekme, alt öğenin girintileme düzeyini gösterir.|`This is an unordered list: \n * List item 1 \n\t * Child1 \n\t * Child2 \n * List item 2`<br><br>`This is an ordered nested list: \n 1. Parent1 \n\t 1. Child1 \n\t 1. Child2 \n 1. Parent2`|![iç içe sıralanmamış liste için biçim](./media/qnamaker-concepts-datasources/format-nested-unordered-list.png)<br>![iç içe geçmiş liste için biçim](./media/qnamaker-concepts-datasources/format-nested-ordered-list.png)|

\* Soru-Cevap Oluşturma görüntüyü hiçbir şekilde işlemez. Bu, görüntüyü işlemek için istemci uygulamanın rolüdür.

Güncelleştirme/değiştirme Bilgi Bankası API 'Lerini kullanarak içerik eklemek istiyorsanız ve içerik/dosya HTML etiketleri içeriyorsa, etiketlerin açılış ve kapanışının kodlanmış biçimde dönüştürülmesini sağlamak için dosyanızdaki HTML 'yi koruyabilirsiniz.

| HTML 'yi koru  | API isteğindeki temsili  | KB cinsinden temsil |
|-----------|---------|-------------------------|
| Yes | \&lt; br\&gt; | &lt;br&gt; |
| Yes | \&lt; H3\&gt; üst bilgi\&lt;/H3\&gt; | &lt;H3&gt;üst bilgisi&lt;/H3&gt; |

Ayrıca, CR LF (\r\n), BB 'ye dönüştürülür. LF (\n) olduğu gibi tutulur. Bir kaçış sırasını \t veya \n gibi atlamak istiyorsanız ters eğik çizgi kullanabilirsiniz, örneğin: '\\\\r\\\\n ' ve '\\\\t '

## <a name="next-steps"></a>Sonraki adımlar

Toplu işlem testi [dosya biçimlerini](reference-tsv-format-batch-testing.md)gözden geçirin.
