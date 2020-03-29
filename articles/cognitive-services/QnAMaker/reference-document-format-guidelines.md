---
title: Belge biçim yönergelerini içe aktarma - QnA Maker
description: QnA kümelerini almak ve oluşturmak için URL türlerinin nasıl kullanıldığını anlayın.
ms.topic: reference
ms.date: 01/02/2020
ms.openlocfilehash: 6a954f2fd607b70c6db256ab6dcc1dbcd7a5a473
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77651844"
---
# <a name="format-guidelines-for-imported-documents-and-urls"></a>İthal edilen belgeler ve URL'ler için biçimlendirme yönergeleri

İçeriğiniz için en iyi sonuçları almak için bu biçimlendirme yönergelerini gözden geçirin.

## <a name="formatting-considerations"></a>Konuları biçimlendirme

Bir dosya veya URL aldıktan sonra, QnA Maker içeriğinizi [işaretleme biçiminde](https://en.wikipedia.org/wiki/Markdown)dönüştürür ve saklar. Dönüştürme işlemi metne yeni satırlar `\n\n`ekler, örneğin. İşaretle biçimi hakkında bilgi sahibi olmak, dönüştürülen içeriği anlamanıza ve bilgi tabanı içeriğinizi yönetmenize yardımcı olur.

İçeriğinizi doğrudan bilgi tabanınıza ekler veya değiştirirseniz, zengin metin içeriği oluşturmak veya yanıtta bulunan işaretleme biçimi içeriğini değiştirmek için **işaretleme biçimlendirmesini** kullanın. QnA Maker, içeriğinize zengin metin özellikleri getirmek için işaretleme biçiminin çoğunu destekler. Ancak, sohbet botu gibi istemci uygulaması aynı işaretleme biçimleri kümesini desteklemeyebilir. İstemci uygulamasının yanıt ları görüntülemesini sınamak önemlidir.

## <a name="basic-document-formatting"></a>Temel belge biçimlendirme

QnA Maker gibi görsel ipuçlarıdayalı dosyada bölümleri ve alt bölümleri ve ilişkileri tanımlar:

* yazı tipi boyutu
* yazı tipi stili
* Numaralandırma
* Renkler

|Belge örnekleri|
|--|
||



## <a name="product-manuals"></a>Ürün kılavuzları

Kılavuz genellikle bir ürüne eşlik eden kılavuz malzemesidir. Kullanıcının ürünü ayarlamasına, kullanmasına, bakımına ve sorun gidermesine yardımcı olur. QnA Maker bir kılavuzu işlediğinde, başlıkları ve alt başlıkları soru olarak ayıklar ve sonraki içeriği yanıt olarak ayıklar. [Burada](https://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf)bir örnek bakın.

Aşağıda, dizin sayfası ve hiyerarşik içeriğe sahip bir kılavuz örneği ve

 ![Bilgi tabanı için Ürün El Kitabı örneği](./media/qnamaker-concepts-datasources/product-manual.png)

> [!NOTE]
> Çıkarma işlemi, içindekiler tablosu na ve/veya dizin sayfasına ve hiyerarşik başlıklara sahip net bir yapıya sahip kılavuzlarda en iyi şekilde çalışır.

## <a name="brochures-guidelines-papers-and-other-files"></a>Broşürler, kılavuzlar, kağıtlar ve diğer dosyalar

Diğer birçok belge türü, net bir yapı ve düzene sahip olmaları koşuluyla, QA çiftleri oluşturmak için de işlenebilir. Bunlar şunlardır: Broşürler, kılavuzlar, raporlar, teknik incelemeler, bilimsel makaleler, politikalar, kitaplar, vb. [Burada](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Manage%20Azure%20Blob%20Storage.docx)bir örnek bakın.

Aşağıda, dizin içermeyen yarı yapılandırılmış bir doküman örneği verilmiştir:

 ![Azure Blob depolama yarı yapılandırılmış Doc](./media/qnamaker-concepts-datasources/semi-structured-doc.png)

## <a name="structured-qna-document"></a>Yapılandırılmış QnA Belgesi

DOC dosyalarında yapılandırılmış Soru-Cevaplar biçimi, satır başına alternatif Sorular ve Yanıtlar şeklindedir, satır başına bir soru ve aşağıdaki satırda cevap takip, aşağıda gösterildiği gibi:

```text
Question1

Answer1

Question2

Answer2
```

Aşağıda yapılandırılmış bir QnA word belgesi örneği verilmiştir:

 ![Bir bilgi tabanı için yapılandırılmış QnA belge örneği](./media/qnamaker-concepts-datasources/structured-qna-doc.png)

## <a name="structured-txt-tsv-and-xls-files"></a>Yapılandırılmış *TXT*, *TSV* ve *XLS* Dosyaları

Yapılandırılmış *.txt*, *.tsv* veya *.xls* dosyaları şeklindeki QnA'lar da bilgi tabanı oluşturmak veya güçlendirmek için QnA Maker'a yüklenebilir.  Bunlar düz metin olabilir veya RTF veya HTML içeriğine sahip olabilir.

| Soru  | Yanıt  | Meta veriler (1 anahtar: 1 değer) |
|-----------|---------|-------------------------|
| Soru1 | Cevap1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Soru2 | Cevap2 |      `Key:Value`           |

Kaynak dosyadaki ek sütunlar yoksayılır.

### <a name="example-of-structured-excel-file"></a>Yapılandırılmış Excel dosyası örneği

Aşağıda HTML içeriği ile yapılandırılmış bir QnA *.xls* dosyası, bir örnektir:

 ![Bir bilgi tabanı için yapılandırılmış QnA excel örneği](./media/qnamaker-concepts-datasources/structured-qna-xls.png)

### <a name="example-of-alternate-questions-for-single-answer-in-excel-file"></a>Excel dosyasında tek cevapiçin alternatif soru örneği

Aşağıda, tek bir yanıt için birkaç alternatif soru içeren yapılandırılmış bir QnA *.xls* dosyası örneği verilmiştir:

 ![Excel dosyasında tek cevapiçin alternatif soru örneği](./media/qnamaker-concepts-datasources/xls-alternate-question-example.png)

Dosya alındıktan sonra, soru-cevap çifti aşağıda gösterildiği gibi bilgi tabanındadır:

 ![Bilgi tabanına aktarılan tek yanıt için alternatif soruların ekran görüntüsü](./media/qnamaker-concepts-datasources/xls-alternate-question-example-after-import.png)

## <a name="structured-data-format-through-import"></a>Alma yoluyla yapılandırılmış veri biçimi

Bilgi tabanının içe aktarılışı, varolan bilgi tabanının içeriğinin yerini alır. Alma, veri kaynağı bilgilerini içeren yapılandırılmış bir .tsv dosyası gerektirir. Bu bilgiler, QnA Maker'ın soru-yanıt çiftlerini gruplandırmasına ve bunları belirli bir veri kaynağına bağlamasına yardımcı olur.

| Soru  | Yanıt  | Kaynak| Meta veriler (1 anahtar: 1 değer) |
|-----------|---------|----|---------------------|
| Soru1 | Cevap1 | Url1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Soru2 | Cevap2 | Editoryal|    `Key:Value`       |

<a href="#formatting-considerations"></a>

## <a name="multi-turn-document-formatting"></a>Çok döndürme belge biçimlendirmesi

* Hiyerarşiyi belirtmek için başlıkları ve alt başlıkları kullanın. Örneğin, hızlı olarak alınması gereken QnA'yı belirtmek için üst QnA ve h2'yi belirtmek için h1 yapabilirsiniz. Sonraki hiyerarşiyi belirtmek için küçük başlık boyutunu kullanın. Belgenizdeki yapıyı ima etmek için stil, renk veya başka bir mekanizma kullanmayın, QnA Maker çok döndürülme istemlerini ayıklamaz.
* Başlığın ilk karakteri büyük harfle yazılmalı.
* Bir başlığı soru işaretiyle bitirmeyin. `?`


|Belge örnekleri|
|--|
||