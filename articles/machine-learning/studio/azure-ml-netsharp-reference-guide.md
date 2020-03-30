---
title: Net# özel sinir ağları
titleSuffix: ML Studio (classic) - Azure
description: Net# sinir ağları belirtimi dili için sözdizimi kılavuzu. Azure Machine Learning Studio'da (klasik) özel sinir ağ modelleri oluşturmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: reference
author: likebupt
ms.author: keli19
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/01/2018
ms.openlocfilehash: c1912e670a9cf1c178b58cefbd33171f15be2483
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218259"
---
# <a name="guide-to-net-neural-network-specification-language-for-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio için Net# sinir ağı belirtimi dili kılavuzu (klasik)

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Net#, Microsoft tarafından geliştirilen ve derin sinir ağları veya rasgele boyutlardaki kıvrımlar gibi karmaşık sinir selağ mimarilerini tanımlamak için kullanılan bir dildir. Görüntü, video veya ses gibi verilerde öğrenmeyi geliştirmek için karmaşık yapıları kullanabilirsiniz.

Bu bağlamlarda Net# mimari belirtimi kullanabilirsiniz:

+ Microsoft Azure Machine Learning Studio'daki tüm sinirsel ağ modülleri (klasik): [Çok Sınıflı Sinir Ağı,](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/multiclass-neural-network) [İki Sınıfnsinir Ağı](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/two-class-neural-network)ve [Nöral Ağ Regresyonu](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/neural-network-regression)
+ Microsoft ML Server'da nöral ağ fonksiyonları: R dili için [NeuralNet](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/neuralnet) ve [rxNeuralNet](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxneuralnet)ve Python için [rx_neural_network.](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/rx-neural-network)


Bu makalede, Net#'i kullanarak özel bir sinir ağı geliştirmek için gereken temel kavramlar ve sözdizimi açıklanmaktadır:

+ Nöral ağ gereksinimleri ve birincil bileşenlerin nasıl tanımlanılı
+ Net# belirtimi dilinin sözdizimi ve anahtar kelimeleri
+ Net kullanılarak oluşturulan özel sinir ağlarıörnekleri #



## <a name="neural-network-basics"></a>Sinir ağı temelleri

Nöral ağ yapısı katmanlar halinde düzenlenmiş düğümlerden ve düğümler arasındaki ağırlıklı bağlantılardan (veya kenarlardan) oluşur. Bağlantılar yönlüve her bağlantının bir kaynak düğümü ve hedef düğümü vardır.

Her eğitilebilir katman (gizli veya çıktı katmanı) bir veya daha fazla **bağlantı demeti**vardır. Bağlantı paketi bir kaynak katmanından ve bu kaynak katmanından gelen bağlantıların belirtiminden oluşur. Belirli bir paketteki tüm bağlantılar kaynak ve hedef katmanlarını paylaşır. Net#'te, bir bağlantı paketi paketin hedef katmanına ait olarak kabul edilir.

Net#, girişlerin gizli katmanlara eşlenme ve çıktılara eşlenme biçimini özelleştirmenize izin veren çeşitli bağlantı demetlerini destekler.

Varsayılan veya standart paket, kaynak katmandaki her düğümün hedef katmandaki her düğüme bağlı olduğu tam bir **pakettir.**

Ayrıca, Net#aşağıdaki dört tür gelişmiş bağlantı paketini destekler:

+ **Filtrelenmiş demetler**. Kaynak katman düğümü ve hedef katman düğümükonumlarını kullanarak bir yüklem tanımlayabilirsiniz. Yüklem Doğru olduğunda düğümler bağlanır.

+ **Kıvrımlı demetler.** Kaynak katmanında düğümlerin küçük mahallelerini tanımlayabilirsiniz. Hedef katmandaki her düğüm, kaynak katmandaki düğümlerin bir mahallesine bağlanır.

+ **Demetleri birleştirme** ve **Yanıt normalleştirme demetleri**. Bunlar, kullanıcının kaynak katmandaki küçük düğüm mahallelerini tanımladığı kıvrımlı demetlere benzer. Fark, bu demetler içinde kenarların ağırlıkları eğitilebilir değildir. Bunun yerine, hedef düğüm değerini belirlemek için kaynak düğüm değerlerine önceden tanımlanmış bir işlev uygulanır.


## <a name="supported-customizations"></a>Desteklenen özelleştirmeler

Azure Machine Learning Studio'da (klasik) oluşturduğunuz sinirsel ağ modellerinin mimarisi Net# kullanılarak kapsamlı bir şekilde özelleştirilebilir. Şunları yapabilirsiniz:

+ Gizli katmanlar oluşturun ve her katmandaki düğüm sayısını denetleyin.
+ Katmanların birbirine nasıl bağlanılmasını belirtin.
+ Convolutions ve ağırlık paylaşım demetleri gibi özel bağlantı yapıları tanımlayın.
+ Farklı etkinleştirme işlevleri belirtin.

