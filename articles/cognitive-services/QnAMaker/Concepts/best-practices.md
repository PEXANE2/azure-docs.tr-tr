---
title: En iyi uygulamalar - soru-cevap Oluşturucu
description: Bilgi bankanızı artırmak ve uygulama/sohbet Robotu ait son kullanıcılara daha iyi sonuçlar sağlamak için bu en iyi uygulamaları kullanın.
ms.topic: conceptual
ms.date: 02/15/2020
ms.openlocfilehash: fb935aeed7b492a3a0c213d6d7166bd5d80144c1
ms.sourcegitcommit: f255f869c1dc451fd71e0cab340af629a1b5fb6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2020
ms.locfileid: "77370107"
---
# <a name="best-practices-of-a-qna-maker-knowledge-base"></a>Soru-cevap Oluşturucu Bilgi Bankası en iyi yöntemleri

[Bilgi Bankası geliştirme yaşam döngüsü](../Concepts/development-lifecycle-knowledge-base.md) , KB 'nizi baştan sona nasıl yönetebileceğinizi size rehberlik eder. Bilgi bankanızı geliştirmek ve istemci uygulamanıza veya sohbet bot 'un son kullanıcılarına daha iyi sonuçlar sağlamak için bu en iyi yöntemleri kullanın.

## <a name="extraction"></a>Ayıklama

Soru-cevap Oluşturucu hizmetini sürekli olarak içerik ve desteklenen dosya ve HTML biçimleri listesini genişleterek Bankalarıyla ayıklamak algoritmaları gelişiyor. Belge türüne göre veri ayıklama [yönergelerini](../Concepts/content-types.md) izleyin.

Genel olarak, SSS sayfaları, tek başına ve diğer bilgilerle birleşik olması gerekir. Ürün kılavuzlarını, NET başlıklar ve tercihen dizin sayfası olması gerekir.

### <a name="configuring-multi-turn"></a>Çoklu açmayı yapılandırma

