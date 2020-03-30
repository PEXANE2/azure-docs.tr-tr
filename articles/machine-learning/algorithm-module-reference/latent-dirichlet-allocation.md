---
title: Gizli Dirichlet Tahsisi
titleSuffix: Azure Machine Learning
description: Gizli Dirichlet Tahsisi modülünün, sınıflandırılmamış metinleri bir dizi kategoride gruplandırmak için nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 03/11/2020
ms.openlocfilehash: 1384491489c175ffc338f80a99aa8d5050f835d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80109231"
---
# <a name="latent-dirichlet-allocation"></a>Gizli Dirichlet Tahsisi

Bu makalede, aksi takdirde sınıflandırılmamış metinleri çeşitli kategorilerde gruplandırmak için Azure Machine Learning tasarımcısında (önizleme) **Gizli Dirichlet Ayırma** modülünün nasıl kullanılacağı açıklanmaktadır. 

Latent Dirichlet Ayırma (LDA) genellikle benzer metinleri bulmak için doğal dil işleme (NLP) kullanılır. Başka bir yaygın terim *konu modelleme*olduğunu.

Bu modül bir metin sütunu alır ve şu çıktıları oluşturur:

+ Kaynak metin, her kategori için bir puan ile birlikte

+ Her kategori için ayıklanmış terimler ve katsayılar içeren bir özellik matrisi

+ Giriş olarak kullanılan yeni metne kaydedip yeniden uygulayabileceğiniz bir dönüşüm

