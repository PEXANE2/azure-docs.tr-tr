---
title: Belge biçimini içeri aktarma yönergeleri-Soru-Cevap Oluşturma
description: URL türlerinin, QnA kümelerini içeri ve dışarı aktarmak için nasıl kullanılacağını anlayın.
ms.topic: reference
ms.date: 01/02/2020
ms.openlocfilehash: 6a954f2fd607b70c6db256ab6dcc1dbcd7a5a473
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77651844"
---
# <a name="format-guidelines-for-imported-documents-and-urls"></a>İçeri aktarılan belgeler ve URL 'Ler için biçimlendirme yönergeleri

İçeriğiniz için en iyi sonuçları elde etmek üzere bu biçimlendirme kılavuzlarını gözden geçirin.

## <a name="formatting-considerations"></a>Biçimlendirme konuları

Bir dosya veya URL 'YI içeri aktardıktan sonra Soru-Cevap Oluşturma, içeriğinizi [markı biçiminde](https://en.wikipedia.org/wiki/Markdown)dönüştürür ve depolar. Dönüştürme işlemi, metne `\n\n`gibi yeni satırlar ekler. Markaşağı biçimi hakkında bilgi, dönüştürülen içeriği anlamanıza ve Bilgi Bankası içeriğinizi yönetmenize yardımcı olur.

İçeriğinizi doğrudan bilgi tabanınız içinde ekler veya düzenlersiniz, zengin metin içeriği oluşturmak veya zaten yanıtdaki markaşağı biçim içeriğini değiştirmek için **markı biçimlendirme** kullanın. Soru-Cevap Oluşturma, içeriğinize zengin metin özellikleri getirmek için markaşağı biçiminin çoğunu destekler. Ancak, bir sohbet bot gibi istemci uygulaması aynı markı biçimleri kümesini desteklemiyor olabilir. İstemci uygulamanın yanıt görüntüsünü test etmek önemlidir.

## <a name="basic-document-formatting"></a>Temel belge biçimlendirmesi

Soru-Cevap Oluşturma, dosyadaki görsel ipuçlarına göre bölümleri ve alt bölümleri ve ilişkileri tanımlar:

* yazı tipi boyutu
* yazı tipi stili
* numaralandırmasının
* renkler

|Belge örnekleri|
|--|
||



## <a name="product-manuals"></a>Ürün kılavuzlarını

