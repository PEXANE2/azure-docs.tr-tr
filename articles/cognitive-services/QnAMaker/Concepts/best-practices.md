---
title: En iyi uygulamalar - QnA Maker
description: Bilgi tabanınızı geliştirmek ve uygulamanızı/sohbet botunuzun son kullanıcılarına daha iyi sonuçlar sağlamak için bu en iyi uygulamaları kullanın.
ms.topic: conceptual
ms.date: 02/15/2020
ms.openlocfilehash: 9a6f7f7d6edc4544942476050a1ed3c2011af7fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80053125"
---
# <a name="best-practices-of-a-qna-maker-knowledge-base"></a>QnA Maker bilgi tabanının en iyi uygulamaları

[Bilgi bankası geliştirme yaşam döngüsü,](../Concepts/development-lifecycle-knowledge-base.md) KB'nizi başından sonuna kadar nasıl yönetecekleriniz konusunda size rehberlik eder. Bilgi tabanınızı geliştirmek ve istemci uygulamanıza veya sohbet botunuzun son kullanıcılarına daha iyi sonuçlar sağlamak için bu en iyi uygulamaları kullanın.

## <a name="extraction"></a>Ayıklama

QnA Maker hizmeti, Qna'ları içerikten ayıklayan algoritmaları sürekli olarak geliştiriyor ve desteklenen dosya ve HTML biçimleri listesini genişletiyor. Belge türünüze göre veri ayıklama [yönergelerini](../Concepts/content-types.md) izleyin.

Genel olarak, SSS sayfaları tek başına olmalı ve diğer bilgilerle birleştirilmemelidir. Ürün kılavuzlarının net başlıkları ve tercihen bir dizin sayfası olmalıdır.

### <a name="configuring-multi-turn"></a>Çok dönüşlü yapılandırma

