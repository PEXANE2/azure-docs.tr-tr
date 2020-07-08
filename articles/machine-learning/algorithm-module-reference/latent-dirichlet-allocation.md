---
title: 'Latent Dirichlet ayırması: modül başvurusu'
titleSuffix: Azure Machine Learning
description: Aksi halde sınıflandırılmayan metinleri kategorilere ayırmak için, latent Dirichlet ayırma modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 06/05/2020
ms.openlocfilehash: 2fa969b6dd89000b4d669bc5d42aa09b3cf3a2b2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84751689"
---
# <a name="latent-dirichlet-allocation-module"></a>Latent Dirichlet ayırma modülü

Bu makalede, Azure Machine Learning Tasarımcısı 'nda (Önizleme), aksi takdirde sınıflandırılabilen metni kategorilere ayırmak için (Önizleme), denetimçi Dirichlet ayırma modülünün nasıl kullanılacağı açıklanır. 

Latent Dirichlet ayırması (LDA), benzer metinleri bulmak için genellikle doğal dil işlemede kullanılır. Diğer bir yaygın terim, *Konu modellemesi*.

Bu modül bir metin sütunu alır ve bu çıktıları oluşturur:

+ Kaynak metin, her kategori için bir puanla birlikte

+ Her kategori için Ayıklanan terimleri ve katsayıları içeren bir özellik matrisi

+ Giriş olarak kullanılan yeni metinlere kaydedip yeniden uygulayabilmeniz için bir dönüşüm

