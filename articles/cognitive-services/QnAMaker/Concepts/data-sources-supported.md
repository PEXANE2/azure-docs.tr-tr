---
title: Destekleniyor - veri kaynakları soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu SSS, ürün kılavuzlarını, kılavuzlar, destek belgeleri ve web sayfaları, PDF dosyaları ya da Word MS doc dosyalarını depolanan ilkeleri gibi yarı yapılandırılmış içeriği otomatik olarak soru-cevap çiftlerini ayıklar. İçeriği, yapılandırılmış soru-cevap içerik dosyalarından Bilgi Bankası'na da eklenebilir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 07/25/2019
ms.author: diberry
ms.openlocfilehash: 78c75e69be5521660937e34da0d7261dba1de385
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68955550"
---
# <a name="data-sources-for-qna-maker-content"></a>Veri kaynakları için soru-cevap Oluşturucu içeriği

Soru-cevap Oluşturucu SSS, ürün kılavuzlarını, kılavuzlar, destek belgeleri ve web sayfaları, PDF dosyaları ya da Word MS doc dosyalarını depolanan ilkeleri gibi yarı yapılandırılmış içeriği otomatik olarak soru-cevap çiftlerini ayıklar. İçeriği, yapılandırılmış soru-cevap içerik dosyalarından Bilgi Bankası'na da eklenebilir. 

Aşağıdaki tabloda, soru-cevap Oluşturucu tarafından desteklenen içeriği ve dosya biçimlerini türlerini özetler.

