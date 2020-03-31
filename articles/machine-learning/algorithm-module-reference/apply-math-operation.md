---
title: Matematik İşlemi Uygulama
titleSuffix: Azure Machine Learning
description: Bir ardışık işlemdeki sütun değerlerine matematiksel bir işlem uygulamak için Azure Machine Learning'de Matematik İşlemuygula modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 09/09/2019
ms.openlocfilehash: 2b4d6939aa1db4b8321c792898ed421c0d16cc99
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79456752"
---
# <a name="apply-math-operation"></a>Matematik İşlemi Uygulama

Bu makalede, Azure Machine Learning tasarımcısının (önizleme) bir modülü açıklanmaktadır.

Giriş veri kümesindeki sayısal sütunlara uygulanan hesaplamalar oluşturmak için Matematik Uygula İşlemini kullanın. 

Desteklenen matematik işlemleri, çarpma ve bölme, trigonometrik fonksiyonlar, çeşitli yuvarlama işlevleri ve gama ve hata işlevleri gibi veri bilimlerinde kullanılan özel işlevler gibi yaygın aritmetik işlevleri içerir.  

Bir işlemi tanımladıktan ve ardışık hattı çalıştırdıktan sonra, değerler veri kümenize eklenir. Modülü nasıl yapılandırdığınıza bağlı olarak şunları yapabilirsiniz:

+ Sonuçları veri setinize ekle. Bu, özellikle işlemin sonucunu doğrularken kullanışlıdır.
+ Sütun değerlerini yeni, hesaplanmış değerlerle değiştirin.
+ Sonuçlar için yeni bir sütun oluşturun ve özgün verileri göstermeyin. 

Bu kategorilerde ihtiyacınız olan işlemi arayın:  

