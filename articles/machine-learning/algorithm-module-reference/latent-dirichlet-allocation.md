---
title: Görünmeyen Dirichlet Ayırması
titleSuffix: Azure Machine Learning
description: Aksi halde sınıflandırılmayan metinleri bir dizi kategoride gruplamak için Latent Dirichlet ayırma modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 03/11/2020
ms.openlocfilehash: 1384491489c175ffc338f80a99aa8d5050f835d5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80109231"
---
# <a name="latent-dirichlet-allocation"></a>Görünmeyen Dirichlet Ayırması

Bu makalede, Azure Machine Learning Tasarımcısı 'nda (Önizleme) yer aldığı ve sınıflandırılmayan metinleri bir dizi kategoride gruplamak için, tasarımcısında ' de **görünmeyen Dirichlet ayırma** modülünün nasıl kullanılacağı açıklanır. 

Latent Dirichlet ayırması (LDA), benzer metinleri bulmak için genellikle doğal dil işlemede (NLP) kullanılır. Diğer bir yaygın terim, *Konu modellemesi*.

Bu modül bir metin sütunu alır ve şu çıkışları üretir:

+ Kaynak metin, her kategori için bir puanla birlikte

+ Her kategori için Ayıklanan terimleri ve katsayıları içeren bir özellik matrisi

+ Giriş olarak kullanılan yeni metinlere kaydedip yeniden uygulayabilmeniz için bir dönüşüm