Bu modül scikit-learn kitaplığını kullanır. Scikit-learn hakkında daha fazla bilgi için, öğreticiler ve algoritmanın açıklamasını içeren [GitHub deposuna bakın.

### <a name="more-about-latent-dirichlet-allocation-lda"></a>Latent Dirichlet Tahsisi (LDA) hakkında daha fazla bilgi

Genel olarak konuşursak, LDA kendi başına sınıflandırma yöntemi değildir, ancak üretici bir yaklaşım kullanır. Bunun anlamı, bilinen sınıf etiketleri sağlamanız ve ardından desenleri çıkaramanız gerekolmadığıdır.  Bunun yerine, algoritma konu gruplarını tanımlamak için kullanılan olasılıksal bir model oluşturur. Varolan eğitim örneklerini veya modele sağladığınız yeni servis taleplerini giriş olarak sınıflandırmak için olasılıksal modeli kullanabilirsiniz.

Üretken bir model, metin ve kategoriler arasındaki ilişki hakkında güçlü varsayımlarda bulunmaktan kaçındığından ve yalnızca sözcüklerin konuları matematiksel olarak modellemek için dağılımını kullandığından tercih edilebilir.

+ Teori bu makalede, PDF indir olarak kullanılabilir tartışılmıştır: [Latent Dirichlet Tahsisi: Blei, Ng ve Ürdün](https://ai.stanford.edu/~ang/papers/nips01-lda.pdf)

+ Bu modüldeki uygulama LDA için [scikit-learn kütüphanesine](https://github.com/scikit-learn/scikit-learn/blob/master/sklearn/decomposition/_lda.py) dayanmaktadır.

Daha fazla bilgi için [Teknik notlar](#technical-notes) bölümüne bakın.

## <a name="how-to-configure-latent-dirichlet-allocation"></a>Latent Dirichlet Tahsisi nasıl yapılandırılır?

Bu modül, ham veya önceden işlenmiş bir metin sütunu içeren bir veri kümesi gerektirir.

1. **Latent Dirichlet Ayırma** modüllerini boru hattınıza ekleyin.

2. Modül için giriş olarak, bir veya daha fazla metin sütunu içeren bir veri kümesi sağlayın.

3. **Hedef sütunlar**için, çözümlemek için metin içeren bir veya daha fazla sütun seçin.

    Birden çok sütun seçebilirsiniz, ancak bunlar dize veri türünde olmalıdır.

    Genel olarak, LDA metinden büyük bir özellik matrisi oluşturduğundan, genellikle tek bir metin sütununa çözüm ler siniz.

4. **Modellendirilecek konu sayısı**için, giriş metninden kaç kategori veya konu türetmek istediğinizi belirten 1 ile 1000 arasında bir arayla bir arayla bir aratmeör yazın.

    Varsayılan olarak, 5 konu oluşturulur.

5. **N-gram**için, karma sırasında oluşan maksimum N gram uzunluğunu belirtin.

    Varsayılan değer 2'dir, yani hem bigrams hem de tek parça oluşturulur.

6. Çıktı değerlerini olasılıklara dönüştürmek için **Normale Döndürme** seçeneğini seçin. Bu nedenle, dönüştürülmüş değerleri tamsayılar olarak temsil etmek yerine, çıktı ve özellik veri kümesindeki değerler aşağıdaki gibi dönüştürülür:

    + Veri kümesindeki değerler bir olasılık olarak temsil `P(topic|document)`edilecektir nerede .

    + Özellik konu matrisindeki değerler bir olasılık olarak `P(word|topic)`temsil edilecektir.

    > [!NOTE] 
    > Azure Machine Learning tasarımcısında (önizleme), çünkü temel aldığımız kitaplık, scikit-learn, artık sürüm 0.19'dan normalleştirilmemiş *doc_topic_distr* çıktısını desteklemediği için, bu modülde **normale dönüştürme** parametresi yalnızca **Özellik Topic matris** çıktısına uygulanabilir, Dönüştürülmüş veri **kümesi** çıktısı her zaman normalleştirilmiştir.

7. Seçeneği seçin, **tüm seçenekleri göster**ve görüntülemek istiyorsanız TRUE olarak ayarlayın ve sonra ek gelişmiş parametrelerayarlayın.

    Bu parametreler LDA'nın scikit-learn uygulamasına özgüdür. Scikit-learn'te LDA hakkında bazı iyi öğreticilerin yanı sıra resmi [scikit-learn belgesi](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.LatentDirichletAllocation.html)de vardır.

    + **Rho parametresi**. Konu dağılımlarının seyrekliği için önceden bir olasılık sağlayın. SKLearn'nin `topic_word_prior` parametresine karşılık gelir. Sözcüklerin dağılımının düz olmasını beklerseniz 1 değerini kullanırsınız; yani, tüm kelimeler inquiprobable kabul edilir. Çoğu sözcük seyrek görünür düşünüyorsanız, çok daha düşük bir değere ayarlayabilirsiniz.

    + **Alfa parametresi**. Belge başına konu ağırlıklarının seyrekliği için önceki bir olasılık belirtin.  SKLearn'nin `doc_topic_prior` parametresine karşılık gelir.

    + **Tahmini belge sayısı.** İşlenecek belge (satır) sayısı hakkında en iyi tahmininizi temsil eden bir sayı yazın. Bu, modülün yeterli boyutta bir karma tablo ayırmasını sağlar.  Scikit-learn'teki `total_samples` parametreye karşılık gelir.

    + **Toplu iş boyutu**. LDA modeline gönderilen her metin toplu bölümüne kaç satır eklenebildiğini belirten bir sayı yazın. Scikit-learn'teki `batch_size` parametreye karşılık gelir.

    + **Öğrenme güncelleştirme çizelgesinde kullanılan yinelemenin başlangıç değeri.** Çevrimiçi öğrenmede erken yinelemeler için öğrenme oranını düşüren başlangıç değerini belirtin. Scikit-learn'teki `learning_offset` parametreye karşılık gelir.

    + **Güncelleştirmeler sırasında yinelemeye uygulanan güç.** Çevrimiçi güncelleştirmeler sırasında öğrenme oranını kontrol etmek için yineleme sayısına uygulanan güç düzeyini belirtin. Scikit-learn'teki `learning_decay` parametreye karşılık gelir.

    + **Verilerin üzerinden geçen lerin sayısı.** Algoritmanın veriler üzerinde döngüye girecek maksimum kaç kez olacağını belirtin. Scikit-learn'teki `max_iter` parametreye karşılık gelir.

8. Metni sınıflandırmadan önce, **ngrams'dan** önce ngrams sözlüğü oluşturma veya **NDA'dan önce ngrams**sözlüğü oluşturma seçeneğini seçin.

    İlk sözlüğü önceden oluşturursanız, modeli gözden geçirirken sözlüğü daha sonra kullanabilirsiniz. Sonuçları sayısal endeksler yerine metne göre eşlenebilmek genellikle yorumlanması daha kolaydır. Ancak, sözlük tasarrufu daha uzun sürer ve ek depolama kullanın.

9. **Ngram sözlüğünün maksimum boyutu**için, n-gram sözlüğünde oluşturulabilecek toplam satır sayısını yazın.

    Bu seçenek, sözlüğün boyutunu denetlemek için yararlıdır. Ancak, girişteki ngrams sayısı bu boyutu aşarsa, çakışması oluşabilir.

10. Boru hattını gönderin. LDA modülü, hangi konuların tek tek sözcüklerle ilişkilendirilebileceğini belirlemek için Bayes teoremi kullanır. Sözcükler yalnızca herhangi bir konu veya grupla ilişkili değildir; bunun yerine, her n-gram keşfedilen sınıfların herhangi biriyle ilişkili olma olasılığı öğrenilmiş.

## <a name="results"></a>Sonuçlar

Modülün iki çıkışı vardır:

+ **Dönüştürülmüş veri kümesi**: Her kategori için her metin örneğinin puanlarıyla birlikte giriş metnini ve belirli sayıda keşfedilen kategoriyi içerir.

+ **Özellik konu matrisi**: En soldaki sütun, çıkarılan metin özelliğini içerir ve her kategori için bu kategorideki özelliğin puanını içeren bir sütun vardır.


### <a name="lda-transformation"></a>LDA dönüşümü

Bu modül ayrıca veri kümesine LDA uygulayan *LDA dönüşümlerini* de ortaya çıkar.

Bu dönüşümü, modülün sağ bölmesinde **Outputs+logs** sekmesi altında kaydederek kaydedebilir ve diğer veri kümeleri için yeniden kullanabilirsiniz. Büyük bir korpus üzerinde eğitim aldıysanız ve katsayıları veya kategorileri yeniden kullanmak istiyorsanız bu yararlı olabilir.

### <a name="refining-an-lda-model-or-results"></a>Bir LDA modelini veya sonuçlarını rafine etme

Genellikle tüm gereksinimleri karşılayacak tek bir LDA modeli oluşturamazsınız ve hatta bir görev için tasarlanmış bir model doğruluğu artırmak için birçok yineleme gerektirebilir. Modelinizi geliştirmek için tüm bu yöntemleri denemenizi öneririz:

+ Model parametrelerini değiştirme
+ Sonuçları anlamak için görselleştirmeyi kullanma
+ Oluşturulan konuların yararlı olup olmadığını belirlemek için konu uzmanlarının geri bildirimlerini almak.

Nitel önlemler de sonuçları değerlendirmek için yararlı olabilir. Konu modelleme sonuçlarını değerlendirmek için şunları göz önünde bulundurun:

+ Doğruluk - Benzer öğeler gerçekten benzer mi?
+ Çeşitlilik - Model, iş sorunu için gerektiğinde benzer öğeler arasında ayrım yapabilir mi?
+ Ölçeklenebilirlik - Çok çeşitli metin kategorilerinde mi yoksa yalnızca dar hedef alan adetkialanında mı çalışıyor?

LDA tabanlı modellerin doğruluğu genellikle metni temizlemek, özetlemek ve basitleştirmek veya kategorilere ayırmak için doğal dil işleme kullanılarak geliştirilebilir. Örneğin, tümü Azure Machine Learning'de desteklenen aşağıdaki teknikler sınıflandırma doğruluğunu artırabilir:

+ Sözcük kaldırmayı durdurma

+ Büyük/küçük harf normalleştirme

+ Lemmatizasyon veya saplanma

+ Adlandırılmış varlık tanıma

Daha fazla bilgi için [İşlem Öncesi Metin'e](preprocess-text.md)bakın.

Tasarımcıda, metin işleme için R veya Python kitaplıklarını da kullanabilirsiniz: [R Script'i çalıştırın,](execute-r-script.md) [Python Scriptini Çalıştırın](execute-python-script.md)



## <a name="technical-notes"></a>Teknik notlar

Bu bölümde uygulama ayrıntıları, ipuçları ve sık sorulan soruların yanıtları içerir.

### <a name="implementation-details"></a>Uygulama ayrıntıları

Varsayılan olarak, dönüştürülmüş veri kümesi ve özellik-konu matrisi için çıktı dağılımları olasılık olarak normale döndürür.

+ Dönüştürülen veri kümesi, belge verilen konuların koşullu olasılığı olarak normale döndürür. Bu durumda, her satırın toplamı 1'e eşittir.

+ Özellik-konu matrisi, bir konu verilen sözcüklerin koşullu olasılığı olarak normale döndürilir. Bu durumda, her sütunun toplamı 1'e eşittir.

> [!TIP]
> Bazen modül, genellikle algoritmanın sözde rasgele başlatılmasından kaynaklanan boş bir konuyu döndürebilir.  Bu durumda, N-gram sözlüğünün en büyük boyutu veya özellik karması için kullanılacak bit sayısı gibi ilgili parametreleri değiştirmeyi deneyebilirsiniz.

### <a name="lda-and-topic-modeling"></a>LDA ve konu modelleme

Gizli Dirichlet Ayırma (LDA) genellikle *içerik tabanlı konu modelleme*için kullanılır , hangi temelde sınıflandırılmamış metinden kategoriler öğrenme anlamına gelir. İçerik tabanlı konu modellemesinde, konu sözcükler üzerinde bir dağıtımdır.

Örneğin, çok, çok sayıda ürünü içeren bir müşteri yorumu korpusu sağladığınızı varsayalım. Zaman içinde birçok müşteri tarafından gönderilen inceleme metni, bazıları birden çok konuda kullanılan birçok terim içerir.

LDA işlemi tarafından tanımlanan bir **konu,** tek bir Ürün A için yapılan incelemeleri temsil edebilir veya bir ürün incelemesi grubunu temsil edebilir. LDA için, konunun kendisi bir dizi sözcük için zaman içinde sadece bir olasılık dağılımıdır.

Terimler nadiren herhangi bir ürüne özeldir, ancak diğer ürünlere başvurabilir veya her şey için geçerli olan genel terimler olabilir ("harika", "korkunç"). Diğer terimler gürültü kelimeler olabilir.  Ancak, LDA yönteminin evrendeki tüm sözcükleri yakalamayı ya da kelimelerin birlikte oluşum olasılıkları nın dışında nasıl ilişkili olduğunu anlamadığını anlamak önemlidir. Yalnızca hedef etki alanında kullanılan sözcükleri gruplandırabilir.

Dizinler terimi hesaplandıktan sonra, iki metnin birbirine benzeyip benzemediğini belirlemek için tek tek metin satırları mesafe tabanlı benzerlik ölçüsü kullanılarak karşılaştırılır.  Örneğin, ürünün güçlü bir şekilde ilişkili birden çok adı olduğunu görebilirsiniz. Veya, güçlü negatif terimlergenellikle belirli bir ürün ile ilişkili olduğunu görebilirsiniz. Benzerlik ölçüsünü hem ilgili terimleri tanımlamak hem de öneriler oluşturmak için kullanabilirsiniz.

###  <a name="module-parameters"></a>Modül parametreleri

|Adı|Tür|Aralık|İsteğe bağlı|Varsayılan|Açıklama|  
|----------|----------|-----------|--------------|-------------|-----------------|  
|Hedef sütun(lar)|Sütun Seçimi||Gerekli|StringFeature|Hedef sütun adı veya dizin|  
|Modellenene konu sayısı|Tamsayı|[1;1000]|Gerekli|5|Belge dağıtımını N konularına göre modelleme|  
|N gram|Tamsayı|[1;10]|Gerekli|2|Karma sırasında oluşturulan N gram sırası|  
|Normalleştirmek|Boole|Doğru veya Yanlış|Gerekli|true|Çıktıyı olasılıklara normalleştirin.  Dönüştürülmüş veri kümesi P (konu&#124;belge) ve özellik konu matris P (word&#124;konu) olacaktır|  
|Tüm seçenekleri göster|Boole|Doğru veya Yanlış|Gerekli|False|Scikit-learn online LDA'ya özgü ek parametreler sunar|  
|Rho parametresi|Kayan|[0.00001;1.0]|Tüm seçenekleri **göster** onay kutusu seçildiğinde geçerlidir|0.01|Konu sözcüğü önceki dağıtım|  
|Alfa parametresi|Kayan|[0.00001;1.0]|Tüm seçenekleri **göster** onay kutusu seçildiğinde geçerlidir|0.01|Belge konusu önceki dağıtım|  
|Tahmini belge sayısı|Tamsayı|[1;int. MaxValue]|Tüm seçenekleri **göster** onay kutusu seçildiğinde geçerlidir|1000|Tahmini belge sayısı (total_samples parametreye karşılık gelir)|  
|Toplu iş boyutu|Tamsayı|[1;1024]|Tüm seçenekleri **göster** onay kutusu seçildiğinde geçerlidir|32|Toplu iş boyutu|  
|Öğrenme hızı güncelleştirme çizelgesinde kullanılan yinelemenin başlangıç değeri|Tamsayı|[0;int. MaxValue]|Tüm seçenekleri **göster** onay kutusu seçildiğinde geçerlidir|0|Erken yinelemeler için öğrenme oranını düşüren başlangıç değeri. learning_offset parametresine karşılık gelir|  
|Güncelleştirmeler sırasında yinelemeye uygulanan güç|Kayan|[0.0;1.0]|Tüm seçenekleri **göster** onay kutusu seçildiğinde geçerlidir|0,5|Öğrenme oranını kontrol etmek için yineleme sayısına uygulanan güç. learning_decay parametreye karşılık gelir |  
|Eğitim yinelemelerinin sayısı|Tamsayı|[1;1024]|Tüm seçenekleri **göster** onay kutusu seçildiğinde geçerlidir|25|Eğitim yinelemelerinin sayısı|  
|Ngrams sözlüğü oluştur|Boole|Doğru veya Yanlış|Tüm seçenekleri **göster** onay kutusu *seçilmediğinde* geçerlidir|True|LDA hesaplamadan önce ngrams bir sözlük oluşturur. Model denetimi ve yorumu için yararlıdır|  
|Ngram sözlüğünün maksimum boyutu|Tamsayı|[1;int. MaxValue]|**Ngrams'ın yapı sözlüğü** Doğru olduğunda geçerlidir|20000|Ngrams sözlüğünün maksimum boyutu. Girişteki belirteç sayısı bu boyutu aşarsa, çakışmeler oluşabilir|  
|Özellik karmaları için kullanılacak bit sayısı|Tamsayı|[1;31]|Tüm seçenekleri **göster** onay kutusu *seçilmediğinde* ve **ngrams sözlüğü Oluşturma** False olduğunda geçerlidir|12|Özellik karmaları için kullanılacak bit sayısı| 
|LDA'dan önce ngram sözlüğü oluşturma|Boole|Doğru veya Yanlış|Tüm seçenekleri **göster** onay kutusu seçildiğinde geçerlidir|True|LDA'dan önce ngram sözlüğü oluşturur. Model denetimi ve yorumu için yararlıdır|  
|Sözlükte maksimum ngram sayısı|Tamsayı|[1;int. MaxValue]|Tüm seçenekleri **göster** onay kutusu seçildiğinde ve **ngrams'ın yapı sözlüğü** True olduğunda geçerlidir|20000|Sözlüğün maksimum boyutu. Girişteki belirteç sayısı bu boyutu aşarsa, çakışmeler oluşabilir|  
|Karma bit sayısı|Tamsayı|[1;31]|Tüm seçenekleri **göster** onay kutusu seçildiğinde ve **ngrams'ın yapı sözlüğü** False olduğunda geçerlidir|12|Özellik karmaları sırasında kullanılacak bit sayısı|   


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın.   
Modüllere özgü hataların listesi [için, tasarımcının özel durumları ve hata kodlarına](designer-error-codes.md)bakın.
