---
title: Visual Studio Code için Azure Stream Analytics araçlarında IntelliSense
description: Bu makalede, Visual Studio Code için Azure Akış Analizi araçlarında IntelliSense özelliklerinin nasıl kullanılacağı açıklanmaktadır.
ms.service: stream-analytics
author: su-jie
ms.author: sujie
ms.date: 4/11/2020
ms.topic: conceptual
ms.openlocfilehash: 25ece3174cd65df9c665888d913cf6b9029904b1
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81394403"
---
# <a name="intellisense-in-azure-stream-analytics-tools-for-visual-studio-code"></a>Visual Studio Code için Azure Stream Analytics araçlarında IntelliSense

IntelliSense, [VS Code için Azure Akış Analizi araçlarında](https://marketplace.visualstudio.com/items?itemName=ms-bigdatatools.vscode-asa&ssr=false#overview)Stream [Analytics Query Language](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference?toc=https%3A%2F%2Fdocs.microsoft.com%2Fazure%2Fstream-analytics%2Ftoc.json&bc=https%3A%2F%2Fdocs.microsoft.com%2Fazure%2Fbread%2Ftoc.json) için kullanılabilir. IntelliSense bir dizi özellik içeren bir kod tamamlama yardımıdır: Liste Üyeleri, Parametre Bilgileri, Hızlı Bilgi ve Tam Word. IntelliSense özellikleri bazen "kod tamamlama", "içerik yardımı" ve "kod ipucu" gibi diğer adlarla çağrılır.

![IntelliSense demo](./media/vs-code-intellisense/intellisense.gif)

## <a name="intellisense-features"></a>IntelliSense özellikleri

VS Code için Stream Analytics araçlarındaki IntelliSense özellikleri bir dil hizmeti tarafından desteklenmektedir. Bir dil hizmeti kaynak kodunuzu analiz eder ve dil semantiklerine dayalı akıllı kod tamamlamaları sağlar. Bir dil hizmeti olası tamamlamaları biliyorsa, siz yazarken IntelliSense önerileri açılır. Yazmaya devam ederseniz, değişkenler ve yöntemler gibi bir üye listesi yalnızca yazdığınız karakterleri içeren üyeleri içerecek şekilde filtrelenir. Tuşlara `Tab` veya `Enter` tuşlara bastığınızda, IntelliSense seçtiğiniz üyeyi ekler.

Nokta karakteri `.`gibi bir tetikleyici karakter yazarak intelliSense'i herhangi bir düzenleyici penceresinde tetikleyebilirsiniz.

![intellisense otomatik tamamlama](./media/vs-code-intellisense/auto-completion.gif)

> [!TIP]
> Öneriler widget CamelCase filtreleme destekler. Önerileri sınırlamak için bir yöntem adına büyük harf yazabilirsiniz. Örneğin, "cra" hızlı bir şekilde "createApplication" kadar getirecektir.

### <a name="types-of-completions"></a>Tamamlanma türleri

VS Code IntelliSense için Stream Analytics araçları, dil sunucusu önerileri, parçacıklar ve basit sözcük tabanlı metin tamamlamalar dahil olmak üzere farklı türde tamamlamalar sunar.

|       |         |       |
| ----- | ------- | ----- |
| Anahtar sözcükler | `keyword`
| İşlevler | `build-in function`, `user defined function`  |
| Veri Kümesi Adı| `input`, `output`, `intermediate result set`|
| Veri Seti Sütun Adı|`input`, `intermediate result set`|
|

#### <a name="name-completion"></a>Ad tamamlama

Anahtar kelimenin otomatik olarak tamamlanmasının yanı sıra, VS Code için Stream Analytics araçları, iş giriş ve çıktı adlarının listesini ve yapılandırıldıklarında veri kaynaklarınızdaki sütunların adlarını okur. Uzantı, birkaç tuş vuruşuyla deyimler girmek için yararlı olan ad tamamlama özellikleri sağlamak için bu bilgileri hatırlar:

Kodlama yaparken, iş giriş adları, çıktı adı ve sütun adlarında arama yapmak için düzenleyiciden ayrılmanız gerekmez. Bağlamınızı koruyabilir, ihtiyacınız olan bilgileri bulabilir, öğeleri doğrudan kodunuza ekleyebilir ve IntelliSense'in sizin için yazmanızı tamamlamasını sağlayabilirsiniz.

Yerel girişi veya canlı girişi yapılandırmanız ve ad tamamlamayı kullanabilmek için yapılandırma dosyasını kaydetmeniz gerektiğini unutmayın.

![isim tamamlama](./media/vs-code-intellisense/name-completion.gif)

### <a name="parameter-info"></a>Parametre Bilgisi

IntelliSense **Parametre Bilgisi** seçeneği, bir işlev tarafından gerekli olan parametrelerin sayısı, adları ve türleri hakkında bilgi sağlayan bir parametre listesi açar. Kalın daki parametre, bir işlev yazarken gereken bir sonraki parametreyi gösterir.

Parametre listesi iç içe yönelik işlevler için de görüntülenir. Başka bir işleve parametre olarak bir işlev yazarsanız, parametre listesi iç işlevin parametrelerini görüntüler. Daha sonra, iç işlev parametre listesi tamamlandığında, parametre listesi dış işlev parametrelerini görüntülemeye geri döner.

![parametre bilgisi](./media/vs-code-intellisense/parameter-info.gif)

### <a name="quick-info"></a>Hızlı Bilgi

Dil hizmeti tarafından sağlandığı üzere, kodunuzdaki her tanımlayıcı için **Hızlı Bilgi'yi** görebilirsiniz. Tanımlayıcılara bazı örnekler giriş, çıktı, ara sonuç kümesi veya işlevdir. Fare işaretçisini bir tanımlayıcının üzerine taşıdığınızda, bildirimi açılır pencerede görüntülenir. Yapılandırılan girişler için özellikler ve veri şemaları ve ara veri kümesi gösterilir.

![hızlı bilgi](./media/vs-code-intellisense/quick-info.gif)

## <a name="troubleshoot-intellisense"></a>Sorun Giderme IntelliSense

Bu sorun, veri sağlayan eksik giriş yapılandırması neden olur. [Yerel bir girişin](visual-studio-code-local-run.md#define-a-local-input) veya [canlı girişin](visual-studio-code-local-run-live-input.md#define-a-live-stream-input) doğru şekilde yapılandırıp yapılmamadığını kontrol edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Visual Studio Code'da Azure Akışı Analizi işi oluşturun](quick-create-vs-code.md)
* [Visual Studio Code kullanarak örnek verilerle test Akışı Analizi sorguları](visual-studio-code-local-run.md)
* [Test Stream Analytics, Visual Studio Code'u kullanarak canlı akış girişine karşı yerel olarak sorgular](visual-studio-code-local-run-live-input.md)