Çoklu açma ayıklama özelliği etkinken [Bilgi Bankası 'Nizi oluşturun](../how-to/multiturn-conversation.md#create-a-multi-turn-conversation-from-a-documents-structure) . Bilgi tabanınız soru hiyerarşisini destekliyorsa veya destekliyorsa, bu hiyerarşi belgeden ayıklanamaz veya belge ayıklandıktan sonra oluşturulmuş olabilir.

## <a name="creating-good-questions-and-answers"></a>İyi sorular ve yanıtlar oluşturma

### <a name="good-questions"></a>İyi sorular

En iyi sorular basittir. Her soru için anahtar sözcük veya tümceciği göz önünde bulundurun ve ardından bu anahtar sözcük veya tümceciği için basit bir soru oluşturun.

İhtiyaç duyduğunuz sayıda alternatif soru ekleyin, ancak değişiklikleri basit tutun. Sorunun ana hedefinin parçası olmayan daha fazla sözcük veya amaç eklemek Soru-Cevap Oluşturma bir eşleşme bulmaya yardımcı olmaz.


### <a name="add-relevant-alternative-questions"></a>İlgili alternatif soruları ekleyin

Kullanıcılarınız bir metin stili, `How do I add a toner cartridge to my printer?` veya `toner cartridge`gibi bir anahtar sözcük araması ile soru girebilir. Bilgi Bankası, en iyi yanıtı doğru bir şekilde döndürmek için her iki türde soruya sahip olmalıdır. Müşterinin hangi anahtar kelimelerle girileceği emin değilseniz, sorguları çözümlemek için Application Insights veri kullanın.

### <a name="good-answers"></a>İyi yanıtlar

En iyi yanıtlar basit yanıtlardan oluşur ancak çok basittir. `yes` ve `no`gibi yanıtları kullanmayın. Yanıtlarınızın diğer kaynaklara bağlanması veya medya ve bağlantılarla zengin bir deneyim sağlaması gerekiyorsa, yanıtlar arasında ayrım yapmak için [meta veri etiketlemesini](../how-to/edit-knowledge-base.md#add-metadata) kullanın ve ardından doğru yanıt sürümünü almak için `strictFilters` özelliğindeki meta veri etiketleriyle [sorguyu gönderebilirsiniz](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration) .

|Yanıt|Yukarı açılan istemler|
|--|--|
|Klavye üzerindeki güç düğmesiyle yüzey dizüstü bilgisayarınızı kapatın.|* Uyku, kapatma ve yeniden başlatma için anahtar birleşimleri.<br>* Bir yüzey dizüstü bilgisayarı nasıl sabit bir şekilde Önyükle<br>* Yüzey dizüstü bilgisayar için BIOS 'U değiştirme<br>* Uyku, kapatma ve yeniden başlatma arasındaki farklar|
|Müşteri Hizmetleri, günde 24 saat boyunca telefon, Skype ve kısa mesaj yoluyla kullanılabilir.|* Sales için iletişim bilgileri.<br> * Office ve mağaza konumları ve saatleri bir kişi için ziyaret edin.<br> * Bir yüzey dizüstü bilgisayar için aksesuarlar.|

## <a name="chit-chat"></a>Chit sohbet
Botunuzun daha damıtarak konuşma bağlamında kullanılabilen ve ilgi çekici, botunuzun yapmak için chit sohbet ekleme az çaba. KB 'nizi oluştururken önceden tanımlanmış kişiselleştirimler aracılığıyla kolayca CHIT-chat veri kümeleri ekleyebilir ve bunları dilediğiniz zaman değiştirebilirsiniz. [KB 'nize Chit sohbeti ekleme](../How-To/chit-chat-knowledge-base.md)hakkında bilgi edinin.

Chit-sohbet [birçok dilde](../how-to/chit-chat-knowledge-base.md#language-support)desteklenmektedir.

### <a name="choosing-a-personality"></a>Bir kişilik seçme
Chit-sohbet, önceden tanımlanmış birkaç kişisel için desteklenir:

|Kişilik |Soru-Cevap Oluşturma veri kümesi dosyası |
|---------|-----|
|Professional |[qna_chitchat_professional. tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_professional.tsv) |
|Uygun |[qna_chitchat_friendly. tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_friendly.tsv) |
|Wtty |[qna_chitchat_witty. tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_witty.tsv) |
|Giriş |[qna_chitchat_caring. tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_caring.tsv) |
|Heyecanlandırdı |[qna_chitchat_enthusiastic. tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_enthusiastic.tsv) |

Yanıtları aralıktan biçimsel resmi olmayan ve saygısız. En yakın botunuz için istediğiniz sesi birlikte hizalanır kişilik seçmeniz gerekir. [Veri kümelerini](https://github.com/Microsoft/BotBuilder-PersonalityChat/tree/master/CSharp/Datasets)görüntüleyebilir ve botunuzun temeli olarak işlev gören bir tane seçebilir ve ardından yanıtları özelleştirebilirsiniz.

### <a name="edit-bot-specific-questions"></a>Bot özgü soruları Düzenle
Sohbet chit veri kümesinin parçası olan ve genel yanıtlar oturum doldurulmuş bazı bot özgü soruları vardır. Bu yanıtlar, en iyi bot ayrıntılarınızı yansıtacak şekilde değiştirin.

Aşağıdaki sohbet chit Bankalarıyla daha belirgin hale öneririz:

* Kimsiniz?
* Ne yapabilirsiniz?
* Kaç yaşındasınız?
* Kim, oluşturduğunuz?
* Merhaba

### <a name="adding-custom-chit-chat-with-a-metadata-tag"></a>Meta veri etiketiyle özel chit-sohbet ekleme

Kendi CHIT-chat QnA çiftlerinden birini eklerseniz, Bu yanıtların döndürüldüğünden meta veri eklediğinizden emin olun. Meta veri adı/değer çifti `editorial:chitchat`.

## <a name="searching-for-answers"></a>Yanıtlar aranıyor

GenerateAnswer API 'SI, bir kullanıcının sorgusuna en iyi yanıtları aramak için hem soruları hem de yanıtı kullanır.

### <a name="searching-questions-only-when-answer-is-not-relevant"></a>Yalnızca yanıt ilgili olmadığında soruları arama

Yanıtları aramak istemiyorsanız [`RankerType=QuestionOnly`](#choosing-ranker-type) kullanın.

Bu, Bilgi Bankası 'nın, yanıt olarak tam biçimiyle soru olarak kısaltmalar olarak bir katalog olduğu bir örnektir. Yanıtın değeri, uygun yanıtı aramanıza yardımcı olmaz.

## <a name="rankingscoring"></a>Derecelendirme ve puanlı
Soru-cevap Oluşturucu desteklediği derecelendirme özellikleri en iyi kullanımı kuran emin olun. Bunun yapılması olasılığına artıracak belirli kullanıcı sorgusu ile uygun bir yanıt yanıtlandığı.

### <a name="choosing-a-threshold"></a>Bir eşiği seçme

Eşik olarak kullanılan varsayılan [Güvenirlik puanı](confidence-score.md) 0 ' dır, ancak GEREKSINIMLERINIZE göre KB 'nizin [eşiğini değiştirebilirsiniz](confidence-score.md#set-threshold) . Her KB farklı olduğundan, test edin ve en iyi eşiği için KB uygun seçin gerekir.

### <a name="choosing-ranker-type"></a>Ranker türü seçiliyor
Varsayılan olarak, Soru-Cevap Oluşturma sorular ve yanıtlar arasında arama yapar. Yalnızca sorulardan arama yapmak istiyorsanız, yanıt oluşturmak için GenerateAnswer isteğinin GÖNDERI gövdesinde `RankerType=QuestionOnly` kullanın.

### <a name="add-alternate-questions"></a>Diğer sorular ekleyin
[Alternatif sorular](../How-To/edit-knowledge-base.md) , bir kullanıcı sorgusuyla eşleşme olasılığını artırır. Diğer sorular, aynı soruyu istenebilir birden çok yol olduğunda yararlıdır. Word-style ve cümle yapısı içinde bu değişiklikleri içerebilir.

|Özgün sorgu|Diğer sorgular|Değişiklik|
|--|--|--|
|Kullanılabilir park olan?|Araba park var mı?|cümle yapısı|
 |Merhaba|Yo<br>Hey var!|Word stil veya argo kullanımlar|

<a name="use-metadata-filters"></a>

### <a name="use-metadata-tags-to-filter-questions-and-answers"></a>Soruları ve yanıtları filtrelemek için meta veri etiketlerini kullanma

[Meta veriler](../How-To/edit-knowledge-base.md) , bir istemci uygulamanın tüm yanıtları getirmemelidir, ancak meta veri etiketlerine göre bir Kullanıcı sorgusunun sonuçlarını daraltmak için bu özelliği ekler. Sorgu aynı olsa bile, Bilgi Bankası yanıt meta veri etiketine göre değişebilir. Örneğin, *"nerede park yeri"* , Restoran dalının konumu farklıysa farklı bir yanıta sahip olabilir. Bu, meta veriler *Konum: Seattle* ve *Konum: Redmond*.

### <a name="use-synonyms"></a>Eş anlamlıları kullanma
Ingilizce dilde eş anlamlı sözcükler için bazı destek olsa da, farklı biçimde olan anahtar sözcüklere eşanlamlı ekler eklemek için [alterler API 'si](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/alterations/replace) aracılığıyla büyük/küçük harf duyarsız sözcük değişikliklerini kullanın. Eş anlamlılar Soru-Cevap Oluşturma hizmet düzeyinde eklenir ve hizmetteki tüm bilgi tabanları tarafından paylaşılır.

|Özgün word|Eş anlamlılar|
|--|--|
|Satın alma|satın al<br>net-bankacılık<br>NET bankacılık|

### <a name="use-distinct-words-to-differentiate-questions"></a>Sorular ayırt etmek için farklı sözcükler kullanın
Bilgi bankasındaki bir soru ile bir kullanıcı sorgusuyla eşleşen derecelendirme algoritması, her bir soru farklı bir gereksinim ele alıyorsa en iyi şekilde geçerlidir. Soru-Cevap Oluşturma Yineleme sorular arasında ayarlanmış aynı kelimenin doğru yanıt sözcükleri ile verilen kullanıcı sorgusu için seçilen olasılığını azaltır.

Örneğin, aşağıdaki soruları ile iki ayrı Bankalarıyla olabilir:

|Bankalarıyla|
|--|
|Park *konumu* nerede|
|Burada ATM *konumudur*|

Bu iki QnAs çok benzer sözcüklerle phrased olduğundan, bu benzerlik çok sayıda kullanıcı sorgusunun *"where `<x>` konumu"* olduğu gibi çok benzer puanlara neden olabilir. Bunun yerine, KB 'unuzda birçok soruda olabilecek "konum" gibi kelimeleri önleyerek " *Park partisi olan* " ve *"ATM*olduğu" gibi sorguları açıkça ayırt etmeye çalışın.

## <a name="collaborate"></a>İşbirliği yapın
Soru-Cevap Oluşturma, kullanıcıların Bilgi Bankası 'nda [işbirliği](../How-to/collaborate-knowledge-base.md) yapmasına olanak sağlar. Kullanıcıların bilgi bankalarından erişmek için soru-cevap Oluşturucu Azure kaynak grubuna erişim gerekir. Bazı kuruluşlar Bilgi Bankası düzenleme ve Bakım dış ve Azure kaynaklarına erişim hala koruyabilmesini isteyebilirsiniz. Bu düzenleyici-onaylayan modeli, farklı aboneliklerde iki özdeş [soru-cevap oluşturma hizmeti](../How-to/set-up-qnamaker-service-azure.md) ayarlanarak ve düzenleme testi döngüsüne yönelik bir seçim yaparak yapılır. Sınama tamamlandıktan sonra Bilgi Bankası içerikleri, son olarak Bilgi Bankası 'nı yayımlayacak ve uç noktayı güncelleştiren, onaylayanın Soru-Cevap Oluşturma hizmetine bir [içeri aktarma](../Tutorials/migrate-knowledge-base.md) işlemi ile aktarılır.



## <a name="active-learning"></a>Etkin öğrenme

[Etkin öğrenme](../How-to/improve-knowledge-base.md) , çok sayıda kalite ve Kullanıcı tabanlı sorgu miktarına sahip olduğunda alternatif soruların önermesinin en iyi İşlidir. İstemci uygulamalarının Kullanıcı sorgularının, censorgemesiz etkin öğrenme geri bildirim döngüsüne katılmasını sağlamak önemlidir. Soru-Cevap Oluşturma portalında sorular Önerildikten sonra, **[önerilere göre filtreleyerek](../How-To/improve-knowledge-base.md#accept-an-active-learning-suggestion-in-the-knowledge-base)** bu önerileri inceleyip kabul edebilir veya reddedebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası düzenleme](../How-to/edit-knowledge-base.md)