Bu modül, scikit-öğren kitaplığını kullanır. Scikit-öğrenme hakkında daha fazla bilgi için bkz. [GitHub deposu ve algoritmaların açıklaması.

### <a name="more-about-latent-dirichlet-allocation-lda"></a>Görünmeyen Dirichlet ayırma (LDA) hakkında daha fazla bilgi

Genellikle, LDA, sınıf başına sınıflandırma için bir yöntem değildir, ancak bir genel yaklaşım kullanır. Bunun anlamı, bilinen sınıf etiketleri sağlamanız ve sonra desenleri çıkarmayın.  Bunun yerine, algoritma, konu gruplarını tanımlamak için kullanılan bir dayalı modeli oluşturur. Mevcut eğitim çalışmalarını veya modele giriş olarak sağladığınız yeni servis taleplerini sınıflandırmak için dayalı modelini kullanabilirsiniz.

Metin ve kategoriler arasındaki ilişki hakkında kesin bir varsayımlar yapılmasını önlediği ve yalnızca matematik ve matematiksel olarak modelleme konularına olan sözcüklerin dağıtımını kullandığından, bir genel model tercih edilebilir.

+ Bu, bir PDF indirmesi olarak sunulan bu kağıda tartışılır: [latent Dirichlet ayırması: Blei, NG ve Ürdün](https://ai.stanford.edu/~ang/papers/nips01-lda.pdf)

+ Bu modüldeki uygulama, LDA için [scikit-öğren kitaplığını](https://github.com/scikit-learn/scikit-learn/blob/master/sklearn/decomposition/_lda.py) temel alır.

Daha fazla bilgi için [Teknik notlar](#technical-notes) bölümüne bakın.

## <a name="how-to-configure-latent-dirichlet-allocation"></a>Latent Dirichlet ayırmayı yapılandırma

Bu modül, ham veya önceden işlenmiş bir metin sütunu içeren bir veri kümesi gerektirir.

1. İşlem hattınızı **latent Dirichlet ayırma** modülünü ekleyin.

2. Modülün girişi olarak bir veya daha fazla metin sütunu içeren bir veri kümesi sağlayın.

3. **Hedef sütunlar**için, çözümlenecek metni içeren bir veya daha fazla sütun seçin.

    Birden çok sütun seçebilirsiniz, ancak dize veri türünde olması gerekir.

    Genel olarak, LDA metinden büyük bir özellik matrisi oluşturduğundan, genellikle tek bir metin sütununu çözümleyebilirsiniz.

4. **Model oluşturma konuları**için, giriş metninde kaç kategori veya konu türediğini belirten 1 ile 1000 arasında bir tamsayı yazın.

    Varsayılan olarak, 5 konu oluşturulur.

5. **N-gram**için, karma oluşturma sırasında oluşturulan n-gram maksimum uzunluğunu belirtin.

    Varsayılan değer 2 ' dir, yani hem bigram hem de unigram oluşturulur.

6. Çıktı değerlerini olasılıklara dönüştürmek için **Normalleştir** seçeneğini belirleyin. Bu nedenle, dönüştürülmüş değerleri tamsayılar olarak göstermek yerine, Output ve Feature veri kümesindeki değerler aşağıdaki gibi dönüştürülür:

    + Veri kümesindeki değerler bir olasılık olarak temsil edilir `P(topic|document)`.

    + Özellik konu matrisi içindeki değerler bir olasılık olarak temsil edilir `P(word|topic)`.

    > [!NOTE] 
    > Azure Machine Learning tasarımcı 'da (Önizleme), geliştirdiğimiz kitaplık 0,19 sürümünden Normalleştirilmemiş *doc_topic_distr* çıktıyı desteklemediği için, bu modülde, **normalize** parametresi yalnızca **özellik konu matrisi** çıktısına uygulanabilir, **dönüştürülmüş veri kümesi** çıkışı her zaman normalleştirilir.

7. Seçeneği belirleyin, **tüm seçenekleri göster**' i seçin ve ardından ek Gelişmiş parametreleri görüntülemek ve ayarlamak ISTIYORSANıZ bunu true olarak ayarlayın.

    Bu parametreler LDA 'nın scikit-öğrenme uygulamasına özgüdür. Scikit-öğrenme konusunda ve resmi [scikit-öğrenme belgesi](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.LatentDirichletAllocation.html)hakkında bazı iyi öğreticiler vardır.

    + **Ro parametresi**. Konu dağıtımları seyrekliği için önceki bir olasılık sağlayın. Sköğren 'in `topic_word_prior` parametresine karşılık gelir. Sözcüklerin dağılımının düz olması beklendiğinde 1 değerini kullanırsınız; Yani, tüm kelimeler eşit kabul edilir. Çoğu sözcüğün çok seyrek görüntülendiğini düşünüyorsanız, bunu çok daha düşük bir değere ayarlayabilirsiniz.

    + **Alfa parametresi**. Belge başına seyrekliği için bir önceki olasılık belirtin.  Sköğren 'in `doc_topic_prior` parametresine karşılık gelir.

    + **Tahmini belge sayısı**. İşlenecek belge sayısı (satır) için en iyi tahmininizi temsil eden bir sayı yazın. Bu, modülün yeterli büyüklükte bir karma tablosu ayırmasını sağlar.  Scikit `total_samples` -öğren içindeki parametreye karşılık gelir.

    + **Toplu Işin boyutu**. LDA modeline gönderilen her metin toplu işinde kaç satır ekleneceğini belirten bir sayı yazın. Scikit `batch_size` -öğren içindeki parametreye karşılık gelir.

    + **Öğrenme güncelleştirme zamanlaması 'nda kullanılan yinelemenin başlangıç değeri**. Çevrimiçi öğrenmedeki Erken yinelemeler için öğrenme oranını aşağı örneklerdeki başlangıç değerini belirtin. Scikit `learning_offset` -öğren içindeki parametreye karşılık gelir.

    + **Güncelleştirmeler sırasında yinelemeye uygulanan güç**. Çevrimiçi güncelleştirmeler sırasında öğrenme oranını denetlemek için yineleme sayısına uygulanan güç düzeyini belirtin. Scikit `learning_decay` -öğren içindeki parametreye karşılık gelir.

    + **Veri üzerinden geçen geçiş sayısı**. Algoritmanın verilerin üzerinde kaç kez dolacaktır için en fazla sayıyı belirtin. Scikit `max_iter` -öğren içindeki parametreye karşılık gelir.

8. Metin sınıflandırmadan önce, bir ilk geçişte n-gram listesini oluşturmak istiyorsanız, **Ngram sözlüğü** veya **lda öncesinde Ngram**sözlüğü oluştur seçeneğini belirleyin.

    Başlangıç sözlüğünü önceden oluşturursanız, daha sonra modeli gözden geçirirken sözlüğü kullanabilirsiniz. Sonuçları sayısal dizinler yerine metne eşleyebilir, yorum için genellikle daha kolay olur. Ancak, sözlüğün kaydedilmesi daha uzun sürer ve ek depolama alanı kullanacaktır.

9. **En büyük Ngram sözlüğü boyutu**için, n-gram sözlüğünde oluşturulabilen toplam satır sayısını yazın.

    Bu seçenek, sözlüğün boyutunu denetlemek için faydalıdır. Ancak, girişte Ngram sayısı bu boyutu aşarsa, çakışmalar meydana gelebilir.

10. İşlem hattını gönderme. Lda modülü, tek sözcüklerle hangi konuların ilişkilendirileyeceğini belirlemek için Bayes teoreminin kullanır. Sözcükler, hiçbir konu veya grup ile özel olarak ilişkilendirilmez; Bunun yerine her n gram, bulunan sınıfların herhangi biriyle ilişkilendirilebilen bir olasılığa sahiptir.

## <a name="results"></a>Sonuçlar

Modülün iki çıkışı vardır:

+ **Dönüştürülmüş veri kümesi**: her kategori için her bir metin örneği için puanlar ile birlikte giriş metnini ve belirtilen sayıda kategoriyi içerir.

+ **Özellik konu matrisi**: en soldaki sütun Ayıklanan metin özelliğini içerir ve bu kategoride bu özelliğin Puanını içeren her bir kategori için bir sütun vardır.


### <a name="lda-transformation"></a>LDA dönüşümü

Bu modül ayrıca veri kümesine LDA uygulayan *lda dönüşümünü* verir.

Bu dönüştürmeyi, modülün sağ bölmesindeki **çıktılar + Günlükler** sekmesinden Kaydet ' in altında bulunan veri kümesi ile kaydedebilir ve diğer veri kümeleri için yeniden kullanabilirsiniz. Bu, büyük bir CorpU üzerinde eğitilen ve katsayıları veya kategorileri yeniden kullanmak istediğiniz durumlarda yararlı olabilir.

### <a name="refining-an-lda-model-or-results"></a>Bir LDA modelini veya sonuçlarını iyileştirme

Genellikle, tüm ihtiyaçları karşılayan tek bir LDA modeli oluşturamaz ve hatta bir görev için tasarlanan bir model, doğruluğu artırmak için birçok yineleme gerektirebilir. Modelinizi geliştirmek için tüm bu yöntemleri denemenizi öneririz:

+ Model parametrelerini değiştirme
+ Sonuçları anlamak için görselleştirmeyi kullanma
+ Oluşturulan konuların yararlı olup olmadığını belirlemek için konuyla ilgili uzmanlardan geri bildirim alma.

Nitel ölçüleri sonuçları değerlendirmek için de kullanışlı olabilir. Konu modelleme sonuçlarını değerlendirmek için şunları göz önünde bulundurun:

+ Doğruluk-benzer öğeler gerçekten benzerdir mi?
+ Ayrımlılık-iş sorunu için gerektiğinde model benzer öğeler arasında ayırt edilebilir mi?
+ Ölçeklenebilirlik-çok çeşitli metin kategorileri mi yoksa yalnızca dar bir hedef etki alanı üzerinde mi çalışıyor?

LDA tabanlı modellerin doğruluğu, genellikle metni temizlemek, özetlemek ve basitleştirmek ya da kategorilere ayırmak için doğal dil işleme kullanılarak artırılabilir. Örneğin, tüm Azure Machine Learning desteklenmiş olan aşağıdaki teknikler, sınıflandırma doğruluğunu iyileştirebilir:

+ Word kaldırmayı durdur

+ Örnek olay normalleştirme

+ Katileştirme veya sözcük ayırmayı çözme

+ Adlandırılmış varlık tanıma

Daha fazla bilgi için bkz. [preprocess Text](preprocess-text.md).

Tasarımcıda metin işleme için R veya Python kitaplıklarını da kullanabilirsiniz: [r betiği Yürüt](execute-r-script.md), [Python betiğini Yürüt](execute-python-script.md)



## <a name="technical-notes"></a>Teknik notlar

Bu bölümde, sık sorulan soruların uygulama ayrıntıları, ipuçları ve yanıtları yer almaktadır.

### <a name="implementation-details"></a>Uygulama ayrıntıları

Varsayılan olarak, dönüştürülmüş veri kümesi ve özellik konu matrisi için çıkış dağıtımları, olasılıklara göre normalleştirillerdir.

+ Dönüştürülmüş veri kümesi, bir belge verilen konuların koşullu olasılığı olarak normalleştirilir. Bu durumda, her satırın toplamı 1 eşittir.

+ Özellik konu matrisi, bir konu verilen sözcüklerin koşullu olasılığı olarak normalleştirilir. Bu durumda, her sütunun toplamı 1 ' dir.

> [!TIP]
> Bazen modül, genellikle algoritmanın sözde rastgele başlatılmasından kaynaklanan boş bir konu döndürebilir.  Bu durumda, N-gram sözlüğünün en büyük boyutu veya özellik karması için kullanılacak bit sayısı gibi ilgili parametreleri değiştirmeyi deneyebilirsiniz.

### <a name="lda-and-topic-modeling"></a>LDA ve konu modelleme

Latent Dirichlet ayırması (LDA), genellikle sınıflandırılmayan metinden daha fazla bilgi edinmek için *içerik tabanlı konu modelleme*için kullanılır. İçerik tabanlı konu modellemesinde, bir konu, sözcüklerden bir dağıtım olur.

Örneğin, çok sayıda ürün içeren müşteri incelemeleriyle ilgili bir yapı sağladığınızı varsayalım. Zaman içinde birçok müşteri tarafından gönderilen incelemelerin metni, bazıları birden çok konuda kullanılan birçok terim içerir.

LDA işlemi tarafından tanımlanan **Konu** , tek bir ürüne yönelik İncelemeleri temsil edebilir veya bir ürün incelemeleri grubunu temsil edebilir. LDA, konunun kendisi bir sözcük kümesi için zaman içinde yalnızca bir olasılık dağılımı olur.

Koşullar bir ürüne nadiren özeldir, ancak diğer ürünlere başvurabilir veya her şeye ("harika", "en uygun") uygulanabilecek genel koşullar olabilir. Diğer terimler gürültü kelimeleri olabilir.  Ancak, LDA yönteminin, Universe 'deki tüm sözcükleri yakalamak ya da sözcüklerin nasıl ilişkili olduğunu anlamak için, ortak oluşumun olasılıkların yanı sıra, LDA yönteminin bir bağlantı noktası olduğunu anlamak önemlidir. Yalnızca hedef etki alanında kullanılan kelimeleri grupleyebilir.

Dizin terimi hesaplandıktan sonra, tek tek metin satırları, iki metin parçasının birbiriyle aynı olup olmadığını anlamak için uzaklık tabanlı bir benzerlik ölçüsü kullanılarak karşılaştırılır.  Örneğin, ürünün kesin bağıntılı birden çok adı olduğunu fark edebilirsiniz. Ya da kesin negatif koşulların genellikle belirli bir ürünle ilişkili olduğunu fark edebilirsiniz. İlgili terimleri tanımlamak ve öneriler oluşturmak için, benzerlik ölçüsünü her ikisini de kullanabilirsiniz.

###  <a name="module-parameters"></a>Modül parametreleri

|Adı|Tür|Aralık|İsteğe Bağlı|Varsayılan|Açıklama|  
|----------|----------|-----------|--------------|-------------|-----------------|  
|Hedef sütun (ler)|Sütun seçimi||Gerekli|StringFeature|Hedef sütun adı veya dizini|  
|Modeledilecek Konu sayısı|Tamsayı|[1; 1000]|Gerekli|5|Belge dağıtımını N konu başlıklarına göre modelleyin|  
|N-gram|Tamsayı|[1; 10]|Gerekli|2|Karma oluşturma sırasında oluşturulan N-gram sırası|  
|Normalleştir|Boole|True veya false|Gerekli|true|Çıktıyı olasılıklara normalleştirin.  Dönüştürülmüş veri kümesi P (konu&#124;belge) olur ve özellik konu matrisi P (Word&#124;konu başlığı) olacaktır|  
|Tüm seçenekleri göster|Boole|True veya false|Gerekli|False|Scikit 'e özgü ek parametreler sunar-çevrimiçi LDA öğrenin|  
|Ro parametresi|Kayan|[0.00001; 1.0]|**Tüm seçenekleri göster** onay kutusu seçildiğinde geçerlidir|0,01|Önceki konu sözcüğü dağıtım|  
|Alpha parametresi|Kayan|[0.00001; 1.0]|**Tüm seçenekleri göster** onay kutusu seçildiğinde geçerlidir|0,01|Dağıtım öncesi belge konusu|  
|Tahmini belge sayısı|Tamsayı|[1; int. Değerini|**Tüm seçenekleri göster** onay kutusu seçildiğinde geçerlidir|1000|Tahmini belge sayısı (total_samples parametreye karşılık gelir)|  
|Toplu işin boyutu|Tamsayı|[1; 1024]|**Tüm seçenekleri göster** onay kutusu seçildiğinde geçerlidir|32|Toplu işin boyutu|  
|Öğrenme oranı güncelleştirme zamanlaması 'nda kullanılan yinelemenin başlangıç değeri|Tamsayı|[0; int. Değerini|**Tüm seçenekleri göster** onay kutusu seçildiğinde geçerlidir|0|Erken yinelemeler için öğrenme oranını aşağı doğru örneklerdeki ilk değer. Learning_offset parametresine karşılık gelir|  
|Güncelleştirmeler sırasında yinelemeye uygulanan güç|Kayan|[0.0; 1.0]|**Tüm seçenekleri göster** onay kutusu seçildiğinde geçerlidir|0,5|Öğrenme oranını denetlemek için yineleme sayısına güç uygulandı. Learning_decay parametresine karşılık gelir |  
|Eğitim yinelemesi sayısı|Tamsayı|[1; 1024]|**Tüm seçenekleri göster** onay kutusu seçildiğinde geçerlidir|25|Eğitim yinelemesi sayısı|  
|Ngram sözlüğü oluşturma|Boole|True veya false|**Tüm seçenekleri göster** onay *kutusu seçili olmadığında* geçerlidir|True|Bilgi işlem bilmesinden önce Ngram sözlüğü oluşturur. Model İnceleme ve yorumu için yararlı|  
|Ngram sözlüğünün en büyük boyutu|Tamsayı|[1; int. Değerini|**Ngram 'ın derleme sözlüğü** seçeneği doğru olduğunda geçerlidir|20000|Ngram sözlüğünün en büyük boyutu. Girişte bulunan belirteçlerin sayısı bu boyutu aşarsa, çakışmalar oluşabilir|  
|Özellik karma için kullanılacak bit sayısı|Tamsayı|[1; 31]|**Tüm seçenekleri göster** onay kutusu seçili olmadığında ve **Ngram derleme sözlüğü** *false olduğunda geçerlidir*|12|Özellik karma için kullanılacak bit sayısı| 
|LDA öncesinde Ngram sözlüğü oluştur|Boole|True veya false|**Tüm seçenekleri göster** onay kutusu seçildiğinde geçerlidir|True|LDA ' dan önce Ngram sözlüğü oluşturur. Model İnceleme ve yorumu için yararlı|  
|Sözlükte en fazla Ngram sayısı|Tamsayı|[1; int. Değerini|**Tüm seçenekleri göster** onay kutusu seçildiğinde ve **Ngram yapı sözlüğü** seçeneği doğru olduğunda geçerlidir|20000|Sözlüğün en büyük boyutu. Girişte bulunan belirteçlerin sayısı bu boyutu aşarsa, çakışmalar oluşabilir|  
|Karma bitlerin sayısı|Tamsayı|[1; 31]|**Tüm seçenekleri göster** onay kutusu seçildiğinde ve **Ngram derleme sözlüğü** seçeneği false olduğunda geçerlidir|12|Özellik karması sırasında kullanılacak bit sayısı|   


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın.   
Modüllere özgü hataların listesi için bkz. [Tasarımcı Için özel durumlar ve hata kodları](designer-error-codes.md).
