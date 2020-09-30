---
title: 'Kelimeyi vector öğesine Dönüştür: modül başvurusu'
titleSuffix: Azure Machine Learning
description: Bir sözlüğü ve ilgili sözcük içeriğini bir metin olarak ayıklayarak ayıklamak için üç adet sunulan Word2Vec modeli kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/19/2020
ms.openlocfilehash: 5fad3e4862b0c40c9edd00a5b9d47b245e529396
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91536741"
---
# <a name="convert-word-to-vector-module"></a>Kelimeyi vektör modülüne Dönüştür

Bu makalede, bu görevleri yapmak için Azure Machine Learning tasarımcısında sözcüğü vektör 'e dönüştürme modülünün nasıl kullanılacağı açıklanır:

- Giriş olarak belirttiğiniz metnin yapı 'ları üzerinde çeşitli Word2Vec modellerini (Word2Vec, fasttext, Glove preeğitilen model) uygulayın.
- Sözcük katıştırile bir sözlük oluşturun.

Bu modül, Gensim kitaplığını kullanır. Gensim hakkında daha fazla bilgi için, öğreticiler ve algoritmaların bir açıklamasını içeren [Resmi Web sitesine](https://radimrehurek.com/gensim/apiref.html)bakın.

### <a name="more-about-converting-words-to-vectors"></a>Sözcükleri vektöre dönüştürme hakkında daha fazla bilgi

Sözcüklerin vektörlere dönüştürülmesi veya sözcük vektörleştirmesi, doğal dil işleme (NLP) işlemidir. İşlem, kelimeleri vektör alanına eşlemek için dil modellerini kullanır. Vektör alanı, her bir sözcüğü gerçek sayı vektörü ile temsil eder. Benzer anlamlara benzer temsiller de sağlar.

Metin sınıflandırması ve yaklaşım analizi gibi NLP aşağı akış görevlerinin ilk girişi olarak Word katıştırılmasını kullanın.

Bu modüldeki çeşitli sözcük ekleme teknolojilerinin yanı sıra, yaygın olarak kullanılan üç yöntemi uyguladık. İki, Word2Vec ve FastText, çevrimiçi eğitim modelleridir. Diğeri de önceden eğitilen bir modeldir, Glove-wiki-gigaword-100. 

Çevrimiçi eğitim modelleri, giriş verilerinize göre eğitilir. Önceden eğitilen modeller, genellikle yaklaşık 100.000.000.000 kelimeleri içeren daha büyük bir metin yapı (örneğin, Vikipedi, Google News) üzerinde çevrimdışı olarak eğitilir. Word gömme, sözcük vektörleştirme sırasında sabit kalır. Önceden eğitilen Word modelleri, daha az eğitim süresi, daha iyi sözcük vektörü kodlamalı ve gelişmiş genel performans gibi avantajlar sağlar.

Yöntemler hakkında bazı bilgiler aşağıda verilmiştir:

+ Word2Vec, bir basit sinir ağı kullanarak Word katıştırarak öğrenilen en popüler tekniklerin biridir. Bu, bir PDF indirmesi olarak sunulan bu kağıda tartışılır: vektör alanında [sözcük temsillerini verimli bir şekilde tahmin](https://arxiv.org/pdf/1301.3781.pdf)edin. Bu modüldeki uygulama, [Word2Vec Için Gensim kitaplığını](https://radimrehurek.com/gensim/models/word2vec.html)temel alır.

+ FastText teorisi, PDF indirmesi olarak kullanılabilen bu kağıda açıklanmaktadır: [sözcük vektörlerini alt sözcük bilgileriyle zenginleştirir](https://arxiv.org/pdf/1607.04606.pdf). Bu modüldeki uygulama, [FastText Için Gensim kitaplığını](https://radimrehurek.com/gensim/models/fasttext.html)temel alır.

+ GloVe preeğitilen modeli, Glove-wiki-gigaword-100 ' dir. Bu, 5.600.000.000 belirteçleri ve 400.000 olmayan sözlük sözcüklerini içeren bir Visez metin Corpus ' i temel alan, önceden eğitilen bir vektör koleksiyonudur. PDF indirmesi kullanılabilir: [Glove: sözcük gösterimi Için genel vektörler](https://nlp.stanford.edu/pubs/glove.pdf).

## <a name="how-to-configure-convert-word-to-vector"></a>Sözcüğü vektöre Dönüştür ' ü yapılandırma

Bu modül bir metin sütunu içeren bir veri kümesi gerektiriyor. Önceden işlenmiş metin daha iyidir.

1. **Word 'Ü vektör 'e dönüştürme** modülüne işlem hattınızı ekleyin.

2. Modülün girişi olarak, bir veya daha fazla metin sütunu içeren bir veri kümesi sağlayın.

3. **Hedef sütun**için, işlemek için metin içeren yalnızca bir sütun seçin.

    Bu modül metinden bir sözlük oluşturduğundan, sütunların içeriği farklılık gösterir, bu da farklı sözlük içeriğine yol açar. Modülün yalnızca bir hedef sütunu kabul ettiği bu budur.

4. **Word2Vec stratejisi**Için, **Glove önceden eğitilen İngilizce modeli**, **Gensım Word2Vec**ve **gensim fasttext**seçeneklerinden birini belirleyin.

5. **Word2Vec stratejisi** **gensim Word2Vec** veya **gensim fasttext**ise:

    + **Word2Vec eğitim algoritması**için **Skip_gram** ve **cfii**arasından seçim yapın. Fark [orijinal kağıda (PDF)](https://arxiv.org/pdf/1301.3781.pdf)eklenmiştir.

        Varsayılan yöntem **Skip_gram**.

    + **Sözcük katıştırma uzunluğu**için sözcük vektörlerine ait boyutalyi belirtin. Bu ayar `size` Gensim içindeki parametreye karşılık gelir.

        Varsayılan gömme boyutu 100 ' dir.

    + **Bağlam penceresi boyutu**için tahmin edilen sözcük ve geçerli sözcük arasındaki en fazla mesafeyi belirtin. Bu ayar `window` Gensim içindeki parametreye karşılık gelir.

        Varsayılan pencere boyutu 5 ' tir.

    + **Dönemler sayısı**için, Corpus üzerinden dönemler (yineleme) sayısını belirtin. `iter`Gensim içindeki parametreye karşılık gelir.

        Varsayılan dönem numarası 5 ' tir.

6. **En büyük sözlük boyutu**için, oluşturulan sözlük içindeki en fazla sözcük sayısını belirtin.

    En büyük boyuttan daha benzersiz sözcükler varsa, seyrek olarak bir uyarı ayıklaın.

    Varsayılan sözlük boyutu 10.000 ' dir.

7. **Minimum sözcük sayısı**için en az bir sözcük sayısı girin. Modül, bu değerden daha düşük bir sıklık değeri olan tüm sözcükleri yok sayacaktır.

    Varsayılan değer 5 ' tir.

8. İşlem hattını gönderme.

## <a name="examples"></a>Örnekler

Modülün bir çıkışı vardır:

+ **Katıştırıla sahip sözlük**: oluşturulan sözlüğü, her sözcüğün gömülmesi ile birlikte içerir. Bir boyut bir sütun kaplar.

Aşağıdaki örnek, kelimenin vektör modülünün nasıl çalıştığını gösterir. Varsayılan ayarlarla, ön işlenmiş Vikipedi SP 500 veri kümesine, Word 'Ü vector öğesine Dönüştür ' ü kullanır.

### <a name="source-dataset"></a>Kaynak veri kümesi

Veri kümesi, Vikümesden alınan tam metin ile birlikte bir kategori sütunu içerir. Aşağıdaki tabloda birkaç temsili örnek gösterilmektedir.

|Metin|
|----------|
|NASDAQ 100 bileşen s p 500 bileşen temeli konum şehir Apple kampüs 1 sonsuz döngüsü cadde sonsuz döngüsü Cupertino California Cupertino California location ülke Amerika Birleşik Devletleri...|
|br NASDAQ 100 NASDAQ 100 Component 500 500 br, Charles Geschke br John Warnock location Adobe Systems...|
|s p 500 s p 500 bileşen sektör Oto için sektör Oto, öncül genel Motors Corporation 1908 2009 ardıl...|
|s p 500 s p 500 bileşen sektöründe şirket Conglomerate, şehir Fairfield Connecticut Fairfield Connecticut location ülke ABD alanı...|
|br s p 500 s p 500 bileşen temeli 1903, William s Harley br Arthur Davidson Harley Davidson, Arthur Davidson br Walter Davidson br William bir Davidson konum...|

### <a name="output-vocabulary-with-embeddings"></a>Katıştırlarla çıkış sözlüğü

Aşağıdaki tablo, Bu modülün çıktısını içerir ve Vivsp 500 veri kümesini giriş olarak alır. En soldaki sütun, sözlüğü gösterir. Gömme vektörü, aynı satırdaki kalan sütunların değerleriyle temsil edilir.

|Sözlük|Gömme karartma 0|Gömme 1|Gömme karartma 2|Gömme karartma 3|Gömme 4|Gömme karartma 5|...|Gömme karartma 99|
|-------------|-------------|-------------|-------------|-------------|-------------|-------------|-------------|-------------|
|nasdaq|-0,375865|0,609234|0,812797|-0,002236|0,319071|-0,591986|...|0,364276
|bileşenleri|0,081302|0,40001|0,121803|0,108181|0,043651|-0,091452|...|0,636587
|s|-0,34355|-0,037092|-0,012167|0,151542|0,601019|0,084501|...|0,149419
|p|-0,133407|0,073244|0,170396|0,326706|0,213463|-0,700355|...|0,530901
kuruluşu|-0,166819|0,10883|-0,07933|-0,073753|0,262137|0,045725|...|0,27487
Kurucu|-0,297408|0,493067|0,316709|-0,031651|0,455416|-0,284208|...|0,22798
location|-0,375213|0,461229|0,310698|0,213465|0,200092|0,314288|...|0,14228
city|-0,460828|0,505516|-0,074294|-0,00639|0,116545|0,494368|...|-0,2403
Apple|0,05779|0,672657|0,597267|-0,898889|0,099901|0,11833|...|0,4636
kamp|-0,281835|0,29312|0,106966|-0,031385|0,100777|-0,061452|...|0,05978
FIN|-0,263074|0,245753|0,07058|-0,164666|0,162857|-0,027345|...|-0,0525
loop|-0,391421|0,52366|0,141503|-0,105423|0,084503|-0,018424|...|-0,0521

Bu örnekte, varsayılan **Gensim Word2Vec** for **Word2Vec stratejisini**kullandık ve **eğitim algoritması** **Skip-gram**' dır. **Sözcük katıştırma uzunluğu** 100, bu nedenle sütunları katıştırma 100.

## <a name="technical-notes"></a>Teknik notlar

Bu bölüm, sık sorulan soruların ipuçlarını ve yanıtlarını içerir.

+ Çevrimiçi eğitim ve önceden eğitilen model arasındaki fark:

    Bu sözcüğü vektör modülüne Dönüştür modülünde üç farklı strateji sağladık: iki çevrimiçi eğitim modeli ve bir önceden eğitilen model. Çevrimiçi eğitim modelleri, giriş veri kümenizi eğitim verileri olarak kullanır ve eğitim sırasında sözlük ve sözcük vektörleri oluşturur. Önceden eğitilen model, Vipeus veya Twitter metni gibi çok daha büyük bir metin için zaten eğitilmiş. Önceden eğitilen model aslında bir sözcük/katıştırma çiftleri koleksiyonudur.  

    GloVe önceden eğitilen modeli, giriş veri kümesinden bir sözlük özetler ve önceden eğitilen modelden her sözcük için bir gömme vektörü oluşturur. Çevrimiçi eğitim olmadan, önceden eğitilen bir modelin kullanımı eğitim süresini kaydedebilir. Özellikle, giriş veri kümesi boyutu nispeten küçük olduğunda daha iyi bir performansa sahiptir.

+ Gömme boyutu:

    Genel olarak, sözcük katıştırma uzunluğu birkaç yüz olarak ayarlanır. Örneğin, 100, 200, 300. Küçük bir gömme, küçük bir vektör alanı anlamına gelir ve bu, sözcük ekleme çakışmalarına neden olabilir.  

    Word katıştırlamaları, önceden eğitilen modeller için düzeltilir. Bu örnekte, Glove-wiki-gigaword-100 katıştırma boyutu 100 ' dir.


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 

Tasarımcı modüllerine özgü hataların listesi için bkz. [Machine Learning hata kodları](designer-error-codes.md).