El ile genellikle bir ürünle birlikte verilen yönergeleri malzeme oluşur. Bunu ayarlamak, kullanma, korumanıza ve ürünle ilgili sorunları giderme kullanıcıya yardımcı olur. Soru-cevap Oluşturucu el ile işlediğinde, başlıklar ve başlıklar olarak sorular ve yanıtlar olarak sonraki içeriği ayıklar. [Buraya](https://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf)bir örnek bakın.

El ile bir dizin sayfası ve hiyerarşik içerik ilişkin bir örnek aşağıda verilmiştir

 ![Ürün el ile Bilgi Bankası Örneğin](./media/qnamaker-concepts-datasources/product-manual.png)

> [!NOTE]
> Ayıklama içeriğini ve/veya dizin sayfası ve açık bir yapı hiyerarşik başlıklara sahip bir tablosu kılavuzları en iyi şekilde çalışır.

## <a name="brochures-guidelines-papers-and-other-files"></a>Broşürler, kılavuzlar, incelemeler ve diğer dosyaları

Açık bir yapı ve düzeni olması kaydıyla, QA çiftleri oluşturmak için ayrıca diğer türlerde belgeleri işlenebilir. Bunlar arasında şunlar yer alır: Broşürler, yönergeler, raporlar, teknik incelemeler, bilimsel incelemeler, ilkeler, kitaplar vb. [Buraya](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Manage%20Azure%20Blob%20Storage.docx)bir örnek bakın.

Dizin olmadan, yarı yapılandırılmış bir belge örneği aşağıdadır:

 ![Azure Blob Depolama yarı yapılandırılmış belge](./media/qnamaker-concepts-datasources/semi-structured-doc.png)

## <a name="structured-qna-document"></a>Yapılandırılmış soru-cevap belge

Belge dosyaları içinde yapılandırılmış soru-yanıt biçimi değişen sorular biçiminde ve her satırda, her satırda bir soruya yanıt, yanıt aşağıdaki satırda, aşağıda gösterildiği gibi ardından:

```text
Question1

Answer1

Question2

Answer2
```

Yapılandırılmış bir soru-cevap word belgesinin bir örnek aşağıda verilmiştir:

 ![Bilgi Bankası için yapılandırılmış soru-cevap belge örneği](./media/qnamaker-concepts-datasources/structured-qna-doc.png)

## <a name="structured-txt-tsv-and-xls-files"></a>Yapılandırılmış *txt*, *TSV* ve *xls* dosyaları

Bir Bilgi Bankası oluşturmak veya genişletmek için yapılandırılmış *. txt*, *. tsv* veya *. xls* dosyaları biçimindeki qnas de soru-cevap oluşturma yüklenebilir.  Bu düz metin olabilir veya RTF veya HTML içeriği sahip olabilir.

| Soru  | Yanıt  | Meta veriler (1 anahtar: 1 değer) |
|-----------|---------|-------------------------|
| Question1 | Answer1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Question2 | Answer2 |      `Key:Value`           |

Kaynak dosyadaki ek sütunlar göz ardı edilir.

### <a name="example-of-structured-excel-file"></a>Yapılandırılmış Excel dosyası örneği

Aşağıda, bir yapılandırılmış QnA *. xls* dosyası olan HTML içeriğiyle ilgili bir örnek verilmiştir:

 ![Yapılandırılmış bir soru-cevap, örnek bir Bilgi Bankası için excel](./media/qnamaker-concepts-datasources/structured-qna-xls.png)

### <a name="example-of-alternate-questions-for-single-answer-in-excel-file"></a>Excel dosyasında tek yanıt için alternatif soruların örneği

Aşağıda, tek bir yanıt için çeşitli alternatif sorulara sahip bir yapılandırılmış QnA *. xls* dosyası örneği verilmiştir:

 ![Excel dosyasında tek yanıt için alternatif soruların örneği](./media/qnamaker-concepts-datasources/xls-alternate-question-example.png)

Dosya alındıktan sonra, soru-cevap çifti aşağıda gösterildiği gibi Bilgi Bankası 'nda bulunur:

 ![Bilgi Bankası 'na aktarılan tek yanıt için alternatif soruların ekran görüntüsü](./media/qnamaker-concepts-datasources/xls-alternate-question-example-after-import.png)

## <a name="structured-data-format-through-import"></a>İçeri aktarma ile yapılandırılmış veri biçimi

Bilgi Bankası içeri aktarma, var olan bir Bilgi Bankası içeriğini değiştirir. İçeri aktarma, veri kaynağı bilgilerini içeren bir yapılandırılmış .tsv dosyası gerektirir. Bu bilgiler, soru-cevap çiftlerini ve onları belirli bir veri kaynağına göre gruplamak Soru-Cevap Oluşturma yardımcı olur.

| Soru  | Yanıt  | Kaynak| Meta veriler (1 anahtar: 1 değer) |
|-----------|---------|----|---------------------|
| Question1 | Answer1 | Url1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Question2 | Answer2 | düzeltme sınıfı,|    `Key:Value`       |

<a href="#formatting-considerations"></a>

## <a name="multi-turn-document-formatting"></a>Belge biçimlendirmesini çoklu aç

* Hiyerarşiyi göstermek için başlıklar ve alt başlıklar kullanın. Örneğin, komut istemi olarak alınması gereken QnA 'yi göstermek için üst QnA ve H2 'yi belirtmek üzere H1 'yi belirtebilirsiniz. Sonraki hiyerarşiyi göstermek için küçük başlık boyutunu kullanın. Belgenizde yapıyı daha fazla yapmak için stil, renk veya başka bir mekanizma kullanmayın, Soru-Cevap Oluşturma çoklu açma istemlerini ayıklamaz.
* Başlığın ilk karakteri büyük harfle yazılmalıdır.
* Bir başlığı soru işaretiyle sonlandırmayın, `?`.


|Belge örnekleri|
|--|
||