---
title: İçerik türleri-Soru-Cevap Oluşturma
description: İçerik türleri arasında PDF, DOC ve TXT gibi birçok standart yapılandırılmış belge bulunur.
services: cognitive-services
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: ac94f51fbe20c2efc277c084f9c704b5bcfa3a86
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76843444"
---
# <a name="content-types-of-documents-you-can-add-to-a-knowledge-base"></a>Bilgi tabanına ekleyebileceğiniz belgelerin içerik türleri
İçerik türleri arasında PDF, DOC ve TXT gibi birçok standart yapılandırılmış belge bulunur.

## <a name="file-and-url-data-types"></a>Dosya ve URL veri türleri

Aşağıdaki tabloda, soru-cevap Oluşturucu tarafından desteklenen içeriği ve dosya biçimlerini türlerini özetler.

|Kaynak Türü|İçerik Türü| Örnekler|
|--|--|--|
|URL|SSS<br> (Düz, bölümler ile veya konu başlığı ile)<br>Destek sayfaları <br> (Tek sayfalı nasıl yapılır makaleleri, sorun giderme makaleleri vb.)|[Düz SSS](https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs), <br>[Bağlantılarla Ilgili SSS](https://www.microsoft.com/en-us/software-download/faq),<br> [Konularda SSS giriş sayfası](https://www.microsoft.com/Licensing/servicecenter/Help/Faq.aspx)<br>[Destek makalesi](https://docs.microsoft.com/azure/cognitive-services/qnamaker/concepts/best-practices)|
|PDF / DOC|Sık<br> Ürün el kitabı,<br> Broşürler<br> T<br> El ilanı Ilkesi,<br> Destek Kılavuzu,<br> Yapılandırılmış QnA,<br> benzerlerini.|[Yapılandırılmış QnA. doc](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/structured.docx),<br> [Örnek ürün el ile. PDF](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf),<br> [Örnek semi-structured. doc](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/semi-structured.docx),<br> [Örnek Teknik İnceleme. PDF](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/white-paper.pdf),<br>[Örnek multi-turn. docx](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/multi-turn.docx)|
|\* Excel|Yapılandırılmış soru-cevap dosyası<br> (RTF, HTML desteği dahil)|[Soru-cevap FAQ.xls örneği](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/QnA%20Maker%20Sample%20FAQ.xlsx)|
|\* TXT/TSV|Yapılandırılmış soru-cevap dosyası|[Örnek chit-chat.tsv](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Scenario_Responses_Friendly.tsv)|

Veri kaynağınız için kimlik doğrulaması gerekiyorsa, bu içeriği Soru-Cevap Oluşturma almak için aşağıdaki yöntemleri göz önünde bulundurun:

* Dosyayı el ile indirin ve Soru-Cevap Oluşturma alın
* Kimliği doğrulanmış [SharePoint konumundan](../how-to/add-sharepoint-datasources.md) dosya Ekle

## <a name="url-content"></a>URL içeriği

Soru-Cevap Oluşturma **URL** aracılığıyla iki tür belge içeri aktarılabilir:

* SSS URL'leri
* Destek URL 'Leri

Her tür beklenen bir biçimi gösterir.

## <a name="file-based-content"></a>Dosya tabanlı içerik

[Soru-cevap oluşturma portalında](https://www.qnamaker.ai)ortak bir kaynaktan veya yerel dosya sisteminizden bir bilgi tabanına dosya ekleyebilirsiniz.

## <a name="content-format-guidelines"></a>İçerik biçimi yönergeleri

Soru-Cevap Oluşturma dosya ve URL içeriği ekler ve içeriği markaşağı kümelerine (`.md`) olarak depolanır. İçerik, içerik yapısal bir biçimde düzenlendikleri ve iyi tanımlanmış bölümlerde temsil edildiği bir bilgi tabanı için uygun olur. Bölümler daha fazla alt bölümleri veya alt konuları bozulabilir. Ayıklama, hiyerarşik başlıkları olan açık bir yapıya sahip içerikte en iyi şekilde kullanılır.

Soru-Cevap Oluşturma yazı tipi boyutu, yazı tipi stili, numaralandırma, renkler vb. görsel ipuçlarına göre içerikteki bölümleri ve alt bölümleri ve ilişkileri tanımlar. Yarı yapılandırılmış içerik, el kitabı, SSS, yönergeler, ilkeler, broşürler, el ilanları ve diğer birçok dosya türlerini içerir.

## <a name="faq-urls"></a>SSS URL'leri

Soru-cevap Oluşturucu SSS web sayfaları 3 farklı formlarda destekleyebilir: düz SSS sayfaları, SSS sayfaları bağlantılarla birlikte, SSS sayfaları konuları giriş sayfası.


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

Soru-Cevap Oluşturma yazı tipi boyutu, yazı tipi stili, numaralandırma, renkler gibi görsel ipuçlarına göre dosyadaki bölümleri ve alt bölümleri ve ilişkileri tanımlar. Yarı yapılandırılmış PDF veya DOC dosyaları el ile, SSS, yönergeler, Ilkeler, broşürler, el Ilanları ve diğer birçok dosya türü olabilir. Bu dosyaların bazı örnek türleri aşağıda verilmiştir.

### <a name="product-manuals"></a>Ürün kılavuzlarını

El ile genellikle bir ürünle birlikte verilen yönergeleri malzeme oluşur. Bunu ayarlamak, kullanma, korumanıza ve ürünle ilgili sorunları giderme kullanıcıya yardımcı olur. Soru-cevap Oluşturucu el ile işlediğinde, başlıklar ve başlıklar olarak sorular ve yanıtlar olarak sonraki içeriği ayıklar. Bir örnek görmek [burada](https://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf).

El ile bir dizin sayfası ve hiyerarşik içerik ilişkin bir örnek aşağıda verilmiştir

 ![Ürün el ile Bilgi Bankası Örneğin](../media/qnamaker-concepts-datasources/product-manual.png)

> [!NOTE]
> Ayıklama içeriğini ve/veya dizin sayfası ve açık bir yapı hiyerarşik başlıklara sahip bir tablosu kılavuzları en iyi şekilde çalışır.

### <a name="brochures-guidelines-papers-and-other-files"></a>Broşürler, kılavuzlar, incelemeler ve diğer dosyaları

Açık bir yapı ve düzeni olması kaydıyla, QA çiftleri oluşturmak için ayrıca diğer türlerde belgeleri işlenebilir. Bunlar arasında şunlar yer alır: Broşürler, yönergeler, raporlar, teknik incelemeler, bilimsel incelemeler, ilkeler, kitaplar vb. [Buraya](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Manage%20Azure%20Blob%20Storage.docx)bir örnek bakın.

Dizin olmadan, yarı yapılandırılmış bir belge örneği aşağıdadır:

 ![Azure Blob Depolama yarı yapılandırılmış belge](../media/qnamaker-concepts-datasources/semi-structured-doc.png)

### <a name="structured-qna-document"></a>Yapılandırılmış QnA belgesi

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

| Soru  | Yanıt  | Meta veriler (1 anahtar: 1 değer) |
|-----------|---------|-------------------------|
| Question1 | Answer1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Question2 | Answer2 |      `Key:Value`           |

Kaynak dosyadaki ek sütunlar göz ardı edilir.

### <a name="example-of-structured-excel-file"></a>Yapılandırılmış Excel dosyası örneği

Yapılandırılmış bir soru-cevap örneği aşağıdadır *.xls* dosyasıyla HTML içeriği:

 ![Yapılandırılmış bir soru-cevap, örnek bir Bilgi Bankası için excel](../media/qnamaker-concepts-datasources/structured-qna-xls.png)

### <a name="example-of-alternate-questions-for-single-answer-in-excel-file"></a>Excel dosyasında tek yanıt için alternatif soruların örneği

Aşağıda, tek bir yanıt için çeşitli alternatif sorulara sahip bir yapılandırılmış QnA *. xls* dosyası örneği verilmiştir:

 ![Excel dosyasında tek yanıt için alternatif soruların örneği](../media/qnamaker-concepts-datasources/xls-alternate-question-example.png)

Dosya alındıktan sonra, soru-cevap çifti aşağıda gösterildiği gibi Bilgi Bankası 'nda bulunur:

 ![Bilgi Bankası 'na aktarılan tek yanıt için alternatif soruların ekran görüntüsü](../media/qnamaker-concepts-datasources/xls-alternate-question-example-after-import.png)

## <a name="formatting-considerations"></a>Biçimlendirme konuları

Bir dosya veya URL 'YI içeri aktardıktan sonra Soru-Cevap Oluşturma, içeriğinizi [markı biçiminde](https://en.wikipedia.org/wiki/Markdown)dönüştürür ve depolar. Dönüştürme işlemi, metne `\n\n`gibi yeni satırlar ekler. Markaşağı biçimi hakkında bilgi, dönüştürülen içeriği anlamanıza ve Bilgi Bankası içeriğinizi yönetmenize yardımcı olur.

İçeriğinizi doğrudan bilgi tabanınız içinde ekler veya düzenlersiniz, zengin metin içeriği oluşturmak veya zaten yanıtdaki markaşağı biçim içeriğini değiştirmek için **markı biçimlendirme** kullanın. Soru-Cevap Oluşturma, içeriğinize zengin metin özellikleri getirmek için markaşağı biçiminin çoğunu destekler. Ancak, bir sohbet bot gibi istemci uygulaması aynı markı biçimleri kümesini desteklemiyor olabilir. İstemci uygulamanın yanıt görüntüsünü test etmek önemlidir.

[Soru-cevap oluşturma markaşağı başvuru belgelerinden](../reference-markdown-format.md)daha fazla bilgi edinin.

## <a name="testing-your-markdown"></a>Marka testinizi test etme

Markın 'ı doğrulamak için **[Commonmark](https://commonmark.org/help/tutorial/index.html)** öğreticisini kullanın. Öğreticide hızlı kopyalama/yapıştırma doğrulaması için bir **deneyin** özelliği bulunur.

## <a name="next-steps"></a>Sonraki adımlar

* [Soru ve cevap (QnA) kümelerinin](question-answer-set.md) nasıl tasarlanacağını ve yönetileceğini anlayın