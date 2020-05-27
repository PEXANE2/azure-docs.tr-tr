---
title: Kelimeyi vector öğesine Dönüştür
titleSuffix: Azure Machine Learning
description: Bir sözlüğü ve ilgili sözcük içeriğini bir metin olarak ayıklayarak ayıklamak için üç adet sunulan Word2Vec modeli kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/19/2020
ms.openlocfilehash: e0e796b75690bcacc6be8ef29b8b490c7faa40af
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83853774"
---
# <a name="convert-word-to-vector"></a>Kelimeyi vector öğesine Dönüştür

Bu makalede, giriş olarak belirttiğiniz metnin yapı 'ları üzerinde çeşitli farklı Word2Vec modellerini (Word2Vec, fasttext, Glove önceden eğitilen model) uygulamak için Azure Machine Learning Designer (Önizleme) içinde **Word 'e dönüştürme** modülünün nasıl kullanılacağı açıklanır.

Bu modül, Gensim kitaplığını kullanır. Gensim hakkında daha fazla bilgi için, algoritmaların öğreticileri ve açıklaması içeren [Resmi Web sitesine](https://radimrehurek.com/gensim/apiref.html) bakın.

### <a name="more-about-convert-word-to-vector"></a>Kelimeyi vektöre dönüştürme hakkında daha fazla bilgi

Genel olarak konuşma, sözcük vektöre veya sözcük vektörleştirmesi, sözcükleri vektör alanına eşlemek için dil modellerini veya tekniklerini kullanan, yani her bir sözcüğü gerçek sayı vektörü olarak temsil eden doğal bir dil işleme işlemidir.

Sözcük ekleme, metin sınıflandırması, yaklaşım Analizi vb. gibi, NLP akış aşağı akış görevleri için ilk giriş olarak kullanılabilir.

Bu modüldeki çeşitli sözcük ekleme teknolojilerinin yanı sıra, iki çevrimiçi eğitim modeli, Word2Vec ve FastText ve önceden eğitilen bir model, Glove-wiki-gigaword-100 dahil olmak üzere, yaygın olarak kullanılan üç yöntemi uyguladık. Çevrimiçi eğitim modelleri giriş verilerinize göre eğitilirken, önceden eğitilen modeller daha büyük bir metin Corpus (örneğin, Viseı, Google News) genellikle yaklaşık 100.000.000.000 sözcük içerir, ardından Word gömme, sözcük vektörleştirme sırasında sabit kalır. Önceden eğitilen Word modelleri, daha az eğitim süresi, daha iyi sözcük vektörü kodlamalı ve gelişmiş genel performans gibi avantajlar sağlar.

+ Word2Vec, basit sinir ağı kullanarak kelime ekleme hakkında bilgi edinmek için en popüler tekniklerin biridir. teorik, bir PDF indirmesi olarak sunulan bu kağıda tartışılır: [vektör alanı, Mikolov, Tomas, et al Içindeki sözcük temsillerinin verimli bir şekilde tahmini](https://arxiv.org/pdf/1301.3781.pdf). Bu modüldeki uygulama, [Word2Vec için gensim kitaplığını](https://radimrehurek.com/gensim/models/word2vec.html)temel alır.

+ FastText teorisi, PDF indirmesi olarak kullanılabilen bu kağıda açıklanmaktadır: [Word vektörlerine, Subword Information, Bojanowski, Piotr, et al Ile zenginleştirme](https://arxiv.org/pdf/1607.04606.pdf). Bu modüldeki uygulama [FastText için gensim kitaplığını](https://radimrehurek.com/gensim/models/fasttext.html)temel alır.

+ Önceden eğitilen model: Glove-wiki-gigaword-100, 5.6 'daki metin Corpus 'ı temel alan, 5.6 B belirteçleri ve 400K sözlüğü, PDF kullanılabilir: [Glove: sözcük gösterimi Için küresel vektörler](https://nlp.stanford.edu/pubs/glove.pdf).

## <a name="how-to-configure-convert-word-to-vector"></a>Sözcüğü vektöre Dönüştür ' ü yapılandırma

Bu modül, metin sütunu içeren bir veri kümesi gerektirir, önceden işlenmiş metin daha iyidir.

1. **Word 'Ü vektör 'e dönüştürme** modülüne işlem hattınızı ekleyin.

2. Modülün girişi olarak bir veya daha fazla metin sütunu içeren bir veri kümesi sağlayın.

3. **Hedef sütun**için yalnızca işlemek üzere metin içeren bir sütun seçin.

    Genel olarak, bu modül metinden bir sözlük oluşturduğundan farklı sütunların içeriği farklı sözlük içeriğine yol açar, bu nedenle modül yalnızca bir hedef sütunu kabul eder.

4. **Word2Vec stratejisi**için, ve seçeneklerinden birini belirleyin `GloVe pretrained English Model` `Gensim Word2Vec` `Gensim FastText` .

5. **Word2Vec stratejisi** veya ise `Gensim Word2Vec` `Gensim FastText` :

    + **Word2Vec eğitim algoritması**. Ve arasından seçim yapın `Skip_gram` `CBOW` . Fark özgün [kağıda](https://arxiv.org/pdf/1301.3781.pdf)eklenmiştir.

        Varsayılan yöntem `Skip_gram` .

    + **Sözcük katıştırma uzunluğu**. Sözcük vektörlerine boyutlılık belirleyin. `size`Gensim içindeki parametreye karşılık gelir.

        Varsayılan embedding_size 100 ' dir.

    + **Bağlam penceresi boyutu**. Tahmin edilen sözcük ve geçerli sözcük arasındaki en fazla mesafeyi belirtin. `window`Gensim içindeki parametreye karşılık gelir.

        Varsayılan pencere boyutu 5 ' tir.

    + **Dönemler sayısı**. Corpus üzerinden dönemler (yineleme) sayısını belirtin. `iter`Gensim içindeki parametreye karşılık gelir.

        Varsayılan dönemler numarası 5 ' tir.

6. **En büyük sözlük boyutu**için, oluşturulan sözlük içindeki en fazla sözcük sayısını belirtin.

    Bundan daha benzersiz sözcükler varsa, sık sık seyrek görünenler 'i ayıklaın.

    Varsayılan sözlük boyutu 10000 ' dir.

7. **Minimum sözcük sayısı**için, modülün bu değerden daha düşük bir sıklık değeri olan tüm sözcükleri yok sayması için en az bir sözcük sayısı girin.

    Varsayılan değer 5 ' tir.

8. İşlem hattını gönderme.

## <a name="examples"></a>Örnekler

Modülün bir çıkışı vardır:

+ **Birlikte bulunan sözlük**: oluşturulan sözlük, her sözcüğün gömülmesi ile birlikte bir sütun kaplar.

### <a name="result-examples"></a>Sonuç örnekleri

**Sözcüğü vektör** modülünün nasıl çalıştığını göstermek için aşağıdaki örnek, Azure Machine Learning (Önizleme) ' de belirtilen önceden işlenmiş Vil. lıb SP 500 veri kümesine varsayılan ayarlarla bu modülü uygular.

#### <a name="source-dataset"></a>Kaynak veri kümesi

Veri kümesi, bir kategori sütunu, Ayrıca, Vikipden alınan tam metni içerir. Bu tabloda yalnızca birkaç temsili örnek gösterilmektedir.

|metin|
|----------|
|NASDAQ 100 bileşen s p 500 bileşen temeli konum şehir Apple kampüs 1 sonsuz döngüsü cadde sonsuz döngüsü Cupertino California Cupertino California location ülke Amerika Birleşik Devletleri...|
|br NASDAQ 100 NASDAQ 100 Component 500 500 br, Charles Geschke br John Warnock location Adobe Systems...|
|s p 500 s p 500 bileşen sektör Oto için sektör Oto, öncül genel Motors Corporation 1908 2009 ardıl...|
|s p 500 s p 500 bileşen sektöründe şirket Conglomerate, şehir Fairfield Connecticut Fairfield Connecticut location ülke ABD alanı...|
|br s p 500 s p 500 bileşen temeli 1903, William s Harley br Arthur Davidson Harley Davidson, Arthur Davidson br Walter Davidson br William bir Davidson konum...|

#### <a name="output-vocabulary-with-embeddings"></a>Katıştırlarla çıkış sözlüğü

Aşağıdaki tabloda, Bu modülün Vil + 500 veri kümesini giriş olarak alan çıktısı yer almaktadır. En soldaki sütun, sözlük, katıştırma vektörünün aynı satırdaki kalan sütunların değerleriyle temsil edildiği anlamına gelir.

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

Bu örnekte, `Gensim Word2Vec` **Word2Vec stratejisi**olarak varsayılan değerini kullandık, **eğitim algoritması** , `Skip-gram` **sözcük katıştırma uzunluğu** 100, bu nedenle sütunları katıştırma 100 ' dir.

## <a name="technical-notes"></a>Teknik notlar

Bu bölüm, sık sorulan soruların ipuçlarını ve yanıtlarını içerir.

+ Çevrimiçi eğitim ve önceden eğitilen model arasındaki fark

    Bu **sözcüğü vektör modülüne Dönüştür modülünde**üç farklı strateji, iki çevrimiçi eğitim modeli ve önceden eğitilen bir model sunuyoruz. Çevrimiçi eğitim modeli, giriş veri kümenizi eğitim verileri olarak kullanır, eğitim sırasında sözlük ve sözcük vektörlerini oluşturur, önceden eğitilen model, Vişe veya Twitter metni gibi çok daha büyük metin corpkiyle eğitilirken, önceden eğitilen model aslında bir (Word, katıştırma) çifti koleksiyonudur.  

    Önceden eğitilen modeli, Word vektörleştirme stratejisi olarak seçilirse, giriş veri kümesinden bir sözlük özetler ve çevrimiçi eğitim olmadan, önceden eğitilen modelden her bir sözcük için ekleme vektörü oluşturur, önceden eğitilen modelin kullanımı eğitim süresini kaydedebilir ve özellikle giriş veri kümesi boyutu nispeten küçük olduğunda daha iyi bir performansa sahiptir.

+ Ekleme boyutu

    Genel olarak, kısa bir yere kadar performans elde etmek için sözcük katıştırma uzunluğu birkaç yüz (örneğin, 100, 200, 300) olarak ayarlanır.  

    Önceden eğitilen modeller için, sözcük katlamaları 'nın uzunluğu düzeltildiğinde, bu uygulamada Glove-wiki-gigaword-100 ' in gömülme boyutu 100 ' dir.


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 

Tasarımcı (Önizleme) modüllerine özgü hataların listesi için bkz. [Machine Learning hata kodları](designer-error-codes.md).
