---
title: Dağıtım & modeller oluşturmak için oto ml kullanma
titleSuffix: Azure Machine Learning
description: Azure Machine Learning ile otomatik makine öğrenimi modelleri oluşturun, gözden geçirin ve dağıtın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: nibaccam
author: tsikiksr
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 03/10/2020
ms.openlocfilehash: 0d6fa02578814c4c5d034be05cbc63093d70603b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81257241"
---
# <a name="create-review-and-deploy-automated-machine-learning-models-with-azure-machine-learning"></a>Azure Machine Learning ile otomatik makine öğrenimi modelleri oluşturun, gözden geçirin ve dağıtın
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Bu makalede, Azure Machine Learning Studio arabiriminde tek bir kod satırı olmadan otomatik makine öğrenimi modelleri oluşturmayı, keşfetmeye ve dağıtmayı öğreneceksiniz. Otomatik makine öğrenimi, sizin için belirli verileriniz için kullanılacak en iyi makine öğrenimi algoritmasının sizin için seçildiği bir işlemdir. Bu işlem, makine öğrenimi modellerini hızlı bir şekilde oluşturmanıza olanak sağlar. [Otomatik makine öğrenimi hakkında daha fazla bilgi edinin](concept-automated-ml.md).
 
Uçtan uca örnek için [Azure Machine Learning OTOMATIKLEŞTIRILMIŞ ml arabirimiyle bir sınıflandırma modeli oluşturma öğreticisini](tutorial-first-experiment-automated-ml.md)deneyin. 

