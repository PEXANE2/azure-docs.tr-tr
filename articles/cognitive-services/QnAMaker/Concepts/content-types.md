---
title: İçerik türleri - QnA Maker
description: İçerik türleri PDF, DOC ve TXT gibi birçok standart yapılandırılmış belge içerir.
services: cognitive-services
ms.topic: conceptual
ms.date: 02/24/2020
ms.openlocfilehash: 7c78f9ea261fa636cce50b69524802d0900e9d7b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77650206"
---
# <a name="content-types-of-documents-you-can-add-to-a-knowledge-base"></a>Bilgi tabanına ekleyebileceğiniz belgelerin içerik türleri
İçerik türleri PDF, DOC ve TXT gibi birçok standart yapılandırılmış belge içerir.

## <a name="file-and-url-data-types"></a>Dosya ve URL veri türleri

Aşağıdaki tabloda QnA Maker tarafından desteklenen içerik ve dosya biçimleri türleri özetlenmektedir.

|Kaynak Türü|İçerik Türü| Örnekler|
|--|--|--|
|URL'si|SSS<br> (Düz, bölümler veya konular ana sayfası ile)<br>Destek sayfaları <br> (Tek sayfa nasıl yapılır makaleleri, sorun giderme makaleleri vb.)|[Düz SSS](https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs), <br>[Linkler ile SSS](https://www.microsoft.com/en-us/software-download/faq),<br> [Konular ana sayfası ile SSS](https://www.microsoft.com/Licensing/servicecenter/Help/Faq.aspx)<br>[Destek makalesi](https://docs.microsoft.com/azure/cognitive-services/qnamaker/concepts/best-practices)|
|PDF / DOKÜMAN|Sss<br> Ürün Kılavuzu,<br> Broşür<br> Kağıt<br> El İlanı Politikası,<br> Destek rehberi,<br> Yapılandırılmış QnA,<br> vb.|**Çok dönüşlü olmadan**<br>[Yapılandırılmış QnA.doc](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/structured.docx),<br> [Örnek Ürün Manual.pdf](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf),<br> [Örnek yarı yapılandırılmış.doc](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/semi-structured.docx),<br> [Örnek beyaz paper.pdf](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/white-paper.pdf),<br><br>**Çok dönüşlü**:<br>[Yüzey Pro (docx)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/multi-turn.docx)<br>[Contoso Faydaları (docx)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.docx)<br>[Contoso Faydaları (pdf)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.pdf)|
|*Excel|Yapılandırılmış QnA dosyası<br> (RTF, HTML desteği dahil)|[Örnek QnA SSS.xls](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/QnA%20Maker%20Sample%20FAQ.xlsx)|
|*TXT/TSV|Yapılandırılmış QnA dosyası|[Örnek chit-chat.tsv](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Scenario_Responses_Friendly.tsv)|

Veri kaynağınız için kimlik doğrulamasına ihtiyacınız varsa, bu içeriği QnA Maker'a almak için aşağıdaki yöntemleri göz önünde bulundurun:

* Dosyayı el ile indirin ve QnA Maker'a aktarın
* Kimlik doğrulaması [Sharepoint konumundan](../how-to/add-sharepoint-datasources.md) dosya ekleme

## <a name="url-content"></a>URL içeriği

QnA Maker'da **URL** üzerinden iki belge türü içe aktarılabilir:

* SSS URL'leri
* Destek URL'leri

Her tür beklenen bir biçimi gösterir.

## <a name="file-based-content"></a>Dosya tabanlı içerik

[QnA Maker portalında](https://www.qnamaker.ai)ortak bir kaynaktan veya yerel dosya sisteminizden bilgi tabanına dosya ekleyebilirsiniz.

## <a name="content-format-guidelines"></a>İçerik biçimi yönergeleri

Farklı dosyaların [biçim yönergeleri](../reference-document-format-guidelines.md) hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Bir soru ve yanıt [(QnA) kümesinde](question-answer-set.md)hangi bilgilerin depolananın.