Çok döndürme etkinleştirilmiş bilgi [tabanınızı oluşturun.](../how-to/multiturn-conversation.md#create-a-multi-turn-conversation-from-a-documents-structure) Bilgi tabanınız soru hiyerarşisini destekliyorsa veya destekliyorsa, bu hiyerarşi belgeden ayıklanabilir veya belge ayıklandıktan sonra oluşturulabilir.

## <a name="creating-good-questions-and-answers"></a>İyi sorular ve yanıtlar oluşturma

### <a name="good-questions"></a>İyi sorular

En iyi sorular basittir. Her soru için anahtar sözcük veya tümcecik düşünün sonra bu anahtar kelime veya tümcecik için basit bir soru oluşturun.

İhtiyacınız olduğu kadar çok alternatif soru ekleyin, ancak değişiklikleri basit tutun. Sorunun ana amacının bir parçası olmayan daha fazla sözcük veya tümce eklemek QnA Maker'ın eşleşme bulmasına yardımcı olmaz.


### <a name="add-relevant-alternative-questions"></a>İlgili alternatif sorular ekleme

Kullanıcınız, konuşma tarzı bir metin `How do I add a toner cartridge to my printer?` veya '. gibi `toner cartridge`bir anahtar kelime araması ile sorular girebilir. Bilgi tabanı doğru en iyi cevabı döndürmek için her iki soru stilleri olmalıdır. Müşterinin hangi anahtar kelimeleri girdiğinden emin değilseniz, sorguları çözümlemek için Application Insights verilerini kullanın.

### <a name="good-answers"></a>İyi cevaplar

En iyi cevaplar basit cevaplar ama çok basit değil. Gibi cevaplar kullanmayın `yes` `no`ve . Yanıtınız diğer kaynaklara bağlantı verecekse veya medya ve bağlantılarla ilgili zengin bir deneyim sağlayacaksa, yanıtları `strictFilters` ayırt etmek için meta veri [etiketlemeyi](../how-to/edit-knowledge-base.md#add-metadata) kullanın ve ardından doğru yanıt sürümünü almak için sorguyu özellikteki meta veri etiketleri ile [gönderin.](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration)

|Yanıt|Follup-up istemleri|
|--|--|
|Klavyedeki güç düğmesiyle Surface dizüstü bilgisayarı kapatın.|* Anahtar kombinasyonları uyku, kapatın ve yeniden başlatın.<br>* Nasıl sert önyükleme bir Yüzey dizüstü bilgisayar<br>* Nasıl bir Yüzey dizüstü bilgisayar için BIOS değiştirmek için<br>* Uyku arasındaki farklar, kapatın ve yeniden|
|Müşteri hizmetleri telefon, Skype ve kısa mesaj yoluyla günün 24 saati kullanılabilir.|* Satış için iletişim bilgileri.<br> * Ofis ve mağaza yerleri ve saat bir in-person ziyaret için.<br> * Bir Yüzey dizüstü bilgisayar için aksesuarlar.|

## <a name="chit-chat"></a>Chit-Sohbet
Botunuzun daha düşük çabayla daha konuşkan ve ilgi çekici olmasını sağlamak için botunuza chit-chat ekleyin. KB'nizi oluştururken önceden tanımlanmış kişiliklerden kolayca chit-chat veri kümelerini ekleyebilir ve istediğiniz zaman değiştirebilirsiniz. [KB'nize nasıl sohbet ekleyeceğinizi](../How-To/chit-chat-knowledge-base.md)öğrenin.

Chit-chat [birçok dilde](../how-to/chit-chat-knowledge-base.md#language-support)desteklenir.

### <a name="choosing-a-personality"></a>Kişilik seçimi
Chit-chat birkaç önceden tanımlanmış kişilikleri için desteklenir:

|Kişilik |QnA Maker Dataset dosyası |
|---------|-----|
|Professional |[qna_chitchat_professional.tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_professional.tsv) |
|Dostu |[qna_chitchat_friendly.tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_friendly.tsv) |
|Esprili |[qna_chitchat_witty.tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_witty.tsv) |
|Bakım |[qna_chitchat_caring.tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_caring.tsv) |
|Hevesli |[qna_chitchat_enthusiastic.tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_enthusiastic.tsv) |

Yanıtlar resmi ve saygısız arasında değişir. Botunuzun tonunu en yakın şekilde seçmelisiniz. [Veri kümelerini](https://github.com/Microsoft/BotBuilder-PersonalityChat/tree/master/CSharp/Datasets)görüntüleyebilir ve botunuzun temeli olarak hizmet veren bir tane seçebilir ve ardından yanıtları özelleştirebilirsiniz.

### <a name="edit-bot-specific-questions"></a>Bota özgü soruları edin
Chit-chat veri kümesinin bir parçası olan ve genel yanıtlarla doldurulmuş bota özgü bazı sorular vardır. Bot bilgilerinizi en iyi şekilde yansıtacak şekilde bu yanıtları değiştirin.

Aşağıdaki chit-chat QnAs'ı daha spesifik hale getirmenizi öneririz:

* Kimsin?
* Ne yapabilirsin ki?
* Kaç yaşındasın?
* Seni kim yarattı?
* Hello

### <a name="adding-custom-chit-chat-with-a-metadata-tag"></a>Meta veri etiketiyle özel chit-chat ekleme

Kendi chit-chat QnA çiftlerinizi eklerseniz, bu yanıtların döndürülmesi için meta veri eklediğinizden emin olun. Meta veri adı/değer `editorial:chitchat`çifti.

## <a name="searching-for-answers"></a>Yanıtları arama

GenerateAnswer API, bir kullanıcının sorgusuna en iyi yanıtları aramak için hem soruları hem de yanıtı kullanır.

### <a name="searching-questions-only-when-answer-is-not-relevant"></a>Soruları yalnızca yanıt alakalı olmadığında arama

[`RankerType=QuestionOnly`](#choosing-ranker-type) Yanıtları aramak istemiyorsanız kullanın.

Buna bir örnek, bilgi tabanının yanıt olarak tam formu olan sorular olarak kısaltmaların kataloğu olmasıdır. Yanıtın değeri uygun yanıtı aramak için yardımcı olmayacaktır.

## <a name="rankingscoring"></a>Sıralama / Puanlama
QnA Maker'ın desteklediği sıralama özelliklerinden en iyi şekilde yararlandığınızdan emin olun. Bunu yapmak, belirli bir kullanıcı sorgusunun uygun bir yanıtla yanıtlanma olasılığını artırır.

### <a name="choosing-a-threshold"></a>Eşik seçme

Eşik olarak kullanılan varsayılan [güven puanı](confidence-score.md) 0'dır, ancak ihtiyaçlarınıza göre KB'nizin [eşiğini değiştirebilirsiniz.](confidence-score.md#set-threshold) Her KB farklı olduğundan, test etmeli ve KB için en uygun eşik seçin.

### <a name="choosing-ranker-type"></a>Ranker türünü seçme
Varsayılan olarak, QnA Maker sorular ve yanıtlar aracılığıyla arama. Yalnızca sorular arasında arama yapmak, bir yanıt oluşturmak `RankerType=QuestionOnly` istiyorsanız, GenerateAnswer isteğinin POST gövdesini kullanın.

### <a name="add-alternate-questions"></a>Alternatif sorular ekleme
[Alternatif sorular,](../How-To/edit-knowledge-base.md) kullanıcı sorgusuyla eşleşme olasılığını artırır. Aynı sorunun sorulabileceği birden çok yol olduğunda alternatif sorular yararlıdır. Bu, tümce yapısıve sözcük stilideğişiklikleri içerebilir.

|Özgün sorgu|Alternatif sorgular|Değiştir|
|--|--|--|
|Park yeri var mı?|Otoparkın var mı?|cümle yapısı|
 |Merhaba|Yo<br>Hey, orada!|kelime tarzı veya argo|

<a name="use-metadata-filters"></a>

### <a name="use-metadata-tags-to-filter-questions-and-answers"></a>Soruları ve yanıtları filtrelemek için meta veri etiketlerini kullanma

[Meta veriler,](../How-To/edit-knowledge-base.md) istemci uygulamasının tüm yanıtları almaması gerektiğini bilmesi yerine, meta veri etiketlerini temel alan bir kullanıcı sorgusunun sonuçlarını daraltma olanağı ekler. Sorgu aynı olsa bile, bilgi temel yanıtı meta veri etiketine göre farklılık görebilir. Örneğin, restoran şubesinin konumu farklıysa *"park yeri nerede"* farklı bir yanıt alabilir - yani meta veriler *Konum: Seattle* ve *Konum: Redmond'* dur.

### <a name="use-synonyms"></a>Eş anlamlı ları kullanma
İngilizce'de eşanlamlılar için bazı destek olsa da, farklı biçim alan anahtar kelimelere eş anlamlı lar eklemek için [Değişiklikler API'si](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/alterations/replace) aracılığıyla büyük/küçük harf duyarlı sözcük değişiklikleri kullanın. Eş anlamlılar QnA Maker hizmet düzeyinde eklenir ve hizmetteki tüm bilgi üsleri tarafından paylaşılır.

|Orijinal sözcük|Eş anlamlılar|
|--|--|
|Satın almak|satın alma<br>net bankacılık<br>net bankacılık|

### <a name="use-distinct-words-to-differentiate-questions"></a>Soruları ayırt etmek için farklı sözcükler kullanın
QnA Maker'ın bilgi tabanındaki bir soruyla kullanıcı sorgusuyla eşleşen sıralama algoritması, her soru farklı bir gereksinimi ele alıyorsa en iyi şekilde çalışır. Sorular arasında aynı sözcük kümesinin tekrarı, bu sözcüklerle belirli bir kullanıcı sorgusu için doğru yanıtın seçilme olasılığını azaltır.

Örneğin, aşağıdaki soruları içeren iki ayrı Qna'nız olabilir:

|QnAs|
|--|
|park *yeri* nerede|
|ATM *konumu* nerede|

Bu iki Qna çok benzer sözcüklerle ifade olduğundan, bu benzerlik * `<x>` "nerede konum"* gibi ifade birçok kullanıcı sorguları için çok benzer puanları neden olabilir. Bunun yerine, KB'nizde birçok soruda olabilecek "konum" gibi sözcüklerden kaçınarak *"otopark nerede"* ve *"ATM nerede"* gibi sorgularla açıkça ayırt etmeye çalışın.

## <a name="collaborate"></a>İşbirliği
QnA Maker, kullanıcıların bir bilgi tabanında [işbirliği](../How-to/collaborate-knowledge-base.md) yapmasına olanak tanır. Kullanıcıların bilgi tabanlarına erişmek için Azure QnA Maker kaynak grubuna erişmesi gerekir. Bazı kuruluşlar bilgi tabanı düzenleme ve bakım ını dış kaynaktan sağlamak ve Azure kaynaklarına erişimi korumak isteyebilir. Bu düzenleyici onaylayıcı modeli, farklı aboneliklerde iki özdeş [QnA Maker hizmeti](../How-to/set-up-qnamaker-service-azure.md) ayarlayarak ve düzenleme-sınama döngüsü için bir tane seçerek yapılır. Sınama tamamlandıktan sonra, bilgi bankası içeriği bir [alma-dışa aktarım](../Tutorials/migrate-knowledge-base.md) işlemiyle onaylayanın QnA Maker hizmetine aktarılır ve sonunda bilgi tabanını yayımlar ve bitiş noktasını günceller.



## <a name="active-learning"></a>Etkin öğrenme

[Aktif öğrenme,](../How-to/use-active-learning.md) kullanıcı tabanlı sorguların çok çeşitli kalitesi ve miktarına sahip olduğunda alternatif sorular önermenin en iyi işini yapar. İstemci uygulamalarının kullanıcı sorgularının sansür olmadan etkin öğrenme geri bildirim döngüsüne katılmasına izin vermek önemlidir. Sorular QnA Maker portalında önerildiği zaman, **[önerilere göre filtre](../How-To/improve-knowledge-base.md#accept-an-active-learning-suggestion-in-the-knowledge-base)** uygulayabilir, ardından bu önerileri gözden geçirebilir ve kabul edebilir veya reddedebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası düzenleme](../How-to/edit-knowledge-base.md)