- [Temel](#basic-math-operations)  
  
     **Temel** kategorisindeki işlevler, tek bir değeri veya değer sütununu işlemek için kullanılabilir. Örneğin, bir sütundaki tüm sayıların mutlak değerini alabilir veya bir sütundaki her değerin kare kökünü hesaplayabilirsiniz.  
  
-   [Karşılaştır](#comparison-operations)  
  
      **Karşılaştırma** kategorisindeki işlevlerin tümü karşılaştırma için kullanılır: iki sütundaki değerlerin çift yönlü karşılaştırmasını yapabilir veya sütundaki her değeri belirli bir sabitle karşılaştırabilirsiniz. Örneğin, iki veri kümesinde değerlerin aynı olup olmadığını belirlemek için sütunları karşılaştırabilirsiniz. Veya, sayısal bir sütundaki aykırılıkları bulmak için izin verilen en büyük değer gibi bir sabit kullanabilirsiniz.  
  
-   [İşlemler](#arithmetic-operations)  
  
     **İşlemler** kategorisi temel matematiksel işlevleri içerir: toplama, çıkarma, çarpma ve bölme. Sütunlar veya sabitlerle çalışabilirsiniz. Örneğin, A sütunundaki değeri B Sütunu'ndaki değere ekleyebilirsiniz. Veya, A sütunundaki her değerden önceden hesaplanmış bir ortalama gibi bir sabit çıkarabilirsiniz.  
  
-   [Yuvarlama](#rounding-operations)  
  
     **Yuvarlama** kategorisi, yuvarlama, tavan, zemin ve kesilme gibi işlemleri çeşitli hassasiyet düzeylerinde gerçekleştirmek için çeşitli işlevler içerir. Hem ondalık hem de tam sayılar için kesinlik düzeyini belirtebilirsiniz.  
  
-   [Özel](#special-math-functions)  
  
     **Özel** kategori, özellikle eliptik integraller ve Gaussian hata fonksiyonu gibi veri bilimlerinde kullanılan matematiksel işlevleri içerir.  
  
-   [Trigonometrik](#trigonometric-functions)  
  
     **Trigonometrik** kategori tüm standart trigonometrik fonksiyonları içerir. Örneğin, radyanları dereceye dönüştürebilir veya radyanlarda veya derecelerde teğet gibi hesaplama işlevlerini hesaplayabilirsiniz.
     Bu işlevler unary, yani giriş olarak tek bir değer sütunu alırlar, trigonometrik işlevi uygularlar ve sonuç olarak bir değer sütunu döndürürler.  Bu nedenle, giriş sütununun uygun tür olduğundan ve belirtilen işlem için doğru türde değerler içerdiğinden emin olmanız gerekir.   

## <a name="how-to-configure-apply-math-operation"></a>Matematik Uygula İşlemi nasıl yapılandırılır?  

**Matematik İşlemuygula** modülü, yalnızca sayılar içeren en az bir sütun içeren bir veri kümesi gerektirir. Sayılar ayrık veya sürekli olabilir, ancak bir dize değil, sayısal veri türünde olmalıdır.

Aynı işlemi birden çok sayısal sütuna uygulayabilirsiniz, ancak tüm sütunların aynı veri kümesinde olması gerekir. 

Bu modülün her örneği aynı anda yalnızca bir tür işlem gerçekleştirebilir. Karmaşık matematik işlemleri gerçekleştirmek için, **Matematik İşlemuygula** modülünün birkaç örneğini birbirine zincirlemeniz gerekebilir.  
  
1.  Ardınıza **Matematik İşlemuygula** modülünüzü ekleyin.

1. En az bir sayısal sütun içeren bir veri kümesibağlayın.  

1.  Hesaplamayı gerçekleştirmek için bir veya daha fazla kaynak sütun seçin.   
  
    - Seçtiğiniz herhangi bir sütun sayısal bir veri türü olmalıdır. 
    - Veri aralığı seçilen matematiksel işlem için geçerli olmalıdır. Aksi takdirde bir hata veya NaN (sayı değil) sonucu oluşabilir. Örneğin, Ln(-1.0) geçersiz bir işlemdir ve .'ın değeri ile `NaN`sonuçlanır.
  
1.  Gerçekleşecek matematik işlemi **nin türünü** seçmek için **Kategori'** yi tıklatın.
    
1. Bu kategorideki listeden belirli bir işlem seçin.
  
1.  Her işlem türü için gereken ek parametreleri ayarlayın.  
  
1.  Matematik işleminin nasıl oluşturulmasını istediğinizi belirtmek için **Çıktı modunu** kullanın: 

    - **Ek.** Giriş olarak kullanılan tüm sütunlar çıktı veri kümesine eklenir, ayrıca matematik işleminin sonuçlarını içeren bir ek sütun eklenir.
    - **Yerin karşısı.** Giriş olarak kullanılan sütunlarda değerler yeni hesaplanan değerlerle değiştirilir. 
    - **Sonuç sadece**. Matematik işleminin sonuçlarını içeren tek bir sütun döndürülür.
  
1.  Boru hattını gönderin.  
  
## <a name="results"></a>Sonuçlar

Sonuçları **Append** veya **ResultOnly** seçeneklerini kullanarak oluşturursanız, döndürülen veri kümesinin sütun başlıkları işlemi ve kullanılan sütunları gösterir. Örneğin, **Eşitler** işleci kullanarak iki sütunu karşılaştırırsanız, sonuçlar aşağıdaki gibi görünür:  
  
-   **Equals(Col2_Col1)**, Col2'yi Col1'e karşı test ettiğinizi gösterir.  
-   **Eşittir (Col2_ $10)**, sütun 2 ile sabit 10 arasında karşılaştırıldığında olduğunu gösterir.  

**Yerinde kullanım** seçeneğini kullansanız bile, kaynak veriler silinmez veya değiştirilmez; özgün veri kümesindeki sütun hala tasarımcıda kullanılabilir. Özgün verileri görüntülemek için Sütun [Ekle](add-columns.md) modülünü bağlayabilir ve **Matematik İşlemuygula'nın**çıktısına katılabilirsiniz.  
    
## <a name="basic-math-operations"></a>Temel matematik işlemleri 

**Temel** kategorisindeki işlevler genellikle bir sütundan tek bir değer alır, önceden tanımlanmış işlemi gerçekleştirir ve tek bir değer döndürer. Bazı işlevler için, ikinci bir bağımsız değişken olarak bir sabit veya sütun kümesi belirtebilirsiniz.  
  
 Azure Machine Learning **Temel** kategorisinde aşağıdaki işlevleri destekler:  

### <a name="abs"></a>Abs

Seçili sütunların mutlak değerini verir.  
  
### <a name="atan2"></a>Atan2

Dört dörtlük ters teğet verir.  

Nokta koordinatlarını içeren sütunları seçin. X-koordinatına karşılık gelen ikinci bağımsız değişken için bir sabit de belirtebilirsiniz.  

Matlab'daki ATAN2 işlevine karşılık gelir.  

### <a name="conj"></a>Conj

Seçili sütundaki değerler için eşlekap'ı döndürür.  

### <a name="cuberoot"></a>Küp Kökü

Seçili sütundaki değerler için küp kökünü hesaplar.  

### <a name="doublefactorial"></a>DoubleFactorial  
 Seçili sütundaki değerler için çift faktörlü hesaplar. Çift faktöriyel normal faktöriyel fonksiyonun bir uzantısıdır ve x olarak gösterilir!!.  

### <a name="eps"></a>Eps

Geçerli değer ile sonraki en yüksek, çift duyarlıklı sayı arasındaki boşluğun boyutunu döndürür. Matlab'daki EPS işlevine karşılık gelir.  
  
### <a name="exp"></a>Exp

Seçilen sütundaki değerin gücüne yükseltilen e döndürür. Bu, Excel EXP işleviyle aynıdır.  

### <a name="exp2"></a>Exp2

T'nin üs içeren değerler sütunu olduğu y = x * 2<sup>t</sup> için çözme, bağımsız değişkenlerin taban-2 üstel sini verir.  

**Sütun kümesinde,** t'nin üs değerlerini içeren sütunu seçin.

**Exp2** için, sabit veya başka bir değer sütunu olabilecek ikinci bir bağımsız değişken x belirtebilirsiniz. **İkinci bağımsız değişken türünde,** çarpan ı sabit olarak mı yoksa bir sütundaki değeri mi sağlayacağınızı belirtin.  

Örneğin, hem çarpan hem de {0,1,2,3,4,5} üs için değerleri olan bir sütun seçerseniz, işlev {0, 2, 8, 24, 64 160) döndürür.  

### <a name="expminus1"></a>ExpEksi1 

Seçili sütundaki değerler için negatif üs döndürür.  

### <a name="factorial"></a>Faktöriyel
Seçili sütundaki değerler için faktöriyel verir.  

### <a name="hypotenuse"></a>Hipotenüs
Bir tarafın uzunluğunun değerler sütunu olarak belirtildiği bir üçgen için hipotenüs hesaplar ve ikinci tarafın uzunluğu sabit veya iki sütun olarak belirtilir.  

### <a name="ln"></a>Ln

Seçili sütundaki değerler için doğal logaritma verir.  

### <a name="lnplus1"></a>LnPlus1

Seçili sütundaki değerler için doğal logaritma artı bir verir.  

### <a name="log"></a>Günlük

Belirtilen taban göz önüne alındığında, seçili sütundaki değerlerin günlüğünü verir.  

Temeli (ikinci bağımsız değişken) sabit olarak veya başka bir değer sütunu seçerek belirtebilirsiniz.  

### <a name="log10"></a>Log10

Seçili sütun için temel 10 logarithm değerlerini döndürür.  

### <a name="log2"></a>Günlük2

Seçili sütun için temel 2 logaitm değerlerini döndürür.  

### <a name="nthroot"></a>NthRoot
Belirttiğiniz bir n kullanarak değerin nth kökünü döndürür.  

**ColumnSet** seçeneğini kullanarak kökünü hesaplamak istediğiniz sütunları seçin.  

**İkinci bağımsız değişken türünde,** kökü içeren başka bir sütun seçin veya kök olarak kullanılacak bir sabit belirtin.  

İkinci bağımsız değişken bir sütunsa, sütundaki her değer karşılık gelen satır için n değeri olarak kullanılır. İkinci bağımsız değişken sabitse, İkinci bağımsız **değişken** metin kutusuna n değerini yazın.
### <a name="pow"></a>Pow

Seçili sütundaki değerlerin her biri için Y'nin gücüne yükseltilen X'i hesaplar.  

İlk olarak, **ColumnSet** seçeneğini kullanarak, float olması gereken **tabanı**içeren sütunları seçin.  

**İkinci bağımsız değişken türünde,** üs içeren sütunu seçin veya üs olarak kullanılacak bir sabit belirtin.  

İkinci bağımsız değişken bir sütunsa, sütundaki her değer karşılık gelen satırın üsolarak kullanılır. İkinci bağımsız değişken sabitse, **İkinci bağımsız değişken** metin kutusuna üs için değeri yazın.  

### <a name="sqrt"></a>Sqrt

Seçili sütundaki değerlerin kare kökünü döndürür.  

### <a name="sqrtpi"></a>SqrtPi

Seçili sütundaki her değer için değeri pi ile çarpar ve sonucun kare kökünü döndürür.  

### <a name="square"></a>Square

Seçili sütundaki değerleri kareler.  

## <a name="comparison-operations"></a>Karşılaştırma işlemleri  

İki değer kümesini birbirinibire karşı sınamanız gereken her zaman Azure Machine Learning tasarımcısındaki karşılaştırma işlevlerini kullanın. Örneğin, bir ardışık alanda şu karşılaştırma işlemlerini yapmanız gerekebilir:  

- Olasılık puanları modeli sütununu eşik değere göre değerlendirin.
- İki sonuç kümesinin aynı olup olmadığını belirleyin. Farklı olan her satır için, daha fazla işleme veya filtreleme için kullanılabilecek bir FALSE bayrağı ekleyin.  

### <a name="equalto"></a>Equalto

Değerler aynıysa True döndürür.  

### <a name="greaterthan"></a>GreaterThan

**Sütun kümesindeki** değerler belirtilen sabitten büyükse veya karşılaştırma sütunundaki karşılık gelen değerlerden büyükse True döndürür.  

### <a name="greaterthanorequalto"></a>Greaterthanorequalto

**Sütun kümesindeki** değerler belirtilen sabitten büyük veya eşitse veya karşılaştırma sütunundaki karşılık gelen değerlerden daha büyük veya eşitse True döndürür.  

### <a name="lessthan"></a>LessThan

**Sütun kümesindeki** değerler belirtilen sabitten küçükse veya karşılaştırma sütunundaki karşılık gelen değerlerden daha azsa True döndürür.  
  
### <a name="lessthanorequalto"></a>lessthanorequalto

**Sütun kümesindeki** değerler belirtilen sabitten küçük veya eşitse veya karşılaştırma sütunundaki karşılık gelen değerlerden daha az veya eşitse True döndürür.  

### <a name="notequalto"></a>NotEqualto

**Sütun kümesindeki** değerler sabit veya karşılaştırma sütununa eşit değilse True döndürür ve eşitse False döndürür.  

### <a name="pairmax"></a>PairMax

**Sütun kümesindeki** değeri veya sabit veya karşılaştırma sütunundaki değeri daha büyük olan değeri döndürür.  

### <a name="pairmin"></a>PairMin

Daha küçük olan değeri **verir—Sütun kümesindeki** değer veya sabit veya karşılaştırma sütunundaki değer  
  
##  <a name="arithmetic-operations"></a>Aritmetik işlemler   

Temel aritmetik işlemleri içerir: toplama ve çıkarma, bölme ve çarpma.  İşlemlerin çoğu ikili olduğundan, iki sayı gerektirdiğinden, önce işlemi seçin ve sonra birinci ve ikinci bağımsız değişkenlerde kullanılacak sütun veya sayıları seçin.

Bölme ve çıkarma için sütunları seçtiğiniz sıra mantıksız görünebilir; ancak, sonuçları daha kolay anlamak için, sütun başlığı işlem adını ve sütunların kullanıldığı sırayı sağlar.

İşlem|Sayısal Olarak 1|Sayısal Olarak 2|Sonuç sütunu|Sonuç değeri|
----|----|----|----|----
|Toplama|1|5|Ekle(Num2_Num1)| 4|
|Çarpma|1|5|Çoklu(Num2_Num1)|5|
|Çıkarma|1|5|Çıkarma(Num2_Num1)|4|
|Çıkarma|0|1|Çıkarma(Num2_Num1)|0|
|Bölme|1|5|Bölme(Num2_Num1)|5|
|Bölme|0|1|Bölme(Num2_Num1)|Sonsuz|

### <a name="add"></a>Ekle

**Sütun kümesini**kullanarak kaynak sütunları belirtin ve ardından bu değerlere **İkinci bağımsız değişkende**belirtilen bir sayı ekleyin.  

İki sütundaki değerleri eklemek için **Sütun kümesini**kullanarak bir sütun veya sütun seçin ve **ardından İkinci bağımsız değişkeni**kullanarak ikinci bir sütun seçin.  

### <a name="divide"></a>Böl

**Sütun kümesindeki** değerleri bir sabit veya İkinci bağımsız **değişkende**tanımlanan sütun değerleriyle böler.  Başka bir deyişle, önce böleni, sonra da kar payını seçersin. Çıktı değeri bölüm değeridir.

### <a name="multiply"></a>Çarp

Sütunda belirtilen sabit veya sütun değerleri **ile ayarlanan** değerleri çarpar.  

### <a name="subtract"></a>Çıkar

**Sütun kümesi** seçeneğini kullanarak farklı bir sütun seçerek, üzerinde çalışacak değerler sütunu *(minuend)* belirtin. Ardından, **İkinci bağımsız değişken** açılır listesini kullanarak çıkarı *(subtrahend)* numarasını belirtin. Sabit veya değerler sütunu seçebilirsiniz.

##  <a name="rounding-operations"></a>Yuvarlama işlemleri 

Azure Machine Learning tasarımcısı çeşitli yuvarlama işlemlerini destekler. Birçok işlem için, yuvarlama yaparken kullanılacak hassasiyet miktarını belirtmeniz gerekir. Sabit olarak belirtilen statik bir duyarlık düzeyi kullanabilir veya bir değer sütunundan elde edilen dinamik bir duyarlık değeri uygulayabilirsiniz.  

- Sabit kullanıyorsanız, **Precision Type'ı** **Sabit** olarak ayarlayın ve ardından basamak sayısını **Sabit Kesinmetin** metin kutusuna tamsayı olarak yazın. Tamsayı olmayan bir yazar yazarsanız, modül hata yapmaz, ancak sonuçlar beklenmeyen olabilir.  

- Veri setinizdeki her satır için farklı bir duyarlık değeri kullanmak için **Precision Type'ı** **ColumnSet'e**ayarlayın ve ardından uygun hassas değerleri içeren sütunu seçin.  

### <a name="ceiling"></a>Üst sınır

**Sütun kümesindeki**değerlerin tavanını döndürür.  

### <a name="ceilingpower2"></a>TavanGücü2

Sütun kümesindeki değerler için kareli tavanı **döndürür.**  

### <a name="floor"></a>Floor

**Sütun kümesindeki**değerlerin zeminini belirtilen duyarlıkiçin döndürür.  

### <a name="mod"></a>Mod

**Sütun kümesindeki**değerlerin kesirli kısmını belirtilen duyarlıka döndürür.  

### <a name="quotient"></a>Üs

**Sütun kümesindeki**değerlerin kesirli kısmını belirtilen duyarlıka döndürür.  

### <a name="remainder"></a>Kalan

**Sütun kümesindeki**değerler için kalan değeri verir.  

### <a name="rounddigits"></a>Yuvarlak Basamaklar

4/5 kuralıyla yuvarlatılmış **Sütun kümesindeki**değerleri belirtilen basamak sayısına döndürür.  

### <a name="rounddown"></a>RoundDown

**Sütun kümesindeki**değerleri , belirtilen basamak sayısına yuvarlatılır.  

### <a name="roundup"></a>RoundUp

**Sütun kümesindeki**değerleri, belirtilen basamak sayısına yuvarlanır.  

### <a name="toeven"></a>Toeven

**Sütun kümesindeki**değerleri , en yakın tam, çift sayıya yuvarlanır.  

### <a name="toodd"></a>ToOdd

**Sütun kümesindeki**değerleri , yuvarlatılmış en yakın tam, tek sayıya döndürür.  

### <a name="truncate"></a>Truncate

Belirtilen kesinlik tarafından izin verilmeyen tüm basamakları kaldırarak **Sütun kümesindeki** değerleri kırar.  
  
## <a name="special-math-functions"></a>Özel matematik fonksiyonları

Bu kategori, genellikle veri biliminde kullanılan özel matematiksel işlevleri içerir. Aksi belirtilmedikçe, işlev unary ve seçili sütun veya sütunlarda her değer için belirtilen hesaplama döndürür.  

### <a name="beta"></a>Beta

Euler'in beta işlevinin değerini verir.  

### <a name="ellipticintegrale"></a>EliptikIntegralE
Tamamlanmamış eliptik integralin değerini verir.  
  

### <a name="ellipticintegralk"></a>EliptikIntegralK

Tam eliptik integralin (K) değerini verir.  

### <a name="erf"></a>Erf

Hata işlevinin değerini verir.  

Hata işlevi (Gauss hata işlevi olarak da adlandırılır) difüzyonu tanımlamak için olasılık olarak kullanılan sigmoid şeklin özel bir fonksiyonudur.  

### <a name="erfc"></a>Erfc

Tamamlayıcı hata işlevinin değerini verir.  

Erfc 1 – erf(x) olarak tanımlanır.  

### <a name="erfscaled"></a>ErfÖlçekli

Ölçeklenen hata işlevinin değerini verir.  

Hata işlevinin ölçeklenmiş sürümü aritmetik akışı önlemek için kullanılabilir.  

### <a name="erfinverse"></a>ErfInverse

Ters erf işlevinin değerini verir.  

### <a name="exponentialintegralein"></a>Üstel IntegralEin

Üstel integral Ei değerini verir.  

### <a name="gamma"></a>Gama

Gama fonksiyonunun değerini verir.  

### <a name="gammaln"></a>Gamaln

Gama fonksiyonunun doğal logaritmasını verir.  

### <a name="gammaregularizedp"></a>GammaRegularizedP

Düzenli olarak tamamlanmamış gama fonksiyonunun değerini verir.  

Bu işlev, sabit veya değerler sütunu olarak sağlanabilir ikinci bir bağımsız değişken alır.  

### <a name="gammaregularizedpinverse"></a>GammaRegularizedPInverse

Ters düzenli olarak yapılan eksik gama fonksiyonunun değerini verir.  

Bu işlev, sabit veya değerler sütunu olarak sağlanabilir ikinci bir bağımsız değişken alır.  

### <a name="gammaregularizedq"></a>GammaRegularizedQ  

Düzenli olarak tamamlanmamış gama fonksiyonunun değerini verir.  

Bu işlev, sabit veya değerler sütunu olarak sağlanabilir ikinci bir bağımsız değişken alır.  

### <a name="gammaregularizedqinverse"></a>GammaRegularizedQInverse

Ters jeneralize edilmiş normalleştirilmiş eksik gama fonksiyonunun değerini verir.

Bu işlev, sabit veya değerler sütunu olarak sağlanabilir ikinci bir bağımsız değişken alır.  

### <a name="polygamma"></a>Poligama

Poligama fonksiyonunun değerini verir.  

Bu işlev, sabit veya değerler sütunu olarak sağlanabilir ikinci bir bağımsız değişken alır.

##  <a name="trigonometric-functions"></a>Trigonometrik fonksiyonlar 

Bu kategori de önemli trigonometrik ve ters trigonometrik fonksiyonların çoğunu içerir. Tüm trigonometrik işlevler unary ve ek bağımsız değişkenler gerektirir.  

### <a name="acos"></a>Acos

Sütun değerleri için arccosine hesaplar.  

### <a name="acosdegree"></a>AcosDerece

Sütun değerlerinin arccosine derece olarak hesaplar.  

### <a name="acosh"></a>Acosh

Sütun değerlerinin hiperbolik arckozini hesaplar.  

### <a name="acot"></a>Acot

Sütun değerlerinin arccotant'ını hesaplar.  

### <a name="acotdegrees"></a>AcotDegrees

Sütun değerlerinin arccotant'ını derece olarak hesaplar.  

### <a name="acoth"></a>Acoth

Sütun değerlerinin hiperbolik arccotant'ını hesaplar.  

### <a name="acsc"></a>Acsc

Sütun değerlerinin arkentot'unu hesaplar.  

### <a name="acscdegrees"></a>AcscDegrees

Sütun değerlerinin arkentot'unu derece olarak hesaplar.  
### <a name="asec"></a>Asec

Sütun değerlerinin yaylı değerini hesaplar.  

### <a name="asecdegrees"></a>AsecDegrees

Sütun değerlerinin yay değerini derece olarak hesaplar.  

### <a name="asech"></a>Asech

Sütun değerlerinin hiperbolik yaylarını hesaplar.  

### <a name="asin"></a>Asin

Sütun değerlerinin arcsine'sini hesaplar.  

### <a name="asindegrees"></a>AsinDegrees

Sütun değerlerinin yay larını derece olarak hesaplar.  

### <a name="asinh"></a>Asinh

Sütun değerleri için hiperbolik arcsine hesaplar.  

### <a name="atan"></a>Atan

Sütun değerlerinin arktantını hesaplar.  

### <a name="atandegrees"></a>AtanDegrees

Sütun değerlerinin arktantını derece olarak hesaplar.  

### <a name="atanh"></a>Atanh

Sütun değerlerinin hiperbolik arctant'ını hesaplar.  

### <a name="cos"></a>Cos

Sütun değerlerinin kosinüslerini hesaplar.  

### <a name="cosdegrees"></a>CosDegrees

Sütun değerleri için kosinüsün derece olarak hesaplar.  

### <a name="cosh"></a>Cosh

Sütun değerleri için hiperbolik kosinüs hesaplar.  

### <a name="cot"></a>Cot

Sütun değerleri için kotanjant hesaplar.  

### <a name="cotdegrees"></a>Karyola Dereceleri

Sütun değerleri için kotanjantı derece olarak hesaplar.  

### <a name="coth"></a>Koth
Sütun değerleri için hiperbolik kotanjant hesaplar.  

### <a name="csc"></a>Csc

Sütun değerleri için cosecant hesaplar.  

### <a name="cscdegrees"></a>CscDereceler

Sütun değerlerinin cosecant'ını derece olarak hesaplar.  

### <a name="csch"></a>Csch

Sütun değerleri için hiperbolik cosecant hesaplar.  

### <a name="degreestoradians"></a>DereceToRadians

Dereceyi radyana dönüştürür.  

### <a name="sec"></a>Sn

Sütun değerlerinin secantını hesaplar.  

### <a name="asecdegrees"></a>aSecDegrees

Sütun değerlerinin secantını derece olarak hesaplar.  

### <a name="asech"></a>ASech

Sütun değerlerinin hiperbolik secantını hesaplar.  

### <a name="sign"></a>İşaret

Sütun değerlerinin işaretini verir.  

### <a name="sin"></a>Sin

Sütun değerlerinin sinüslerini hesaplar.  

### <a name="sinc"></a>Sinc

Sütun değerlerinin sinüs-kosinüs değerini hesaplar.  

### <a name="sindegrees"></a>SinDegrees

Sütun değerlerinin sinüslerini derece olarak hesaplar.  

### <a name="sinh"></a>Sinh

Sütun değerlerinin hiperbolik sinüslerini hesaplar.  

### <a name="tan"></a>Tan

Sütun değerlerinin teğetini hesaplar.  

### <a name="tandegrees"></a>TanDegrees

Argüman için teğeti derece olarak hesaplar.  

### <a name="tanh"></a>Tanh

Sütun değerlerinin hiperbolik teğetini hesaplar.  
  
## <a name="technical-notes"></a>Teknik notlar

İkinci işleç olarak birden fazla sütun seçerken dikkatli olun. İşlemin tüm sütunlara sabit ekleme gibi basit olup olmadığını anlamak kolaydır. 

Veri kümenizde birden çok sütun olduğunu varsayalım ve veri kümesini kendisine ekleyin. Sonuçlarda, her sütun kendisine aşağıdaki gibi eklenir:  
  
|Sayısal Olarak 1|Sayısal Olarak 2|Sayısal Tsük3|Ekle(Num1_Num1)|Ekle(Num2_Num2)|Ekle(Num3_Num3)|
|----|----|----|----|----|----|
|1|5|2|2|10|4|
|2|3|-1|4|6|-2|
|0|1|-1|0|2|-2|

Daha karmaşık hesaplamalar gerçekleştirmeniz gerekiyorsa, **Matematik İşlemi Uygula'nın**birden çok örneğini zincirleyebilirsiniz. Örneğin, **Matematik İşlemi Uygula'nın**bir örneğini kullanarak iki sütun ekleyebilir ve ardından ortalamayı elde etmek için toplamı sabite bölmek için **Matematik İşlemi Uygula'nın** başka bir örneğini kullanabilirsiniz.  
  
Alternatif olarak, SQL, R veya Python komut dosyasını kullanarak tüm hesaplamaları aynı anda yapmak için aşağıdaki modüllerden birini kullanın:
 
+ [R Betiği yürütme](execute-r-script.md)
+ [Python Betiği Yürütme](execute-python-script.md)
+ [SQL Dönüşümü Uygulama](apply-sql-transformation.md)   
  
## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 