Bu modül, scikit-öğren kitaplığını kullanır. Scikit-öğrenme hakkında daha fazla bilgi için bkz. öğretici ve algoritmaların açıklaması içeren [GitHub deposu](https://github.com/scikit-learn/scikit-learn).

## <a name="more-about-latent-dirichlet-allocation"></a>Görünmeyen Dirichlet ayırması hakkında daha fazla bilgi

LDA, sınıflandırma için genellikle bir yöntem değildir. Ancak, yaygın bir yaklaşım kullandığından, bilinen sınıf etiketleri sağlamanız ve sonra desenleri çıkarmaları gerekmez.  Bunun yerine, algoritma, konu gruplarını tanımlamak için kullanılan bir dayalı modeli oluşturur. Mevcut eğitim çalışmalarını veya modele giriş olarak sağladığınız yeni servis taleplerini sınıflandırmak için dayalı modelini kullanabilirsiniz.

Metin ve kategoriler arasındaki ilişki hakkında güçlü varsayımlar yapılmasını önlediği için bir genel modeli tercih edebilirsiniz. Bu, yalnızca matematik ve matematiksel olarak modelleme konularına yönelik sözcükleri kullanır.

Bu, bir PDF indirmesi olarak sunulan bu kağıda tartışılır: [latent Dirichlet ayırması: Blei, NG ve Ürdün](https://ai.stanford.edu/~ang/papers/nips01-lda.pdf).

Bu modüldeki uygulama, LDA için [scikit-öğren kitaplığını](https://github.com/scikit-learn/scikit-learn/blob/master/sklearn/decomposition/_lda.py) temel alır.

Daha fazla bilgi için [Teknik notlar](#technical-notes) bölümüne bakın.

## <a name="how-to-configure-latent-dirichlet-allocation"></a>Latent Dirichlet ayırmayı yapılandırma

Bu modül, ham veya önceden işlenmiş bir metin sütunu içeren bir veri kümesi gerektirir.

1. İşlem hattınızı **latent Dirichlet ayırma** modülünü ekleyin.

2. Modülün girişi olarak, bir veya daha fazla metin sütunu içeren bir veri kümesi sağlayın.

3. **Hedef sütunlar**için, çözümlenecek metni içeren bir veya daha fazla sütun seçin.

    Birden çok sütun seçebilirsiniz, ancak **dize** veri türünde olmaları gerekir.

    LDA metinden büyük bir özellik matrisi oluşturduğundan, genellikle tek bir metin sütunu analiz edersiniz.

4. **Model oluşturma konuları**için, giriş metninde kaç kategori veya konu türediğini belirten 1 ile 1000 arasında bir tamsayı girin.

    Varsayılan olarak, 5 konu oluşturulur.

5. **N-gram**için, karma oluşturma sırasında oluşturulan n-gram maksimum uzunluğunu belirtin.

    Varsayılan değer 2 ' dir, yani hem bigram hem de unigram oluşturulur.

6. Çıktı değerlerini olasılıklara dönüştürmek için **Normalleştir** seçeneğini belirleyin. 

    Dönüştürülmüş değerleri tamsayılar olarak temsil etmek yerine, Output ve Feature veri kümesindeki değerler aşağıdaki gibi dönüştürülür:

    + Veri kümesindeki değerler bir olasılık olarak temsil edilir `P(topic|document)` .

    + Özellik konu matrisi içindeki değerler bir olasılık olarak temsil edilir `P(word|topic)` .

    > [!NOTE] 
    > Azure Machine Learning tasarımcı 'da (Önizleme), scikit-öğren kitaplığı artık 0,19 sürümünden normalleştirilmiş olmayan *doc_topic_distr* çıktıyı desteklememektedir. Bu modülde, **normalize** parametresi yalnızca *özellik konu matrisi* çıktısına uygulanabilir. *Dönüştürülmüş veri kümesi* çıkışı her zaman normalleştirilmelidir.

7. **Tüm seçenekleri göster**seçeneğini belirleyin ve ardından aşağıdaki Gelişmiş parametreleri ayarlamak Istiyorsanız bunu **true** olarak ayarlayın.

    Bu parametreler LDA 'nın scikit-öğrenme uygulamasına özgüdür. Scikit-öğrenme konusunda ve resmi [scikit-öğrenme belgesi](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.LatentDirichletAllocation.html)hakkında bazı iyi öğreticiler vardır.

    + **Ro parametresi**. Konu dağıtımları seyrekliği için önceki bir olasılık sağlayın. Bu parametre, sköğren 'in parametresine karşılık gelir `topic_word_prior` . Sözcüklerin dağılımının düz olduğunu düşünüyorsanız **1** değerini kullanın; diğer bir deyişle, tüm kelimeler eşit kabul edilir. Çoğu sözcüğün çok seyrek görüntülendiğini düşünüyorsanız, daha düşük bir değere ayarlayabilirsiniz.

    + **Alfa parametresi**. Belge başına seyrekliği için bir önceki olasılık belirtin. Bu parametre, sköğren 'in parametresine karşılık gelir `doc_topic_prior` .

    + **Tahmini belge sayısı**. İşlenecek belge sayısı (satır) için en iyi tahmininizi temsil eden bir sayı girin. Bu parametre, modülün yeterli büyüklükte bir karma tablosu ayırmasını sağlar. `total_samples`Scikit-öğren içindeki parametreye karşılık gelir.

    + **Toplu Işin boyutu**. LDA modeline gönderilen her metin toplu işinde kaç satır ekleneceğini gösteren bir sayı girin. Bu parametre, `batch_size` scikit-öğren içindeki parametreye karşılık gelir.

    + **Öğrenme güncelleştirme zamanlaması 'nda kullanılan yinelemenin başlangıç değeri**. Çevrimiçi öğrenmedeki Erken yinelemeler için öğrenme oranını aşağı örneklerdeki başlangıç değerini belirtin. Bu parametre, `learning_offset` scikit-öğren içindeki parametreye karşılık gelir.

    + **Güncelleştirmeler sırasında yinelemeye uygulanan güç**. Çevrimiçi güncelleştirmeler sırasında öğrenme oranını denetlemek için yineleme sayısına uygulanan güç düzeyini belirtin. Bu parametre, `learning_decay` scikit-öğren içindeki parametreye karşılık gelir.

    + **Veri üzerinden geçen geçiş sayısı**. Algoritmanın verilerin üzerinde kaç kez dolacaktır için en fazla sayıyı belirtin. Bu parametre, `max_iter` scikit-öğren içindeki parametreye karşılık gelir.

8. Metni sınıflandırmadan önce bir ilk geçişte n-gram listesini oluşturmak istiyorsanız, **Ngram** 'lar veya **lda öncesinde Ngram 'lar için**derleme sözlüğü seçeneğini belirleyin.

    Başlangıç sözlüğünü önceden oluşturursanız, daha sonra modeli gözden geçirirken sözlüğü kullanabilirsiniz. Sonuçları sayısal dizinler yerine metne eşleyebilir, yorum için genellikle daha kolay olur. Ancak, sözlüğün kaydedilmesi daha uzun sürer ve ek depolama alanı kullanacaktır.

9. **En büyük Ngram sözlüğü boyutu**için, n-gram sözlüğünde oluşturulabilen toplam satır sayısını girin.

    Bu seçenek, sözlüğün boyutunu denetlemek için faydalıdır. Ancak, girişte Ngram sayısı bu boyutu aşarsa, çakışmalar meydana gelebilir.

10. İşlem hattını gönderme. Lda modülü, tek sözcüklerle hangi konuların ilişkilendirileyeceğini belirlemek için Bayes teoreminin kullanır. Sözcükler, hiçbir konu veya grup ile özel olarak ilişkili değildir. Bunun yerine her n gram, bulunan sınıfların herhangi biriyle ilişkilendirilebilen bir olasılığa sahiptir.

## <a name="results"></a>Sonuçlar

Modülün iki çıkışı vardır:

+ **Dönüştürülmüş veri kümesi**: Bu çıktı, giriş metnini, belirtilen sayıda kategoriyi ve her bir kategorinin her bir metin örneğine ilişkin puanları içerir.

+ **Özellik konu matrisi**: en soldaki sütun Ayıklanan metin özelliğini içerir. Her kategori için bir sütun ilgili kategorideki bu özelliğin Puanını içerir.


### <a name="lda-transformation"></a>LDA dönüşümü

Bu modül ayrıca veri kümesine LDA uygulayan *lda dönüşümünü* verir.

Bu dönüştürmeyi kaydedebilir ve diğer veri kümeleri için yeniden kullanabilirsiniz. Bu teknik, büyük bir CorpU üzerinde eğitim yaptıysanız ve katsayıları veya kategorileri yeniden kullanmak istiyorsanız yararlı olabilir.

Bu dönüştürmeyi yeniden kullanmak için, modülün modül listesindeki **veri kümeleri** kategorisi altında kalmasını sağlamak Için, latent Dirichlet ayırma modülünün sağ panelindeki **veri kümesini kaydet** simgesini seçin. Daha sonra bu dönüştürmeyi yeniden kullanmak için bu modülü [dönüştürme uygula](apply-transformation.md) modülüne bağlayabilirsiniz.

### <a name="refining-an-lda-model-or-results"></a>Bir LDA modelini veya sonuçlarını iyileştirme

Genellikle, tüm ihtiyaçları karşılayacak tek bir LDA modeli oluşturamazsınız. Bir görev için tasarlanan bir model bile doğruluğu artırmak için çok sayıda yineleme gerektirebilir. Modelinizi geliştirmek için tüm bu yöntemleri denemenizi öneririz:

+ Model parametrelerini değiştirme
+ Sonuçları anlamak için görselleştirmeyi kullanma
+ Oluşturulan konuların yararlı olup olmadığını belirleme konulu uzmanların geri bildirimini alma

Nitel ölçüleri sonuçları değerlendirmek için de kullanışlı olabilir. Konu modelleme sonuçlarını değerlendirmek için şunları göz önünde bulundurun:

+ Veritabanınızın. Benzer öğeler gerçekten benzerdir mi?
+ Lılık. İş sorunu için gerektiğinde model benzer öğeler arasında ayırt edilebilir mi?
+ Ölçeklendirilebilirlik. Çok çeşitli metin kategorileri üzerinde mi yoksa yalnızca dar bir hedef etki alanı üzerinde mi çalışıyor?

Genellikle, metni temizlemek, özetlemek ve basitleştirmek ya da kategorilere ayırmak için doğal dil işlemeyi kullanarak LDA modellerini temel alan modellerin doğruluğunu geliştirebilirsiniz. Örneğin, tüm Azure Machine Learning desteklenmiş olan aşağıdaki teknikler, sınıflandırma doğruluğunu iyileştirebilir:

+ Word kaldırmayı durdur

+ Örnek olay normalleştirme

+ Katileştirme veya sözcük ayırmayı çözme

+ Adlandırılmış varlık tanıma

Daha fazla bilgi için bkz. [preprocess Text](preprocess-text.md).

Tasarımcıda metin işleme için R veya Python kitaplıklarını da kullanabilirsiniz: [r betiği Yürüt](execute-r-script.md), [Python betiğini Yürüt](execute-python-script.md).



## <a name="technical-notes"></a>Teknik notlar

Bu bölümde, sık sorulan soruların uygulama ayrıntıları, ipuçları ve yanıtları yer almaktadır.

### <a name="implementation-details"></a>Uygulama ayrıntıları

Varsayılan olarak, dönüştürülmüş bir veri kümesi ve özellik konu matrisi için çıkış dağıtımları, olasılıklara göre normalleştirillerdir:

+ Dönüştürülmüş veri kümesi, bir belge verilen konuların koşullu olasılığı olarak normalleştirilir. Bu durumda, her satırın toplamı 1 eşittir.

+ Özellik konu matrisi, bir konu verilen sözcüklerin koşullu olasılığı olarak normalleştirilir. Bu durumda, her sütunun toplamı 1 ' dir.

> [!TIP]
> Bazen modül boş bir konu döndürebilir. Çoğu zaman, algoritmanın sözde rastgele başlatılmasından bu neden olur. Bu durumda, ilgili parametreleri değiştirmeyi deneyebilirsiniz. Örneğin, N-gram sözlüğünün en büyük boyutunu veya özellik karması için kullanılacak bit sayısını değiştirin.

### <a name="lda-and-topic-modeling"></a>LDA ve konu modelleme

Görünmeyen metinden daha fazla bilgi edinmek için, bu durum genellikle *içerik tabanlı konu modelleme*için kullanılır. İçerik tabanlı konu modellemesinde, bir konu, sözcüklerden bir dağıtım olur.

Örneğin, çok sayıda ürün içeren müşteri incelemelerinin bir CorpU sağladığınızı varsayalım. Zaman içinde müşteriler tarafından gönderilen incelemelerin metni, bazıları birden çok konuda kullanılan birçok terim içerir.

LDA işleminin tanımladığı bir *Konu* , bireysel bir ürüne yönelik İncelemeleri temsil edebilir veya bir ürün incelemeleri grubunu temsil edebilir. LDA, konunun kendisi bir sözcük kümesi için zaman içinde yalnızca bir olasılık dağılımı olur.

Koşullar, herhangi bir ürüne nadiren özeldir. Diğer ürünlere başvurabilirler veya her şeye ("harika", "en uygun") uygulanabilecek genel koşullar olabilir. Diğer terimler gürültü kelimeleri olabilir. Ancak, LDA yöntemi, Universe 'deki tüm kelimeleri yakalamaya veya sözcüklerin nasıl ilişkili olduğunu anlamak için, ortak oluşumun olasılıkların yanı sıra bir bütün sözcükleri yakalamayı denemez. Yalnızca hedef etki alanında kullanılan kelimeleri grupleyebilir.

Dizin terimi hesaplandıktan sonra, uzaklık tabanlı bir benzerlik ölçüsü, iki satırlık metin parçasının benzer olup olmadığını anlamak için tek tek metin satırlarını karşılaştırır. Örneğin, ürünün kesin bağıntılı birden çok adı olduğunu fark edebilirsiniz. Ya da kesin negatif koşulların genellikle belirli bir ürünle ilişkili olduğunu fark edebilirsiniz. İlgili terimleri tanımlamak ve öneriler oluşturmak için, benzerlik ölçüsünü her ikisini de kullanabilirsiniz.

###  <a name="module-parameters"></a>Modül parametreleri

|Name|Tür|Aralık|İsteğe Bağlı|Varsayılan|Açıklama|  
|----------|----------|-----------|--------------|-------------|-----------------|  
|Hedef sütun (ler)|Sütun seçimi||Gerekli|StringFeature|Hedef sütun adı veya dizini.|  
|Modeledilecek Konu sayısı|Tamsayı|[1; 1000]|Gerekli|5|Belge dağıtımını N konu başlıklarına göre modelleyin.|  
|N-gram|Tamsayı|[1; 10]|Gerekli|2|Karma oluşturma sırasında oluşturulan N-gram sırası.|  
|Normalleştir|Boole|True veya false|Gerekli|true|Çıktıyı olasılıklara normalleştirin.  Dönüştürülmüş veri kümesi P (konu&#124;belge) olur ve özellik konu matrisi P (Word&#124;konu başlığı) olur.|  
|Tüm seçenekleri göster|Boole|True veya false|Gerekli|False|, Scikit 'e özgü ek parametreler sunar ve çevrimiçi LDA öğrenin.|  
|Ro parametresi|Float|[0.00001; 1.0]|**Tüm seçenekleri göster** onay kutusu seçildiğinde geçerlidir|0,01|Önceki dağıtım konu başlığı.|  
|Alpha parametresi|Float|[0.00001; 1.0]|**Tüm seçenekleri göster** onay kutusu seçildiğinde geçerlidir|0,01|Dağıtım öncesi belge konusu.|  
|Tahmini belge sayısı|Tamsayı|[1; int. Değerini|**Tüm seçenekleri göster** onay kutusu seçildiğinde geçerlidir|1000|Tahmini belge sayısı. Parametresine karşılık gelir `total_samples` .|  
|Toplu işin boyutu|Tamsayı|[1; 1024]|**Tüm seçenekleri göster** onay kutusu seçildiğinde geçerlidir|32|Toplu işin boyutu.|  
|Öğrenme oranı güncelleştirme zamanlaması 'nda kullanılan yinelemenin başlangıç değeri|Tamsayı|[0; int. Değerini|**Tüm seçenekleri göster** onay kutusu seçildiğinde geçerlidir|0|Erken yinelemeler için öğrenme oranını aşağı doğru örneklerdeki ilk değer. Parametresine karşılık gelir `learning_offset` .|  
|Güncelleştirmeler sırasında yinelemeye uygulanan güç|Float|[0.0; 1.0]|**Tüm seçenekleri göster** onay kutusu seçildiğinde geçerlidir|0,5|Öğrenme oranını denetlemek için yineleme sayısına güç uygulandı. Parametresine karşılık gelir `learning_decay` . |  
|Eğitim yinelemesi sayısı|Tamsayı|[1; 1024]|**Tüm seçenekleri göster** onay kutusu seçildiğinde geçerlidir|25|Eğitim yinelemesi sayısı.|  
|Ngram sözlüğü oluşturma|Boole|True veya false|**Tüm seçenekleri göster** onay *kutusu seçili olmadığında* geçerlidir|True|Bilgi işlem bilmesinden önce Ngram sözlüğü oluşturur. Model İnceleme ve yorumu için faydalıdır.|  
|Ngram sözlüğünün en büyük boyutu|Tamsayı|[1; int. Değerini|**Ngram 'ın derleme sözlüğü** seçeneği **doğru** olduğunda geçerlidir|20000|Ngram sözlüğünün en büyük boyutu. Giriş içindeki belirteçlerin sayısı bu boyutu aşarsa, çakışmalar meydana gelebilir.|  
|Özellik karma için kullanılacak bit sayısı.|Tamsayı|[1; 31]|**Tüm seçenekleri göster** onay *kutusu seçili olmadığında* ve **Ngram derleme sözlüğü** **false** olduğunda geçerlidir|12|Özellik karma için kullanılacak bit sayısı.| 
|LDA öncesinde Ngram sözlüğü oluştur|Boole|True veya false|**Tüm seçenekleri göster** onay kutusu seçildiğinde geçerlidir|True|LDA ' dan önce Ngram sözlüğü oluşturur. Model İnceleme ve yorumu için faydalıdır.|  
|Sözlükte en fazla Ngram sayısı|Tamsayı|[1; int. Değerini|**Tüm seçenekleri göster** onay kutusu seçildiğinde ve **Ngram oluşturma sözlüğü** seçeneği **doğru** olduğunda geçerlidir|20000|Sözlüğün en büyük boyutu. Giriş içindeki belirteçlerin sayısı bu boyutu aşarsa, çakışmalar meydana gelebilir.|  
|Karma bitlerin sayısı|Tamsayı|[1; 31]|**Tüm seçenekleri göster** onay kutusu seçildiğinde ve **Ngram derleme sözlüğü** seçeneği **false** olduğunda geçerlidir|12|Özellik karması sırasında kullanılacak bit sayısı.|   


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 

Modüllere özgü hataların listesi için bkz. [Tasarımcı Için özel durumlar ve hata kodları](designer-error-codes.md).