Belirtim dili sözdiziminin ayrıntıları için [Yapı Belirtimi'ne](#structure-specifications)bakın.

Simplex'ten karmaşıka kadar bazı yaygın makine öğrenimi görevleri için sinir ağları tanımlama örnekleri için [bkz.](#examples-of-net-usage)

## <a name="general-requirements"></a>Genel gereksinimler

+ Tam olarak bir çıkış katmanı, en az bir giriş katmanı ve sıfır veya daha fazla gizli katman olmalıdır.
+ Her katman, kavramsal olarak rasgele boyutlarda dikdörtgen bir dizi düzenlenmiş düğümleri sabit bir sayı vardır.
+ Giriş katmanlarında ilişkili eğitilmiş parametreler yoktur ve örnek verilerin ağa girdiği noktayı temsil eder.
+ Eğitilebilir katmanlar (gizli ve çıkış katmanları) ağırlıklar ve önyargılar olarak bilinen eğitilmiş parametreleri ilişkilendirdi.
+ Kaynak ve hedef düğümleri ayrı katmanlarda olmalıdır.
+ Bağlantılar asiklik olmalıdır; başka bir deyişle, ilk kaynak düğümüne giden bir bağlantı zinciri olamaz.
+ Çıktı katmanı bağlantı paketinin kaynak katmanı olamaz.

## <a name="structure-specifications"></a>Yapı özellikleri

Bir sinir ağ yapısı belirtimi üç bölümden oluşur: **sabit bildirim**, **katman bildirimi**, bağlantı **bildirimi**. İsteğe bağlı **hisse beyannamesi** bölümü de vardır. Bölümler herhangi bir sırada belirtilebilir.

## <a name="constant-declaration"></a>Sabit bildirim

Sabit bir bildirim isteğe bağlıdır. Nöral ağ tanımının başka bir yerinde kullanılan değerleri tanımlamak için bir araç sağlar. Bildirim deyimi, eşit işaret ve değer ifadesinin ardından bir tanımlayıcıdan oluşur.

Örneğin, aşağıdaki ifade bir sabit `x`tanımlar:

`Const X = 28;`

Aynı anda iki veya daha fazla sabittanımlamak için tanımlayıcı adlarını ve değerlerini ayraçlara biçine girin ve yarım nokta nokta larını kullanarak ayırın. Örnek:

`Const { X = 28; Y = 4; }`

Her atama ifadesinin sağ tarafı bir tamsayı, gerçek bir sayı, bir Boolean değeri (Doğru veya Yanlış) veya matematiksel bir ifade olabilir. Örnek:

`Const { X = 17 * 2; Y = true; }`

## <a name="layer-declaration"></a>Katman bildirimi

Katman bildirimi gereklidir. Bağlantı demetleri ve öznitelikleri de dahil olmak üzere katmanın boyutunu ve kaynağını tanımlar. Bildirim deyimi katmanın adı (giriş, gizli veya çıktı) ile başlar ve ardından katmanın boyutları (pozitif tamsayılardan oluşan bir tuple) gelir. Örnek:

```Net#
input Data auto;
hidden Hidden[5,20] from Data all;
output Result[2] from Hidden all;
```

+ Boyutların çarpımı katmandaki düğüm sayısıdır. Bu örnekte, iki boyut [5,20] vardır, bu da katmanda 100 düğüm olduğu anlamına gelir.
+ Katmanlar, bir istisna dışında herhangi bir sırada bildirilebilir: Birden fazla giriş katmanı tanımlanırsa, bunların beyan edildiği sıra, giriş verilerindeki özelliklerin sırasına uygun olmalıdır.

Katmandaki düğüm sayısının otomatik olarak belirleneceğini belirtmek için `auto` anahtar kelimeyi kullanın. Anahtar `auto` kelimenin katmana bağlı olarak farklı etkileri vardır:

+ Giriş katmanı bildiriminde, düğüm sayısı giriş verilerindeki özellik sayısıdır.
+ Gizli katman bildiriminde, düğüm sayısı, gizli düğüm sayısı için parametre değeri tarafından belirtilen **sayıdır.**
+ Bir çıktı katmanı bildiriminde, düğüm sayısı iki sınıflı sınıflandırma için 2, regresyon için 1 ve çok sınıflı sınıflandırma için çıktı düğümü sayısına eşittir.

Örneğin, aşağıdaki ağ tanımı tüm katmanların boyutunun otomatik olarak belirlenmesini sağlar:

```Net#
input Data auto;
hidden Hidden auto from Data all;
output Result auto from Hidden all;
```

Eğitilebilir bir katman (gizli veya çıktı katmanları) için bir katman bildirimi isteğe bağlı olarak, sınıflandırma modelleri için **sigmoid** ve regresyon modelleri için **doğrusal** olarak varsayılan çıktı işlevini (etkinleştirme işlevi olarak da adlandırılır) içerebilir. Varsayılanı kullansanız bile, netlik için istenirse etkinleştirme işlevini açıkça belirtebilirsiniz.

Aşağıdaki çıktı işlevleri desteklenir:

+ Sigmoid
+ Doğrusal
+ softmax
+ rlinear
+ Meydanı
+ Karekök
+ srlinear
+ Abs
+ Tanh
+ brlinear

Örneğin, aşağıdaki **bildirimde softmax** işlevi kullanır:

`output Result [100] softmax from Hidden all;`

## <a name="connection-declaration"></a>Bağlantı bildirimi

Eğitilebilir katmanı tanımladıktan hemen sonra, tanımladığınız katmanlar arasındaki bağlantıları bildirmeniz gerekir. Bağlantı paketi bildirimi, paketin `from`kaynak katmanının adını ve oluşturulacak bağlantı paketinin türünü takip eden anahtar kelimeyle başlar.

Şu anda, beş tür bağlantı demeti desteklenir:

+ Anahtar kelimeyle gösterilen **tam** paketler`all`
+ **Filtrelenmiş** demetler, anahtar kelime `where`ile gösterilir , bir yüklem ifadesi takip
+ **Convolutional** demetleri, anahtar kelime `convolve`ile gösterilir , kıvrım öznitelikleri takip
+ Anahtar kelimeler **max havuzu** veya ortalama **havuz** ile gösterilen havuz demetleri, **havuz**
+ Anahtar kelime **yanıt normuyla** gösterilen **yanıt normalleştirme** demetleri

## <a name="full-bundles"></a>Tam paketler

Tam bağlantı paketi, kaynak katmandaki her düğümden hedef katmandaki her düğüme bağlantı içerir. Bu varsayılan ağ bağlantı türüdür.

## <a name="filtered-bundles"></a>Filtrelenmiş paketler

Filtreuygulanmış bağlantı paketi belirtimi, c# lambda ifadesi gibi sözdizimsel olarak ifade edilen bir yüklem içerir. Aşağıdaki örnekte, filtre uygulanmış iki paket tanımlanır:

```Net#
input Pixels [10, 20];
hidden ByRow[10, 12] from Pixels where (s,d) => s[0] == d[0];
hidden ByCol[5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;
```

+ Için `ByRow` `s` yüklemde, giriş katmanının düğümlerinin dikdörtgen diziiçine bir dizin temsil eden bir parametre, `d` `ByRow` `Pixels`ve gizli tabaka düğümleri dizi içine bir dizin temsil eden bir parametre, . Her ikisinin `s` `d` türü ve uzunluğu iki tamsayılar bir tuple olduğunu. Conceptually, `s` ranges over all pairs of integers with `0 <= s[0] < 10` and `0 <= s[1] < 20`, and `d` ranges over all pairs of integers, with `0 <= d[0] < 10` and `0 <= d[1] < 12`.

+ Yüklem ifadesinin sağ tarafında bir koşul vardır. Bu örnekte, her `s` değer `d` ve durum True olması gibi için, kaynak katman düğümünden hedef katman düğümüne bir kenar vardır. Bu nedenle, bu filtre ifadesi, paketin s[0] `s` d[0]'a eşit olduğu tüm durumlarda tanımlanan `d` düğüme tanımlanan düğümden bir bağlantı içerdiğini gösterir.

İsteğe bağlı olarak, filtre uygulanmış bir paket için bir ağırlık kümesi belirtebilirsiniz. **Ağırlıklar** özniteliğinin değeri, paket tarafından tanımlanan bağlantı sayısıyla eşleşen bir uzunlukta kayan nokta değerlerinin bir tuple'ı olmalıdır. Varsayılan olarak, ağırlıklar rasgele oluşturulur.

Ağırlık değerleri hedef düğüm dizini tarafından gruplandırılır. Diğer bir deyişle, ilk hedef düğüm K kaynak düğümlerine `K` bağlıysa, **Ağırlıklar** tuple'ının ilk öğeleri kaynak dizini siparişindeki ilk hedef düğümün ağırlıklarıdır. Aynı durum kalan hedef düğümleri için de geçerlidir.

Ağırlıkları doğrudan sabit değerler olarak belirtmek mümkündür. Örneğin, ağırlıkları daha önce öğrendiyseniz, bu sözdizimini kullanarak bunları sabitler olarak belirtebilirsiniz:

`const Weights_1 = [0.0188045055, 0.130500451, ...]`

## <a name="convolutional-bundles"></a>Convolutional demetleri

Eğitim verileri homojen bir yapıya sahipolduğunda, kıvrımlı bağlantılar genellikle verilerin üst düzey özelliklerini öğrenmek için kullanılır. Örneğin, görüntü, ses veya video verilerinde, uzamsal veya zamansal boyutsallık oldukça düzgün olabilir.

Convolutional demetleri boyutları ile kaydırılır dikdörtgen **çekirdekleri** kullanır. Esasen, her çekirdek yerel mahallelerde uygulanan ağırlıklar kümesi tanımlar, **çekirdek uygulamaları**olarak anılacaktır. Her çekirdek uygulaması, kaynak katmandaki bir düğüme karşılık gelir ve bu düğüm **merkezi düğüm**olarak adlandırılır. Bir çekirdeğin ağırlıkları birçok bağlantı arasında paylaşılır. Bir kıvrımlı demet, her çekirdek dikdörtgen ve tüm çekirdek uygulamaları aynı boyuttadır.

Convolutional demetleri aşağıdaki öznitelikleri destekler:

**InputShape,** kaynak katmanının boyutsallığını bu kıvrımlı paketin amaçları için tanımlar. Değer pozitif tamsayılar bir tuple olmalıdır. Tümsörlerin ürünü kaynak katmandaki düğüm sayısına eşit olmalıdır, ancak aksi takdirde, kaynak katmanı için bildirilen boyutlandırmaile eşleşmesi gerekmez. Bu tuple uzunluğu kıvrımlı demet için **arity** değeri olur. Genellikle arity, bir işlevin kaldırabileceği bağımsız değişken veya operand sayısını ifade eder.

Çekirdeklerin şeklini ve konumlarını tanımlamak **için, KernelShape**, **Adım**, **Dolgu**, **LowerPad**ve **UpperPad**özelliklerini kullanın:

+ **KernelShape**: (gerekli) Kıvrımlı demet için her çekirdeğin boyutsallığını tanımlar. Değer, paketin aritesiyle eşit uzunlukta pozitif tamsayılardan oluşan bir tuple olmalıdır. Bu tuple'ın her bileşeni **InputShape'in**karşılık gelen bileşeninden büyük olmamalıdır.

+ **Adım**: (isteğe bağlı) Kıvrımların kayan adım boyutlarını (her boyut için bir adım boyutu) tanımlar, yani merkezi düğümler arasındaki mesafedir. Değer, paketin aritmesi olan bir uzunluğa sahip pozitif tamsayılardan oluşan bir tuple olmalıdır. Bu tuple'ın her **bileşeni, KernelShape'in**karşılık gelen bileşeninden büyük olmamalıdır. Varsayılan değer, tüm bileşenlerin bire eşit olduğu bir tuple'dır.

+ **Paylaşım**: (isteğe bağlı) Kıvrımın her boyutu için ağırlık paylaşımını tanımlar. Değer tek bir Boolean değeri veya paketin arity bir uzunluk ile Boolean değerleri bir tuple olabilir. Tek bir Boolean değeri, belirtilen değere eşit tüm bileşenlerle doğru uzunluğun bir tuple'ı olarak genişletilir. Varsayılan değer, tüm True değerlerinden oluşan bir tuple'dır.

+ **MapCount**: (isteğe bağlı) Kıvrımlı paket için özellik haritalarının sayısını tanımlar. Değer, tek bir pozitif tamsayı veya paketin aritesi olan bir uzunluğa sahip pozitif tamsayılar dan oluşan bir tuple olabilir. Tek bir tamsayı değeri, ilk bileşenler belirtilen değere eşit ve kalan tüm bileşenler bire eşit olan doğru uzunluğun bir tuple'ı olarak genişletilir. Varsayılan değer birdir. Toplam özellik eşlem sayısı, tuple bileşenlerinin ürünüdür. Bileşenler arasında bu toplam sayının çarpanyası, özellik eşleme değerlerinin hedef düğümlerde nasıl gruplandırılmalarını belirler.

+ **Ağırlıklar**: (isteğe bağlı) Paketin başlangıç ağırlıklarını tanımlar. Değer, bu makalede daha sonra tanımlandığı gibi çekirdek başına ağırlık sayısının iki katı olan bir uzunluğa sahip kayan nokta değerlerinin bir tuple'sı olmalıdır. Varsayılan ağırlıklar rasgele oluşturulur.

Dolguyu kontrol eden iki özellik kümesi vardır, özellikleri birbirini dışlayan özelliklerdir:

+ **Dolgu**: (isteğe bağlı) Girişin **varsayılan dolgu şeması**kullanılarak dolguyapılıp eklenmemesi gerektiğini belirler. Değer tek bir Boolean değeri olabilir veya paketin arity bir uzunluk ile Boolean değerleri bir tuple olabilir.

    Tek bir Boolean değeri, belirtilen değere eşit tüm bileşenlerle doğru uzunluğun bir tuple'ı olarak genişletilir.

    Bir boyutun değeri True ise, kaynak bu boyutta ki ek çekirdek uygulamalarını desteklemek için sıfır değerli hücrelerle mantıksal olarak o boyutta yastıklanır, bu boyuttaki ilk ve son çekirdeklerin ana düğümleri o boyuttaki ilk ve son düğümlerdir kaynak katmanındaki boyut. Böylece, her boyuttaki "kukla" düğümlerinin sayısı, yastıklı kaynak `(InputShape[d] - 1) / Stride[d] + 1` katmanına tam olarak çekirdek sığdırmak üzere otomatik olarak belirlenir.

    Bir boyutun değeri False ise, çekirdekler, dışarıda bırakılan her iki taraftaki düğüm sayısının aynı olması için tanımlanır (1'e kadar bir fark). Bu özniteliğin varsayılan değeri, tüm bileşenlerin False'a eşit olduğu bir tuple'dır.

+ **UpperPad** ve **LowerPad**: (isteğe bağlı) Kullanılacak dolgu miktarı üzerinde daha fazla kontrol sağlayın. **Önemli:** Bu öznitelikler, yalnızca yukarıdaki **Dolgu** özelliği ***tanımlanmamışsa*** tanımlanabilir. Değerler, paketin aritesi olan uzunluklara sahip, sonda değerli tuples olmalıdır. Bu öznitelikler belirtildiğinde, giriş katmanının her boyutunun alt ve üst uçlarına "kukla" düğümleri eklenir. Her boyutta alt ve üst uçlara eklenen düğüm sayısı sırasıyla **LowerPad**[i] ve **UpperPad**[i] tarafından belirlenir.

    Çekirdeklerin yalnızca "gerçek" düğümlere karşılık geldiğinden ve "kukla" düğümlere karşılık olmadığından emin olmak için aşağıdaki koşulların yerine getirilmesi gerekir:
  - **LowerPad'in** her bileşeni kesinlikle `KernelShape[d]/2`.
  - **UpperPad'in** her bileşeni `KernelShape[d]/2`.
  - Bu özniteliklerin varsayılan değeri, tüm bileşenlerin 0'a eşit olduğu bir tuple'dır.

    Ayar **Dolgu** = true olarak "gerçek" giriş içinde çekirdeğin "merkezi" tutmak için gerekli olduğu kadar dolgu sağlar. Bu, çıktı boyutunu hesaplamak için matematiği biraz değiştirir. Genellikle, çıkış boyutu *D* `D = (I - K) / S + 1`olarak hesaplanır `I` , giriş boyutu `K` nerede, çekirdek boyutu, `S` adım ve `/` ons bölümü (sıfıra doğru yuvarlak). UpperPad = [1, 1]'i ayarlarsanız, giriş boyutu `I` etkin `D = (29 - 5) / 2 + 1 = 13`bir şekilde 29'dur ve böylece. Ancak, **Ne** zaman Dolgu = `I` doğru, aslında `K - 1`tarafından çarpılır alır ; bu `D = ((28 + 4) - 5) / 2 + 1 = 27 / 2 + 1 = 13 + 1 = 14`nedenle . **UpperPad** ve **LowerPad** için değerleri belirterek, dolgu üzerinde, **Dolgu** = doğru olarak ayarladığınızdan çok daha fazla kontrol elde elabilirsiniz.

Kıvrımlı ağlar ve uygulamaları hakkında daha fazla bilgi için şu makalelere bakın:

+ [http://deeplearning.net/tutorial/lenet.html](http://deeplearning.net/tutorial/lenet.html)
+ [https://research.microsoft.com/pubs/68920/icdar03.pdf](https://research.microsoft.com/pubs/68920/icdar03.pdf)

## <a name="pooling-bundles"></a>Demetleri birleştirme

Bir **havuz demeti** kıvrımlı bağlantıbenzer geometri uygular, ancak hedef düğüm değerini türetmek için kaynak düğüm değerleri için önceden tanımlanmış işlevleri kullanır. Bu nedenle, havuz demetleri hiçbir eğitilebilir durumu (ağırlıkları veya önyargıları) var. Havuzlama demetleri **Paylaşım**, **MapCount**ve **Ağırlıklar**dışında tüm kıvrımlı öznitelikleri destekler.

Genellikle, bitişik havuzlama birimleri tarafından özetlenen çekirdekler çakışmaz. Eğer Stride[d] her boyutta KinelShape[d] ile eşitse, elde edilen katman genellikle kıvrımlı sinir ağlarında kullanılan geleneksel yerel havuzlama katmanıdır. Her hedef düğüm, çekirdeklerinin faaliyetlerinin maksimumunu veya ortalamasını kaynak katmanında hesaplar.

Aşağıdaki örnekte bir havuz demeti gösterilebilir:

```Net#
hidden P1 [5, 12, 12]
  from C1 max pool {
  InputShape  = [ 5, 24, 24];
   KernelShape = [ 1,  2,  2];
   Stride      = [ 1,  2,  2];
  }
```

+ Paketin arity 3: yani, tuples `InputShape`uzunluğu , `KernelShape`, ve `Stride`.
+ Kaynak katmandaki düğüm `5 * 24 * 24 = 2880`sayısı.
+ Bu geleneksel bir yerel havuzlama katmanıdır, çünkü **KernelShape** ve **Stride** eşittir.
+ Hedef katmandaki düğüm `5 * 12 * 12 = 1440`sayısı.

Katmanları birleştirme hakkında daha fazla bilgi için şu makalelere bakın:

+ [https://www.cs.toronto.edu/~hinton/absps/imagenet.pdf](https://www.cs.toronto.edu/~hinton/absps/imagenet.pdf)(Bölüm 3.4)
+ [https://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf](https://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf)
+ [https://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf](https://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf)

## <a name="response-normalization-bundles"></a>Yanıt normalleştirme demetleri

**Yanıt normalleştirme** ilk Geoffrey Hinton, ve ark tarafından geliştirilen bir yerel normalleştirme düzeni, ve ark, derin [Convolutional Nöral Ağları ile](https://www.cs.toronto.edu/~hinton/absps/imagenet.pdf)kağıt ImageNet Sınıflandırma .

Yanıt normalizasyonu nöral ağlarda genelleme yardımcı olmak için kullanılır. Bir nöron çok yüksek aktivasyon düzeyinde ateş olduğunda, yerel bir tepki normalleştirme katmanı çevreleyen nöronların aktivasyon düzeyini bastırır. Bu üç parametre`α`(, `β`ve `k`) ve bir kıvrımlı yapı (veya mahalle şekli) kullanılarak yapılır. Y hedef **katmanındaki** her nöron kaynak katmanındaki bir nöron **x'e** karşılık gelir. **Y** aktivasyon düzeyi aşağıdaki formül ile `f` verilir, nerede bir nöronun `Nx` aktivasyon düzeyi, ve çekirdek (veya **x**mahallede nöronlar içeren set), aşağıdaki kıvrımlı yapı ile tanımlanan:

![kıvrımlı yapı formülü](./media/azure-ml-netsharp-reference-guide/formula_large.png)

Yanıt normalleştirme demetleri **Paylaşım**, **MapCount**ve **Ağırlıklar**hariç tüm kıvrımlı öznitelikleri destekler.

+ Çekirdek ***x***ile aynı haritada nöronlar içeriyorsa, normalleştirme şeması **aynı harita normalleştirme**olarak adlandırılır. Aynı eşnormalleştirmeyi tanımlamak **için, InputShape'deki** ilk koordinatın 1 değerine sahip olması gerekir.

+ Çekirdek ***x***ile aynı mekansal pozisyonda nöronlar içeriyorsa , ancak nöronlar diğer haritalarda ise, normalleştirme şeması **haritalar normalleştirme arasında**denir. Yanıt normalleştirme Bu tür farklı haritalarda hesaplanan nöron çıkışları arasında büyük aktivasyon düzeyleri için rekabet yaratarak, gerçek nöronlarda bulunan tip esinlenerek lateral inhibisyon bir form uygular. Haritalar normalleştirme arasında tanımlamak için, ilk koordinat bir den büyük ve eşlem sayısından büyük bir veya diğer koordinatların değeri 1 olmalıdır.

Yanıt normalleştirme demetleri, hedef düğüm değerini belirlemek için kaynak düğüm değerlerine önceden tanımlanmış bir işlev uyguladığından, eğitilebilir durumları (ağırlıklar veya önyargılar) yoktur.

> [!NOTE]
> Hedef katmanındaki düğümler çekirdeklerin merkezi düğümleri olan nöronlara karşılık gelir. Örneğin, tek `KernelShape[d]` ise, `KernelShape[d]/2` merkezi çekirdek düğümüne karşılık gelir. Çift `KernelShape[d]` ise, merkezi düğüm . `KernelShape[d]/2 - 1` Bu nedenle, False `Padding[d]` ise, ilk `KernelShape[d]/2` ve son düğümleri hedef katmanında karşılık gelen düğümleri yok. Bu durumu önlemek **için, Dolgu'u** [doğru, doğru, ..., doğru] olarak tanımlayın.

Daha önce açıklanan dört özniteme ek olarak, yanıt normalleştirme demetleri de aşağıdaki öznitelikleri destekler:

+ **Alfa**: (gerekli) Önceki `α` formülde karşılık gelen kayan nokta değerini belirtir.
+ **Beta**: (gerekli) Önceki `β` formülde karşılık gelen kayan nokta değerini belirtir.
+ **Ofset**: (isteğe bağlı) Önceki `k` formülde karşılık gelen kayan nokta değerini belirtir. Varsayılan olarak 1'dir.

Aşağıdaki örnekte, bu öznitelikleri kullanarak bir yanıt normalleştirme paketi tanımlanır:

```Net#
hidden RN1 [5, 10, 10]
from P1 response norm {
  InputShape  = [ 5, 12, 12];
  KernelShape = [ 1,  3,  3];
  Alpha = 0.001;
  Beta = 0.75;
  }
```

+ Kaynak katmanı, her biri 12x12 boyutunda, toplam 1440 düğümde olmak üzere beş harita içerir.
+ **KernelShape** değeri, bu komşuluk 3x3 dikdörtgen olduğu aynı harita normalleştirme katmanı olduğunu gösterir.
+ **Dolgu** varsayılan değeri False olduğundan, hedef katmanı her boyutta yalnızca 10 düğüm vardır. Kaynak katmandaki her düğüme karşılık gelen hedef katmanına bir düğüm eklemek için Dolgu = [doğru, doğru, doğru] ekleyin; ve RN1'in boyutunu [5, 12, 12] olarak değiştirin.

## <a name="share-declaration"></a>Hisse bildirimi

Net# isteğe bağlı olarak paylaşılan ağırlıklara sahip birden çok paket tanımlamayı destekler. Yapıları aynıysa, herhangi iki paketin ağırlıkları paylaşılabilir. Aşağıdaki sözdizimi paylaşılan ağırlıklara sahip demetleri tanımlar:

```Net#
share-declaration:
  share    {    layer-list    }
  share    {    bundle-list    }
  share    {    bias-list    }

  layer-list:
    layer-name    ,    layer-name
    layer-list    ,    layer-name

  bundle-list:
    bundle-spec    ,    bundle-spec
    bundle-list    ,    bundle-spec

  bundle-spec:
    layer-name    =>     layer-name

  bias-list:
    bias-spec    ,    bias-spec
    bias-list    ,    bias-spec

  bias-spec:
    1    =>    layer-name

  layer-name:
    identifier
```

Örneğin, aşağıdaki paylaşım bildirimi katman adlarını belirtir ve hem ağırlıkların hem de önyargıların paylaşılması gerektiğini belirtir:

```Net#
Const {
  InputSize = 37;
  HiddenSize = 50;
  }
input {
  Data1 [InputSize];
  Data2 [InputSize];
  }
hidden {
  H1 [HiddenSize] from Data1 all;
  H2 [HiddenSize] from Data2 all;
  }
output Result [2] {
  from H1 all;
  from H2 all;
  }
share { H1, H2 } // share both weights and biases
```

+ Giriş özellikleri eşit büyüklükteki iki giriş katmanına bölünür.
+ Gizli katmanlar daha sonra iki giriş katmanı üzerinde daha yüksek düzey özellikleri hesaplamak.
+ Hisse senedi beyannamesi, *H1* ve *H2'nin* ilgili girdilerinden aynı şekilde hesaplanması gerektiğini belirtir.

Alternatif olarak, bu aşağıdaki gibi iki ayrı pay beyannameleri ile belirtilebilir:

```Net#
share { Data1 => H1, Data2 => H2 } // share weights
<!-- -->
    share { 1 => H1, 1 => H2 } // share biases
```

Kısa formu yalnızca katmanlar tek bir paket içerdiğinde kullanabilirsiniz. Genel olarak, paylaşım yalnızca ilgili yapı aynı olduğunda mümkündür, yani aynı boyuta, aynı kıvrımlı geometriye ve benzerlerine sahipler.

## <a name="examples-of-net-usage"></a>Net# kullanım örnekleri

Bu bölümde, gizli katmanlar eklemek, gizli katmanların diğer katmanlarla etkileşim şeklini tanımlamak ve karmaşık ağlar oluşturmak için Net#'i nasıl kullanabileceğinize dair bazı örnekler verilmektedir.

### <a name="define-a-simple-custom-neural-network-hello-world-example"></a>Basit bir özel sinir ağı tanımlayın: "Hello World" örneği

Bu basit örnek, tek bir gizli katmanı olan bir nöral ağ modelinin nasıl oluşturulabildiğini gösterir.

```Net#
input Data auto;
hidden H [200] from Data all;
output Out [10] sigmoid from H all;
```

Örnek, bazı temel komutları aşağıdaki gibi göstermektedir:

+ İlk satır giriş katmanını tanımlar `Data`(adlandırılmış). Anahtar kelimeyi `auto` kullandığınızda, nöral ağ giriş örneklerindeki tüm özellik sütunlarını otomatik olarak içerir.
+ İkinci satır gizli katmanı oluşturur. Ad, `H` 200 düğümiçeren gizli katmana atanır. Bu katman giriş katmanına tamamen bağlıdır.
+ Üçüncü satır, 10 çıkış `Out`düğümü içeren çıkış katmanını (adlandırılmış) tanımlar. Nöral ağ sınıflandırma için kullanılırsa, sınıf başına bir çıkış düğümü vardır. **Sigmoid** anahtar kelimesi, çıktı işlevinin çıktı katmanına uygulandığını gösterir.

### <a name="define-multiple-hidden-layers-computer-vision-example"></a>Birden çok gizli katman tanımlayın: bilgisayar görme örneği

Aşağıdaki örnek, birden çok özel gizli katmanları ile biraz daha karmaşık bir sinir ağı tanımlamak için nasıl gösterir.

```Net#
// Define the input layers
input Pixels [10, 20];
input MetaData [7];

// Define the first two hidden layers, using data only from the Pixels input
hidden ByRow [10, 12] from Pixels where (s,d) => s[0] == d[0];
hidden ByCol [5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;

// Define the third hidden layer, which uses as source the hidden layers ByRow and ByCol
hidden Gather [100]
{
from ByRow all;
from ByCol all;
}

// Define the output layer and its sources
output Result [10]
{
from Gather all;
from MetaData all;
}
```

Bu örnek, sinir ağları belirtimi dilinin çeşitli özelliklerini göstermektedir:

+ Yapının iki giriş katmanı `Pixels` `MetaData`vardır ve .
+ Katman, `Pixels` hedef katmanları olan iki bağlantı demeti `ByRow` için `ByCol`bir kaynak katmanıdır ve.
+ Katmanlar `Gather` `Result` ve birden çok bağlantı demetleri hedef katmanları vardır.
+ Çıktı katmanı, `Result`iki bağlantı demeti içinde bir hedef katmandır; biri hedef katman olarak `Gather` ikinci düzey gizli katmanı, diğeri ise `MetaData` hedef katman olarak giriş katmanı ile.
+ Gizli katmanları `ByRow` ve `ByCol`, yüklem ifadeleri kullanarak filtrelenmiş bağlantı belirtin. Daha doğrusu, [x, y] deki `ByRow` düğüm, düğümün ilk `Pixels` koordinatı olan ilk dizin koordinatına sahip düğümlere bağlanır, x. Benzer şekilde, [x, y] deki `ByCol` düğüm, düğümün ikinci `Pixels` koordinatı olan ikinci dizin koordinatına sahip düğümlere bağlanır, y.

### <a name="define-a-convolutional-network-for-multiclass-classification-digit-recognition-example"></a>Çok sınıflı sınıflandırma için bir kıvrımlı ağ tanımlayın: basamak tanıma örneği

Aşağıdaki ağın tanımı sayıları tanımak için tasarlanmıştır ve bir sinir ağı özelleştirmek için bazı gelişmiş teknikleri göstermektedir.

```Net#
input Image [29, 29];
hidden Conv1 [5, 13, 13] from Image convolve
  {
  InputShape  = [29, 29];
  KernelShape = [ 5,  5];
  Stride      = [ 2,  2];
  MapCount    = 5;
  }
hidden Conv2 [50, 5, 5]
from Conv1 convolve
  {
  InputShape  = [ 5, 13, 13];
  KernelShape = [ 1,  5,  5];
  Stride      = [ 1,  2,  2];
  Sharing     = [false, true, true];
  MapCount    = 10;
  }
hidden Hid3 [100] from Conv2 all;
output Digit [10] from Hid3 all;
```

+ Yapının tek bir giriş `Image`katmanı vardır.
+ Anahtar kelime, `convolve` adlandırılmış `Conv1` katmanların ve `Conv2` karmaşık katmanların olduğunu gösterir. Bu katman bildirimleri her biri kıvrım öznitelikleri bir listesini takip eder.
+ Net üçüncü bir gizli `Hid3`tabaka vardır, tamamen ikinci gizli `Conv2`katmana bağlı, .
+ Çıkış katmanı, `Digit`yalnızca üçüncü gizli katmana `Hid3`bağlanır. Anahtar kelime, `all` çıktı katmanının tamamen `Hid3`.
+ Kıvrımarlık üçtür: `InputShape`tuples uzunluğu , `KernelShape`, `Stride`, ve `Sharing`.
+ Çekirdek başına ağırlık sayısı. `1 + KernelShape\[0] * KernelShape\[1] * KernelShape\[2] = 1 + 1 * 5 * 5 = 26` Ya `26 * 50 = 1300`da .
+ Her gizli katmandaki düğümleri aşağıdaki gibi hesaplayabilirsiniz:

    `NodeCount\[0] = (5 - 1) / 1 + 1 = 5` `NodeCount\[1] = (13 - 5) / 2 + 1 = 5`
    `NodeCount\[2] = (13 - 5) / 2 + 1 = 5`

+ Toplam düğüm sayısı, katmanın beyan edilen boyutlandırması kullanılarak hesaplanabilir, [50, 5, 5], aşağıdaki gibi:`MapCount * NodeCount\[0] * NodeCount\[1] * NodeCount\[2] = 10 * 5 * 5 * 5`
+ Çünkü `Sharing[d]` false yalnızca `d == 0`için , çekirdek lerin `MapCount * NodeCount\[0] = 10 * 5 = 50`sayısı .

## <a name="acknowledgements"></a>Bildirimler

Sinir ağlarının mimarisini özelleştirmek için Net# dili Microsoft'ta Shon Katzenberger (Mimar, Machine Learning) ve Alexey Kamenev (Software Engineer, Microsoft Research) tarafından geliştirilmiştir. Görüntü algılamadan metin analizine kadar makine öğrenimi projeleri ve uygulamaları için dahili olarak kullanılır. Daha fazla bilgi için [Azure Machine Learning stüdyosunda Nöral Ağlar - Net'e Giriş #](https://blogs.technet.com/b/machinelearning/archive/2015/02/16/neural-nets-in-azure-ml-introduction-to-net.aspx)