|Kaynak Türü|İçerik Türü| Örnekler|
|--|--|--|
|URL|SSS<br> (Düz, bölümler ile veya konu başlığı ile)<br>Destek sayfaları <br> (Tek sayfalı nasıl yapılır makaleleri, sorun giderme makaleleri vb.)|[Düz SSS](https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs), <br>[Bağlantılarla Ilgili SSS](https://www.microsoft.com/software-download/faq),<br> [Konularda SSS giriş sayfası](https://www.microsoft.com/Licensing/servicecenter/Help/Faq.aspx)<br>[Destek makalesi](https://docs.microsoft.com/azure/cognitive-services/qnamaker/concepts/best-practices)|
|PDF / DOC|Sık<br> Ürün el kitabı,<br> Broşürler<br> T<br> El ilanı Ilkesi,<br> Destek Kılavuzu,<br> Yapılandırılmış QnA,<br> benzerlerini.|[Yapılandırılmış QnA. doc](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/semi-structured.docx),<br> [Örnek ürün el ile. PDF](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf),<br> [Örnek semi-structured. doc](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/semi-structured.docx),<br> [Örnek Teknik İnceleme. PDF](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/white-paper.pdf)|
|Excel|Yapılandırılmış soru-cevap dosyası<br> (RTF, HTML desteği dahil)|[Soru-cevap FAQ.xls örneği](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/QnA%20Maker%20Sample%20FAQ.xlsx)|
|TXT/TSV|Yapılandırılmış soru-cevap dosyası|[Örnek chit-chat.tsv](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Scenario_Responses_Friendly.tsv)|

## <a name="data-source-locations"></a>Veri kaynağı konumları

Çoğu veri kaynağı konumu, kimlik doğrulaması gerektirmeyen genel URL 'Leri veya dosyaları sağlamalıdır. 

[SharePoint veri kaynağı konumlarına](../How-to/add-sharepoint-datasources.md) kimliği doğrulanmış dosyalar sağlama izni verilir. SharePoint kaynakları, Web sayfaları değil, dosyalar olmalıdır. 

Kimliği doğrulanmış bir dosyanız veya URL 'niz varsa, dosyayı kimliği doğrulanmış siteden yerel bilgisayarınıza indirmek için alternatif bir seçenek, ardından dosyayı yerel bilgisayarınızdan Bilgi Bankası 'na eklemektir. 

## <a name="faq-urls"></a>SSS URL'leri

Soru-Cevap Oluşturma, 3 farklı formda SSS Web sayfalarını destekleyebilir: Düz SSS sayfaları, bağlantılarla ilgili SSS sayfaları, konular giriş sayfası ile SSS sayfaları.

### <a name="plain-faq-pages"></a>Düz SSS sayfaları

Bu SSS sayfası, yanıtları aynı sayfada sorular hemen izleyin, en yaygın türüdür. 

Düz bir SSS sayfasının bir örneği aşağıda verilmiştir:

![Bilgi Bankası için düz SSS sayfası örneği](../media/qnamaker-concepts-datasources/plain-faq.png) 

 
### <a name="faq-pages-with-links"></a>Bağlantılar hakkında SSS sayfaları 

Bu tür bir SSS sayfasını, sorular birlikte toplanır ve aynı sayfa farklı bölümlerde veya farklı sayfalardaki olan yanıtları bağlanır.

SSS sayfası aynı sayfada bölümlerde bağlantılarla birlikte bir örnek aşağıda verilmiştir:

 ![Bilgi Bankası için bölüm bağlantı SSS sayfası örneği](../media/qnamaker-concepts-datasources/sectionlink-faq.png) 


### <a name="faq-pages-with-a-topics-homepage"></a>SSS sayfaları konuları giriş sayfası

Bu tür bir SSS, her konunun farklı sayfasında, ilgili Bankalarıyla bağlantı olduğu konuları ile bir giriş sayfası vardır. Burada, soru-cevap Oluşturucu ilgili sorular ve yanıtlar ayıklamak için tüm bağlı sayfalarda gezinir.

Aşağıdaki konular giriş sayfası bağlantıları için farklı sayfalara SSS bölümlerinde sahip olduğu bir SSS sayfasında örneğidir. 

 ![Bilgi Bankası için ayrıntılı bağlantı SSS sayfası örneği](../media/qnamaker-concepts-datasources/topics-faq.png) 


### <a name="support-urls"></a>Destek URL 'Leri

Soru-Cevap Oluşturma, belirli bir görevin nasıl gerçekleştirileceğini, belirli bir sorunun nasıl tanılanacağı ve çözümleneceğini ve belirli bir işlem için en iyi uygulamaların hangileri olduğunu betimleyen web makaleleri gibi yarı yapılandırılmış destek Web sayfalarını işleyebilir. Ayıklama, hiyerarşik başlıkları olan açık bir yapıya sahip içerikte en iyi şekilde kullanılır.

> [!NOTE]
> Destek makaleleri için ayıklama yeni bir özelliktir ve erken aşamalarındayken. İyi yapılandırılmış olan ve karmaşık üstbilgiler/altbilgiler içermeyen basit sayfalar için en iyi şekilde kullanılır.

![Soru-Cevap Oluşturma, açık bir yapının hiyerarşik başlıklarla sunulduğu yarı yapılandırılmış Web sayfalarından ayıklanmasını destekler](../media/qnamaker-concepts-datasources/support-web-pages-with-heirarchical-structure.png)


## <a name="pdf-doc-files"></a>PDF / DOC dosyaları

Soru-cevap Oluşturucu, yarı yapılandırılmış bir PDF veya belge dosyanın içeriğini işlemek ve Bankalarıyla dönüştürün. İyi ayıklanabileceği iyi bir dosya olduğu içerik bazı yapılandırılmış biçimde düzenlenmiştir ve iyi tanımlanmış bölümlerinde gösterilen olan biridir. Aşağıdaki bölümlerde daha fazla alt bölümleri veya alt konuları bölünebilir. Ayıklama, en iyi açık bir yapı hiyerarşik başlıkları olan belgeler üzerinde çalışır.

Soru-cevap Oluşturucu, numaralandırma, renkler vb. bölüm ve alt bölümleri ve yazı tipi boyutu, yazı tipi stili gibi görsel ipuçları dayalı dosya ilişkilerini tanımlar. Yarı yapılandırılmış PDF veya belge dosyaları kılavuzları, SSS, yönergeleri, ilkeleri, broşürler, el ilanları ve birçok diğer dosya türlerini olabilir. Bu dosyaların bazı örnek türleri aşağıda verilmiştir.

### <a name="product-manuals"></a>Ürün kılavuzlarını

El ile genellikle bir ürünle birlikte verilen yönergeleri malzeme oluşur. Bunu ayarlamak, kullanma, korumanıza ve ürünle ilgili sorunları giderme kullanıcıya yardımcı olur. Soru-cevap Oluşturucu el ile işlediğinde, başlıklar ve başlıklar olarak sorular ve yanıtlar olarak sonraki içeriği ayıklar. Bir örnek görmek [burada](https://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf).

El ile bir dizin sayfası ve hiyerarşik içerik ilişkin bir örnek aşağıda verilmiştir

 ![Ürün el ile Bilgi Bankası Örneğin](../media/qnamaker-concepts-datasources/product-manual.png) 

> [!NOTE]
> Ayıklama içeriğini ve/veya dizin sayfası ve açık bir yapı hiyerarşik başlıklara sahip bir tablosu kılavuzları en iyi şekilde çalışır.

### <a name="brochures-guidelines-papers-and-other-files"></a>Broşürler, kılavuzlar, incelemeler ve diğer dosyaları

Açık bir yapı ve düzeni olması kaydıyla, QA çiftleri oluşturmak için ayrıca diğer türlerde belgeleri işlenebilir. Bunlar: Broşürler, yönergeler, raporlar, teknik incelemeler, bilimsel incelemeler, ilkeler, kitaplar vb. Bir örnek görmek [burada](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Manage%20Azure%20Blob%20Storage.docx).

Dizin olmadan, yarı yapılandırılmış bir belge örneği aşağıdadır:

 ![Azure Blob Depolama yarı yapılandırılmış belge](../media/qnamaker-concepts-datasources/semi-structured-doc.png) 

### <a name="structured-qna-document"></a>Yapılandırılmış soru-cevap belge

Belge dosyaları içinde yapılandırılmış soru-yanıt biçimi değişen sorular biçiminde ve her satırda, her satırda bir soruya yanıt, yanıt aşağıdaki satırda, aşağıda gösterildiği gibi ardından: 

```text
Question1

Answer1

Question2

Answer2
```

Yapılandırılmış bir soru-cevap word belgesinin bir örnek aşağıda verilmiştir:

 ![Bilgi Bankası için yapılandırılmış soru-cevap belge örneği](../media/qnamaker-concepts-datasources/structured-qna-doc.png) 

## <a name="structured-txt-tsv-and-xls-files"></a>Yapılandırılmış *TXT*, *TSV* ve *XLS* dosyaları

Biçiminde Bankalarıyla yapılandırılmış *.txt*, *.tsv* veya *.xls* dosyalar da karşıya yüklenebilir oluşturmak veya bir Bilgi Bankası büyütmek için soru-cevap Oluşturucu.  Bu düz metin olabilir veya RTF veya HTML içeriği sahip olabilir. 

| Soru  | Yanıt  | Meta veriler (1 anahtar: 1 değeri) |
|-----------|---------|-------------------------|
| Question1 | Answer1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Question2 | Answer2 |      `Key:Value`           |

Kaynak dosyadaki ek sütunlar göz ardı edilir.

Yapılandırılmış bir soru-cevap örneği aşağıdadır *.xls* dosyasıyla HTML içeriği:

 ![Yapılandırılmış bir soru-cevap, örnek bir Bilgi Bankası için excel](../media/qnamaker-concepts-datasources/structured-qna-xls.png)

## <a name="structured-data-format-through-import"></a>İçeri aktarma ile yapılandırılmış veri biçimi

Bilgi Bankası içeri aktarma, var olan bir Bilgi Bankası içeriğini değiştirir. İçeri aktarma, veri kaynağı bilgilerini içeren bir yapılandırılmış .tsv dosyası gerektirir. Bu bilgiler, soru-cevap çiftlerini ve onları belirli bir veri kaynağına göre gruplamak Soru-Cevap Oluşturma yardımcı olur.

| Soru  | Yanıt  | Source| Meta veriler (1 anahtar: 1 değeri) |          
|-----------|---------|----|---------------------|
| Question1 | Answer1 | Url1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Question2 | Answer2 | Düzenleme|    `Key:Value`       |

## <a name="editorially-add-to-knowledge-base"></a>Bilgi Bankası'na bilgi bankanızı düzenleyerek Ekle

Bilgi Bankası doldurmak için önceden var olan içerik yoksa, soru-cevap Oluşturucu bilgi temel Bankalarıyla bilgi bankanızı düzenleyerek ekleyebilirsiniz. Bilgi bankanızı güncelleştirmeyi öğrenebilirsiniz [burada](../How-To/edit-knowledge-base.md).

<a href="#formatting-considerations"></a>

## <a name="formatting-considerations"></a>Biçimlendirme konuları

Bir dosya veya URL 'YI içeri aktardıktan sonra Soru-Cevap Oluşturma, içeriğinizi [markı biçiminde](https://en.wikipedia.org/wiki/Markdown)dönüştürür ve depolar. Dönüştürme işlemi, metinde `\n\n`gibi yeni satırlar ekler. Markaşağı biçimi hakkında bilgi, dönüştürülen içeriği anlamanıza ve Bilgi Bankası içeriğinizi yönetmenize yardımcı olur. 

İçeriğinizi doğrudan bilgi tabanınız içinde ekler veya düzenlersiniz, zengin metin içeriği oluşturmak veya zaten yanıtdaki markaşağı biçim içeriğini değiştirmek için **markı biçimlendirme** kullanın. Soru-Cevap Oluşturma, içeriğinize zengin metin özellikleri getirmek için markaşağı biçiminin çoğunu destekler. Ancak, bir sohbet bot gibi istemci uygulaması aynı markı biçimleri kümesini desteklemiyor olabilir. İstemci uygulamanın yanıt görüntüsünü test etmek önemlidir. 

Aşağıda Soru-Cevap Oluşturma kullanabileceğiniz markın biçimlerinin listesi verilmiştir: 

|Amaç|Biçimi|Örnek markaşağı|İşleme<br>Sohbet bot 'ta gösterildiği gibi|
|--|--|--|--|
2 cümle arasında yeni bir satır.|`\n\n`|`How can I create a bot with \n\n QnA Maker?`|![iki cümle arasındaki yeni çizgiyi Biçimlendir](../media/qnamaker-concepts-datasources/format-newline.png)|
|H1 ile H6 arasındaki üstbilgiler, bu sayının `#` hangi üstbilgiyi gösterir. 1 `#` , H1 'dir.|`\n# text \n## text \n### text \n####text \n#####text` |`## Creating a bot \n ...text.... \n### Important news\n ...text... \n### Related Information\n ....text...`<br><br>`\n# my h1 \n## my h2\n### my h3 \n#### my h4 \n##### my h5`|![markaşağı başlıkları ile biçimlendirme](../media/qnamaker-concepts-datasources/format-headers.png)<br>![markaşağı üstbilgileri ile Biçimlendir H1 ile H5](../media/qnamaker-concepts-datasources/format-h1-h5.png)|
|İtalik |`*text*`|`How do I create a bot with *QnA Maker*?`|![italik ile biçimlendirme](../media/qnamaker-concepts-datasources/format-italics.png)|
|Dize (koyu)|`**text**`|`How do I create a bot with **QnA Maker**?`|![kalın için güçlü işaret ile biçimlendirme](../media/qnamaker-concepts-datasources/format-strong.png)|
|Bağlantı URL 'SI|`[text](https://www.my.com)`|`How do I create a bot with [QnA Maker](https://www.qnamaker.ai)?`|![URL için biçim (köprü)](../media/qnamaker-concepts-datasources/format-url.png)|
|\* Ortak görüntü URL 'SI|`![text](https://www.my.com/image.png)`|`How can I create a bot with ![QnAMaker](https://review.docs.microsoft.com/en-us/azure/cognitive-services/qnamaker/media/qnamaker-how-to-key-management/qnamaker-resource-list.png)`|![ortak görüntü URL 'SI için biçim ](../media/qnamaker-concepts-datasources/format-image-url.png)|
|Üstü çizili|`~~text~~`|`some ~~questoins~~ questions need to be asked`|![üstü çizili biçim](../media/qnamaker-concepts-datasources/format-strikethrough.png)|
|Kalın ve italik|`***text***`|`How can I create a ***QnA Maker*** bot?`|![kalın ve italik biçim](../media/qnamaker-concepts-datasources/format-bold-italics.png)|
|Bağlantı için kalın URL|`[**text**](https://www.my.com)`|`How do I create a bot with [**QnA Maker**](https://www.qnamaker.ai)?`|![kalın URL için biçim](../media/qnamaker-concepts-datasources/format-bold-url.png)|
|Bağlantı için italik URL|`[*text*](https://www.my.com)`|`How do I create a bot with [*QnA Maker*](https://www.qnamaker.ai)?`|![italik URL için biçim](../media/qnamaker-concepts-datasources/format-url-italics.png)|
|Kaçış markın sembolleri|`\*text\*`|`How do I create a bot with \*QnA Maker\*?`|![italik URL için biçim](../media/qnamaker-concepts-datasources/format-escape-markdown-symbols.png)|
|Sıralı liste|`\n 1. item1 \n 1. item2`|`This is an ordered list: \n 1. List item 1 \n 1. List item 2`<br>Yukarıdaki örnek, markaşağı içinde yerleşik olarak bulunan otomatik numaralandırmayı kullanır.<br>`This is an ordered list: \n 1. List item 1 \n 2. List item 2`<br>Yukarıdaki örnek, açık numaralandırma kullanır.|![sıralı liste için biçim](../media/qnamaker-concepts-datasources/format-ordered-list.png)|
|Sırasız liste|`\n * item1 \n * item2`<br>veya<br>`\n - item1 \n - item2`|`This is an ordered list: \n * List item 1 \n * List item 2`|![Sırasız liste için biçim](../media/qnamaker-concepts-datasources/format-unordered-list.png)|
|İç içe listeler|`\n * Parent1 \n\t * Child1 \n\t * Child2 \n * Parent2`<br><br>`\n * Parent1 \n\t 1. Child1 \n\t * Child2 \n 1. Parent2`<br><br>Sıralı ve sıralanmamış listeleri birlikte iç içe yerleştirebilirsiniz. Sekmesi `\t`, alt öğesinin girintileme düzeyini gösterir.|`This is an unordered list: \n * List item 1 \n\t * Child1 \n\t * Child2 \n * List item 2`<br><br>`This is an ordered nested list: \n 1. Parent1 \n\t 1. Child1 \n\t 1. Child2 \n 1. Parent2`|![iç içe sıralanmamış liste için biçim](../media/qnamaker-concepts-datasources/format-nested-unordered-list.png)<br>![iç içe geçmiş liste için biçim](../media/qnamaker-concepts-datasources/format-nested-ordered-list.png)|

\* Soru-Cevap Oluşturma görüntüyü hiçbir şekilde işlemez. Bu, görüntüyü işlemek için istemci uygulamanın rolüdür. 

## <a name="editing-your-knowledge-base-locally"></a>Bilgi bankaınızı yerel olarak Düzenle

Bilgi Bankası oluşturulduktan sonra, yerel dosyalar aracılığıyla dışarı aktarmak ve yeniden içeri aktarmak yerine [soru-cevap oluşturma portalındaki](https://qnamaker.ai)Bilgi Bankası metninde düzenleme yapmanız önerilir. Ancak, bir bilgi bankasını yerel olarak düzenlemeniz gerektiği zamanlar olabilir. 

Bilgi Bankası 'nı **Ayarlar** sayfasından dışarı aktarın ve Bilgi Bankası 'Nı Microsoft Excel ile düzenleyin. İçe aktarılmış TSV Dosyanızı düzenlemek için başka bir uygulama kullanmayı seçerseniz, uygulama tam olarak uyumlu olmadığından sözdizimi hataları verebilir. Microsoft Excel 'in TSV dosyaları genellikle biçimlendirme hataları sunmaz. 

Düzenlemelerinizi tamamladıktan sonra, **Ayarlar** sayfasından TSV dosyasını yeniden içeri aktarın. Bu işlem, geçerli bilgi bankasını içeri aktarılan Bilgi Bankası ile tamamen değiştirir. 

## <a name="testing-your-markdown"></a>Marka testinizi test etme

Markın 'ı doğrulamak için **[Commonmark](https://commonmark.org/help/tutorial/index.html)** öğreticisini kullanın. Öğreticide hızlı kopyalama/yapıştırma doğrulaması için bir **deneyin** özelliği bulunur. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir soru-cevap Oluşturucu hizmetini ayarlama](../How-To/set-up-qnamaker-service-azure.md)

## <a name="see-also"></a>Ayrıca bkz. 

[Soru-Cevap Oluşturma’ya genel bakış](../Overview/overview.md)