Python kod tabanlı bir deneyim için [otomatik makine öğrenimi denemeleri](how-to-configure-auto-train.md) Azure Machine Learning SDK ile yapılandırın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* **Enterprise Edition**türünde bir Azure Machine Learning çalışma alanı. Bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md).  Mevcut bir çalışma alanını Enterprise Edition 'a yükseltmek için bkz. [Enterprise Edition 'A yükseltme](how-to-manage-workspace.md#upgrade).

## <a name="get-started"></a>başlarken

1. Azure Machine Learning için oturum açın https://ml.azure.com. 

1. Aboneliğinizi ve çalışma alanınızı seçin. 

1. Sol bölmeye gidin. **Yazar** bölümü altında **Otomatik ml** ' yi seçin.

[![Azure Machine Learning Studio gezinti bölmesi](media/how-to-use-automated-ml-for-ml-models/nav-pane.png)](media/how-to-use-automated-ml-for-ml-models/nav-pane-expanded.png)

 Denemeleri ilk kez varsa, boş bir liste ve belge bağlantıları görürsünüz. 

Aksi takdirde, SDK ile oluşturulanlar da dahil olmak üzere, son otomatik makine öğrenimi denemeleri bir listesini görürsünüz. 

## <a name="create-and-run-experiment"></a>Deneme oluşturma ve çalıştırma

1. **+ Yeni OTOMATIK ml Çalıştır** ' ı seçin ve formu doldurun.

1. Depolama kapsayıcıınızdan bir veri kümesi seçin veya yeni bir veri kümesi oluşturun. Veri kümeleri yerel dosyalardan, Web URL 'lerinden, veri depolarından veya Azure açık veri kümelerinden oluşturulabilir. 

    >[!Important]
    > Eğitim verileri için gereksinimler:
    >* Verilerin tablolu biçimde olması gerekir.
    >* Tahmin etmek istediğiniz değer (hedef sütun) verilerde bulunmalıdır.

    1. Yerel bilgisayarınızdaki bir dosyadan yeni bir veri kümesi oluşturmak için, **Araştır** ' ı seçin ve ardından dosyayı seçin. 

    1. Veri kümenize benzersiz bir ad verin ve isteğe bağlı bir açıklama sağlayın. 

    1. **Veri deposu ve dosya seçim formunu**açmak için **İleri ' yi** seçin. Bu formda veri kümenizin nereye yükleneceğini seçersiniz; Çalışma alanım ile otomatik olarak oluşturulan varsayılan depolama kapsayıcısı veya deneme için kullanmak istediğiniz bir depolama kapsayıcısı seçin. 

    1. Doğruluk için **ayarları ve önizleme** formunu gözden geçirin. Form, dosya türüne göre akıllıca doldurulur. 

        Alan| Açıklama
        ----|----
        Dosya biçimi| Bir dosyada depolanan verilerin yerleşimini ve türünü tanımlar.
        Sınırlayıcı| Düz metin veya diğer veri akışlarında ayrı, bağımsız bölgeler arasındaki sınırı belirtmek için bir veya daha fazla karakter.
        Encoding| Veri kümenizi okumak için kullanılacak bit karakter şeması tablosunu belirler.
        Sütun başlıkları| Veri kümesinin üst bilgilerinin (varsa) nasıl değerlendirileceğini gösterir.
        Satırları atla | Veri kümesinde kaç tane, ne varsa satırların atlandığını gösterir.
    
        **İleri**’yi seçin.

    1. **Şema** formu, **Ayarlar ve önizleme** formundaki seçimlere göre akıllıca doldurulur. Burada her bir sütun için veri türünü yapılandırın, sütun adlarını gözden geçirin ve denemenize **dahil olmayan** sütunları seçin. 
            
        **İleri ' yi seçin.**

    1. **Ayrıntıları Onayla** formu, daha önce **temel bilgi** ve **Ayarlar ve önizleme** formlarında doldurulmuş bilgilerin bir özetidir. Ayrıca, profil oluşturma etkinleştirilmiş bir işlem kullanarak veri kümeniz için bir veri profili oluşturma seçeneğiniz de vardır. [Veri profili oluşturma](#profile)hakkında daha fazla bilgi edinin.

        **İleri**’yi seçin.
1. Yeni oluşturduğunuz veri kümenizi göründüğünde seçin. Ayrıca, veri kümesinin ve örnek istatistiklerin önizlemesini de görüntüleyebilirsiniz. 

1. **Çalıştırma yapılandırması** formunda, benzersiz bir deneme adı girin.

1. Hedef sütun seçin; Bu, tahminlerde yapmak istediğiniz sütundur.

1. Veri profili oluşturma ve eğitim işi için bir işlem seçin. Mevcut hesaplamalarınızın listesi açılan menüde bulunabilir. Yeni bir işlem oluşturmak için adım 7 ' deki yönergeleri izleyin.

1. Bu deneme için işlem bağlamını yapılandırmak üzere **Yeni bir Işlem oluştur** ' u seçin.

    Alan|Açıklama
    ---|---
    İşlem adı| İşlem bağlamını tanımlayan benzersiz bir ad girin.
    Sanal makine boyutu| İşlem için sanal makine boyutunu seçin.
    En az/en fazla düğüm (Gelişmiş ayarlarda)| Veri profili için, 1 veya daha fazla düğüm belirtmeniz gerekir. İşlem için en fazla düğüm sayısını girin. Varsayılan değer bir AML Işlem için 6 düğümünüz.
    
    **Oluştur**’u seçin. Yeni bir işlem oluşturmak birkaç dakika sürebilir.

    >[!NOTE]
    > İşlem adınız, seçtiğiniz işlem *profil oluşturma*/oluşturma işleminin etkin olup olmadığını gösterir. (Daha fazla ayrıntı için bkz. [veri profili oluşturma](#profile) bölümü).

    **İleri**’yi seçin.

1. **Görev türü ve ayarlar** formunda, görev türünü seçin: sınıflandırma, regresyon veya tahmin. Daha fazla bilgi için bkz. [görev türlerini tanımlama](how-to-define-task-type.md) .

    1. Sınıflandırma için, metin özelliklerini de kullanan derin öğrenmeyi de etkinleştirebilirsiniz.

    1. Tahmin için:
        1. Zaman sütununu seçin: Bu sütun kullanılacak saat verilerini içerir.

        1. Tahmin ufku seçin: modelin kaç zaman birimi (dakika/saat/gün/hafta/ay/yıl) gelecek şekilde tahmin edemeyeceğini belirtin. Daha sonra modelin daha iyi tahmin edilmesi gerektiğinde, daha az doğru olacaktır. [Tahmin ve tahmin ufku hakkında daha fazla bilgi edinin](how-to-auto-train-forecast.md).

1. Seçim Ek yapılandırma ayarlarını görüntüle: eğitim işini daha iyi denetleyebilmeniz için kullanabileceğiniz ek ayarlar. Aksi takdirde, denemeler seçimine ve verilerine göre varsayılan ayarlar uygulanır. 

    Ek yapılandırmalar|Açıklama
    ------|------
    Birincil ölçüm| Modelinize Puanlama için kullanılan ana ölçüm. [Model ölçümleri hakkında daha fazla bilgi edinin](how-to-configure-auto-train.md#explore-model-metrics).
    Otomatik olarak korleştirme| Otomatik makine öğrenimi tarafından gerçekleştirilen ön işleme özelliğini etkinleştirmek veya devre dışı bırakmak için seçin. Ön işleme, yapay özellikler oluşturmak için otomatik veri temizleme, hazırlama ve dönüştürme içerir. Zaman serisi tahmin görev türü için desteklenmez. [Ön işleme hakkında daha fazla bilgi edinin](#featurization). 
    En iyi modeli açıkla | Önerilen en iyi modelin explainability gösterilmesini etkinleştirmek veya devre dışı bırakmak için seçin
    Engellenen algoritma| Eğitim işinden dışlamak istediğiniz algoritmaları seçin.
    Çıkış ölçütü| Bu ölçütlerden herhangi biri karşılandığında eğitim işi durdurulur. <br> *Eğitim işi süresi (saat)*: eğitim işinin ne kadar süreyle çalışmasına izin verme. <br> *Ölçüm puan eşiği*: tüm işlem hatları için en düşük ölçüm puanı. Bu, ulaşmak istediğiniz tanımlı bir hedef ölçüsünün olması durumunda eğitim işinde gerekli olandan daha fazla zaman harcamamanızı sağlar.
    Doğrulama| Eğitim işinde kullanmak için çapraz doğrulama seçeneklerinden birini seçin. [Çapraz doğrulama hakkında daha fazla bilgi edinin](how-to-configure-auto-train.md).
    Eşzamanlılık| *Maksimum eşzamanlı yineleme*: eğitim işinde sınanacak maksimum işlem hattı sayısı (yineleme). İş, belirtilen sayıda yinelemeden daha fazla çalıştırmayacak.

1. Seçim Özellik ayarlarını görüntüleme: **ek yapılandırma ayarları** formunda **Otomatik** özelliği etkinleştirmeyi seçerseniz bu form, bu özelliği hangi sütunların gerçekleştireceğini ve eksik imputations değeri için hangi istatistiksel değerin kullanılacağını seçin.

<a name="profile"></a>

## <a name="data-profiling--summary-stats"></a>Veri profili oluşturma & Özet istatistikleri

Veri ayarlanmış olup olmadığını doğrulamak için veri kümesi genelinde çok sayıda Özet istatistiği alabilir. Sayısal olmayan sütunlarda yalnızca min, Max ve Error Count gibi temel istatistikler bulunur. Sayısal sütunlarda, istatistiksel ve tahmini quantiles değerlerini de gözden geçirebilirsiniz. Veri profiliniz özellikle şunları içerir:

>[!NOTE]
> İlgisiz türler içeren özellikler için boş girdiler görüntülenir.

İstatistik|Açıklama
------|------
Özellik| Özetlenen sütunun adı.
Profil| Çıkarılan türe göre satır içi görselleştirme. Örneğin, dizeler, Boole değerleri ve tarihler değer sayılarına sahip olacaktır, ancak Ondalıklar (Numerics) de yaklaşık histogramlar olur. Bu, verilerin dağıtımını hızlı bir şekilde anlayabilmeniz için size izin verir.
Tür dağılımı| Bir sütun içindeki türlerin satır içi değer sayısı. Null değerler kendi türlerdir, bu nedenle bu görselleştirme tek veya eksik değerleri algılamak için yararlıdır.
Tür|Sütunun Çıkarsanan türü. Olası değerler şunlardır: dizeler, Boole değerleri, tarihler ve ondalıklar.
Min| Sütunun minimum değeri. Boş girdiler, türü bir devralınan sıralamaya (örn. Boolean) sahip olmayan özellikler için görünür.
Maks| Sütunun en büyük değeri. 
Sayı| Sütundaki eksik ve eksik olmayan girdilerin toplam sayısı.
Eksik sayı yok| Sütundaki eksik girdi sayısı. Boş dizeler ve hatalar değer olarak değerlendirilir ve bu nedenle "eksik sayısı" öğesine katkıda bulunmazlar.
Quantiles| Verilerin dağıtılması hakkında bir fikir sağlamak için her bir satışla yaklaşık değerler.
Ortalama| Sütunun aritmetik ortalaması veya ortalaması.
Standart sapma| Bu sütun verisinin dağılım veya değişim miktarının ölçümü.
Varyans| Bu sütunun verilerinin ne kadar yayıldığı, ortalama değerinden oluşan ölçüdür. 
Eğrilikleri| Bu sütun verilerinin normal bir dağılıma göre ne kadar farklı olduğunu gösteren ölçü.
Sıklık| Bu sütun verilerinin ne kadar süden bir normal dağıtımla karşılaştırıldığı ölçüdür.

<a name="featurization"></a>

## <a name="advanced-featurization-options"></a>Gelişmiş özellik seçenekleri

Otomatik makine öğrenimi, verilerle ilgili olası sorunları belirlemenize ve yönetmenize yardımcı olmak için otomatik olarak ön işleme ve veri guardları sunar. 

### <a name="preprocessing"></a>Ön

> [!NOTE]
> Auto ML tarafından oluşturulan modellerinizi bir [onnx modeline](concept-onnx.md)dışarı aktarmayı planlıyorsanız, onnx biçiminde yalnızca bir * ile gösterilen featurleştirme seçenekleri desteklenir. [Modelleri ONNX 'e dönüştürme](concept-automated-ml.md#use-with-onnx)hakkında daha fazla bilgi edinin. 

|Ön&nbsp;işleme adımları| Açıklama |
| ------------- | ------------- |
|Yüksek önem düzeyi bırakma veya varyans özelliği yok * |Tüm değerleri eksik olan özellikler de dahil olmak üzere eğitim ve doğrulama kümelerinden bunları bırakın, tüm satırlarda veya çok yüksek kardinalite (örneğin, karma, kimlik veya GUID) ile aynı değeri kullanın.|
|Impute eksik değerler * |Sayısal özellikler için sütunundaki değerleri ortalama olan ımpute.<br/><br/>Kategorik özellikler için en sık değer içeren ımpute.|
|Ek özellikler oluştur * |Tarih saat özellikleri için: yıl, ay, gün, haftanın günü, yılın günü, üç aylık dönem, yılın haftası, saat, dakika, saniye.<br/><br/>Metin özellikleri için: tekli gram, Çift gram ve üçlü karakter-gram temelinde Dönem sıklığı.|
|Dönüştür ve kodla *|Birkaç benzersiz değer içeren sayısal özellikler kategorik özelliklere dönüştürülür.<br/><br/>Düşük kardinalite kategorik için tek bir etkin kodlama gerçekleştirilir; yüksek kardinalite, tek bir sıcak karma kodlama için.|
|Sözcük katıştırlamaları|Metin belirteçlerinin vektörlerini, önceden eğitilen bir model kullanarak tümce vektörlerine dönüştüren metin korleştirici. Belge özellik vektörü oluşturmak için bir belgedeki her bir sözcüğün katıştırma vektörü birlikte toplanır.|
|Hedef kodlamalar|Kategorik özellikler için, her bir kategoriyi gerileme sorunları için Ortalama hedef değeriyle ve sınıflandırma sorunları için her bir sınıfın sınıf olasılığa eşler. Sıklık tabanlı ağırlığa ve k katlamalı çapraz doğrulama, seyrek veri kategorilerinin neden olduğu eşlemenin ve gürültü üzerine gitmek için geçerlidir.|
|Metin hedefi kodlaması|Metin girişi için, her bir sınıfın olasılığını oluşturmak için kelimeleri olan bir yığılmış Doğrusal model kullanılır.|
|Kanıt ağırlığı (WoE)|, Kategorik sütunların bağıntısı olarak hedef sütuna bir ölçü olarak, WoE hesaplar. Sınıf içi ve sınıf dışı olasılıkların oranının günlüğü olarak hesaplanır. Bu adım, her sınıf için bir sayısal özellik sütunu verir ve eksik değerler ve aykırı değer işleme gereksinimini ortadan kaldırır.|
|Küme uzaklığı|K. a, tüm sayısal sütunlarda kümeleme modeli anlamına gelir.  Her bir örneğin her bir kümenin centroıd değerine her bir örnek arasındaki mesafeyi içeren, her küme için yeni bir sayısal özellik olan yeni özellik çıkışları.|

### <a name="data-guardrails"></a>Veri, guardrayları

Otomatik özellik etkinleştirildiğinde veya doğrulama otomatik olarak ayarlandığında, veri guardları uygulanır. Data guardrayları, verileriniz ile ilgili olası sorunları belirlemenize yardımcı olur (örneğin, eksik değerler, sınıf dengesizliği) ve geliştirilmiş sonuçlar için düzeltici eylemler elde etmenize yardımcı olur. 

Kullanıcılar, bir otomatik ML çalıştırmasının **veri verileri** , veya Python SDK 'sını kullanarak bir deneme ```show_output=True``` gönderdiğinizde, Studio 'daki veri guardları sekmesindeki verileri gözden geçirebilir. 

#### <a name="data-guardrail-states"></a>Veri Guardlığı durumları

Data guardrayları şu üç durumdan birini görüntüler: **başarılı**, **bitti**veya **Uyarı**.

Durum| Açıklama
----|----
Geçiril| Hiçbir veri sorunu algılanmadı ve Kullanıcı eylemi gerekli değildir. 
Bitti| Verilerinize değişiklikler uygulandı. Kullanıcıların, değişikliklerin beklenen sonuçlarla hizalandığını garantilemek için otomatik ML 'nin düzeltici eylemleri gözden geçirmesini öneririz. 
Uyarı| Düzeltmelere neden olan bir veri sorunu algılandı. Kullanıcıları sorunu gözden geçirmenizi ve düzeltmesini öneririz. 

>[!NOTE]
> Önceki otomatikleştirilmiş ML denemeleri sürümleri dördüncü bir durum görüntülendi: **düzeltildi**. Daha yeni denemeleri bu durumu görüntülemez ve **sabit** durumu görüntülenen tüm guardrayları artık **tamamlandı**olarak görüntülenecektir.   

Aşağıdaki tabloda, şu anda desteklenmekte olan veriler ve kullanıcıların denedikleri zaman içinde karşılaşabileceğiniz ilgili durumlar açıklanmaktadır.

Guarddemiryolu|Durum|Tetikleyici&nbsp;için&nbsp;koşul
---|---|---
Eksik özellik değerleri imputation |**Geçiril** <br><br><br> **Bitti**| Eğitim verilerinizde eksik özellik değeri algılanmadı. [Eksik imputation değeri](https://docs.microsoft.com/azure/machine-learning/how-to-use-automated-ml-for-ml-models#advanced-featurization-options) hakkında daha fazla bilgi edinin. <br><br> Eğitim verilerinizde eksik özellik değerleri algılandı ve genişletilmiş.
Yüksek kardinalite özelliği işleme |**Geçiril** <br><br><br> **Bitti**| Girişlerinizin çözümlenmesi ve yüksek kardinalite özelliklerinin saptanmamış olması. [Yüksek kardinalite Özellik algılaması](https://docs.microsoft.com/azure/machine-learning/how-to-use-automated-ml-for-ml-models#advanced-featurization-options) hakkında daha fazla bilgi edinin. <br><br> Girdilerde yüksek kardinalite özellikleri algılandı ve işlendi.
Doğrulama bölünmüş işleme |**Bitti**| *Doğrulama yapılandırması ' Auto ' olarak ayarlandı ve eğitim verileri 20.000 satırdan **daha az** satır içeriyordu.* <br> Eğitilen modelin her yinelemesi çapraz doğrulama aracılığıyla doğrulanmıştı. [Doğrulama verileri](https://docs.microsoft.com/azure/machine-learning/how-to-configure-auto-train#train-and-validation-data) hakkında daha fazla bilgi edinin. <br><br> *Doğrulama yapılandırması ' Auto ' olarak ayarlandı ve eğitim verileri 20.000 satırdan **fazla** satır içeriyordu.* <br> Giriş verileri, modelin doğrulanması için bir eğitim veri kümesine ve bir doğrulama veri kümesine bölündü.
Sınıf Dengeleme algılaması |**Geçiril** <br><br><br><br> **Uyarı** | Girişlerinizin çözümlenmesi ve tüm sınıfların eğitim verilerinizde dengelenmesi. Her sınıfın veri kümesinde iyi bir temsili varsa, örneklerin sayısı ve oranı ile ölçüldüğü bir veri kümesi dengeli olarak değerlendirilir. <br><br><br> Girdilerde imdengelenmiş sınıflar algılandı. Model sapmalarının giderilmesi için, Dengeleme sorununu düzeltir. [İmdengelenmiş veriler](https://docs.microsoft.com/azure/machine-learning/concept-manage-ml-pitfalls#identify-models-with-imbalanced-data) hakkında daha fazla bilgi edinin.
Bellek sorunları algılama |**Geçiril** <br><br><br><br> **Bitti** |<br> Seçilen {ufuk, öteleme, hareketli pencere} değerleri çözümlendi ve olası bellek dışı sorunlar algılandı. Zaman serisi [tahmin yapılandırması](https://docs.microsoft.com/azure/machine-learning/how-to-auto-train-forecast#configure-and-run-experiment) hakkında daha fazla bilgi edinin. <br><br><br>Seçilen {ufuk, öteleme, hareketli pencere} değerleri çözümlendi ve bu, denemenizin belleği tükenmesine neden olacak. Öteleme veya sıralı pencere yapılandırması kapatılmış.
Sıklık algılama |**Geçiril** <br><br><br><br> **Bitti** |<br> Zaman serisi çözümlendi ve tüm veri noktaları algılanan sıklığa göre hizalanır. <br> <br> Zaman serisi çözümlendi ve algılanan sıklığa göre hizalanmayan veri noktaları algılandı. Bu veri noktaları, veri kümesinden kaldırılmıştır. [Zaman serisi tahminiyle ilgili veri hazırlığı](https://docs.microsoft.com/azure/machine-learning/how-to-auto-train-forecast#preparing-data) hakkında daha fazla bilgi edinin.

## <a name="run-experiment-and-view-results"></a>Deneme çalıştırma ve sonuçları görüntüleme

Denemenizi çalıştırmak için **son** ' u seçin. Denemeyi hazırlama işlemi 10 dakika kadar sürebilir. Eğitim işlerinde her işlem hattının çalıştırılmasını tamamlamak fazladan 2-3 dakika daha sürebilir.

### <a name="view-experiment-details"></a>Deneme ayrıntılarını görüntüleme

**Ayrıntı Çalıştır** ekranı **Ayrıntılar** sekmesinde açılır. Bu ekranda, çalışma numarasının yanında bir durum çubuğu dahil olmak üzere deneme çalıştırmasının bir özeti gösterilir. 

**Modeller** sekmesi oluşturulan ve ölçüm puanına göre sıralanan model listesini içerir. Varsayılan olarak, seçilen ölçüme göre en yüksek puana sahip olan model listenin en üstünde yer alır. Eğitim işi daha fazla model denedikçe, bu modeller listeye eklenir. Bunu kullanarak, şimdiye kadar oluşturulan modeller için ölçümlerin hızlı bir karşılaştırmasını elde edebilirsiniz.

[![Çalışma ayrıntıları panosu](media/how-to-use-automated-ml-for-ml-models/run-details.png)](media/how-to-use-automated-ml-for-ml-models/run-details-expanded.png#lightbox)

### <a name="view-training-run-details"></a>Eğitim çalışma ayrıntılarını görüntüle

**Görselleştirme** sekmesindeki **model ayrıntıları** sekmesinde veya Performans grafiklerinde ölçümleri Çalıştır gibi eğitim çalıştırma ayrıntılarını görmek için tamamlanan modellerin herhangi birinde detaya gidin. [grafikler hakkında daha fazla bilgi edinin](how-to-understand-automated-ml.md).

[![Yineleme ayrıntıları](media/how-to-use-automated-ml-for-ml-models/iteration-details.png)](media/how-to-use-automated-ml-for-ml-models/iteration-details-expanded.png)

## <a name="deploy-your-model"></a>Modelinizi dağıtın

En iyi modeli elde etmeniz durumunda, yeni verileri tahmin etmek için bunu bir Web hizmeti olarak dağıtmanız zaman alır.

Otomatikleştirilmiş ML, kodu yazmadan modeli dağıtmanıza yardımcı olur:

1. Dağıtım için birkaç seçeneğiniz vardır. 

    + Seçenek 1: en iyi modeli dağıtmak Için (tanımladığınız ölçüm ölçütlerine göre), **Ayrıntılar** sekmesinde **en iyi modeli dağıt** düğmesini seçin.

    + 2. seçenek: bu deneyime ait belirli bir model yinelemesini dağıtmak Için modelin detayına gidin ve model **ayrıntıları** sekmesini açın ve **modeli dağıt**' ı seçin.

1. **Modeli dağıt** bölmesini doldurun.

    Alan| Değer
    ----|----
    Adı| Dağıtımınız için benzersiz bir ad girin.
    Açıklama| Bu dağıtımın ne için olduğunu daha iyi tanımlamak için bir açıklama girin.
    İşlem türü| Dağıtmak istediğiniz uç nokta türünü seçin: *Azure Kubernetes hizmeti (AKS)* veya *Azure Container Instance (acı)*.
    İşlem adı| *Yalnızca AKS Için geçerlidir:* Dağıtmak istediğiniz AKS kümesinin adını seçin.
    Kimlik doğrulamasını etkinleştir | Belirteç tabanlı veya anahtar tabanlı kimlik doğrulamasına izin vermek için seçin.
    Özel dağıtım varlıklarını kullanma| Kendi Puanlama betiğinizi ve ortam dosyanızı karşıya yüklemek istiyorsanız bu özelliği etkinleştirin. [Puanlama betikleri hakkında daha fazla bilgi edinin](how-to-deploy-and-where.md#script).

    >[!Important]
    > Dosya adları 32 karakterden oluşmalıdır ve alfasayısal karakterlerle başlamalı ve bitmelidir. Arasına tire, alt çizgi, nokta ve alfasayısal karakterler içerebilir. Boşluklara izin verilmez.

    *Gelişmiş* menü, [veri toplama](how-to-enable-app-insights.md) ve kaynak kullanımı ayarları gibi varsayılan dağıtım özelliklerini sunar. Bu Varsayılanları geçersiz kılmak istiyorsanız bu menüdeki bu ayarları yapın.

1. **Dağıt**'ı seçin. Dağıtımın tamamlanması yaklaşık 20 dakika sürebilir.

Artık tahmin oluşturmak için işlemsel bir Web hizmetiniz vardır! [Power BI yerleşik Azure Machine Learning desteği '](how-to-consume-web-service.md#consume-the-service-from-power-bi)nden hizmeti sorgulayarak tahminleri test edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Bir Web hizmetini kullanmayı öğrenin](https://docs.microsoft.com/azure/machine-learning/how-to-consume-web-service).
* [Otomatik makine öğrenimi sonuçlarını anlayın](how-to-understand-automated-ml.md).
* [Otomatik makine öğrenimi ve Azure Machine Learning hakkında daha fazla bilgi edinin](concept-automated-ml.md) .
