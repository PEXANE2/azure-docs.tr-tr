---
title: Dağıtım & modeller oluşturmak için oto ml kullanma
titleSuffix: Azure Machine Learning
description: Azure Machine Learning ile otomatik makine öğrenimi modelleri oluşturun, gözden geçirin ve dağıtın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: nibaccam
author: aniththa
ms.reviewer: nibaccam
ms.date: 07/10/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 10c0200aae5ffa432c2da037d58d455fc28e8acd
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90904925"
---
# <a name="create-review-and-deploy-automated-machine-learning-models-with-azure-machine-learning"></a>Azure Machine Learning ile otomatik makine öğrenimi modelleri oluşturun, gözden geçirin ve dağıtın


Bu makalede, Azure Machine Learning Studio 'da tek bir kod satırı olmadan otomatik makine öğrenimi modelleri oluşturmayı, keşfetmeye ve dağıtmayı öğreneceksiniz.

Otomatik makine öğrenimi, sizin için belirli verileriniz için kullanılacak en iyi makine öğrenimi algoritmasının sizin için seçildiği bir işlemdir. Bu işlem, makine öğrenimi modellerini hızlı bir şekilde oluşturmanıza olanak sağlar. [Otomatik makine öğrenimi hakkında daha fazla bilgi edinin](concept-automated-ml.md).
 
Uçtan uca örnek için [Azure Machine Learning OTOMATIKLEŞTIRILMIŞ ml arabirimiyle bir sınıflandırma modeli oluşturma öğreticisini](tutorial-first-experiment-automated-ml.md)deneyin. 

Python kod tabanlı bir deneyim için [otomatik makine öğrenimi denemeleri](how-to-configure-auto-train.md) Azure Machine Learning SDK ile yapılandırın.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* Azure Machine Learning çalışma alanı. Bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md). 

## <a name="get-started"></a>başlarken

1. Azure Machine Learning için oturum açın https://ml.azure.com . 

1. Aboneliğinizi ve çalışma alanınızı seçin. 

1. Sol bölmeye gidin. **Yazar** bölümü altında **Otomatik ml** ' yi seçin.

[![Azure Machine Learning Studio gezinti bölmesi](media/how-to-use-automated-ml-for-ml-models/nav-pane.png)](media/how-to-use-automated-ml-for-ml-models/nav-pane-expanded.png)

 Denemeleri ilk kez varsa, boş bir liste ve belge bağlantıları görürsünüz. 

Aksi takdirde, SDK ile oluşturulanlar da dahil olmak üzere, son otomatik makine öğrenimi denemeleri bir listesini görürsünüz. 

## <a name="create-and-run-experiment"></a>Deneme oluşturma ve çalıştırma

1. **+ Yeni OTOMATIK ml Çalıştır** ' ı seçin ve formu doldurun.

1. Depolama kapsayıcıınızdan bir veri kümesi seçin veya yeni bir veri kümesi oluşturun. Veri kümeleri yerel dosyalardan, Web URL 'lerinden, veri depolarından veya Azure açık veri kümelerinden oluşturulabilir. [Veri kümesi oluşturma](how-to-create-register-datasets.md)hakkında daha fazla bilgi edinin.  

    >[!Important]
    > Eğitim verileri için gereksinimler:
    >* Verilerin tablolu biçimde olması gerekir.
    >* Tahmin etmek istediğiniz değer (hedef sütun) verilerde bulunmalıdır.

    1. Yerel bilgisayarınızdaki bir dosyadan yeni bir veri kümesi oluşturmak için **+ veri kümesi oluştur** ' u seçin ve ardından **yerel dosya**' yı seçin. 

    1. **Temel bilgi** formunda, veri kümenize benzersiz bir ad verin ve isteğe bağlı bir açıklama sağlayın. 

    1. **Veri deposu ve dosya seçim formunu**açmak için **İleri ' yi** seçin. Bu formda veri kümenizin nereye yükleneceğini seçersiniz; Çalışma alanım ile otomatik olarak oluşturulan varsayılan depolama kapsayıcısı veya deneme için kullanmak istediğiniz bir depolama kapsayıcısı seçin. 
    
        1. Verileriniz bir sanal ağın arkasındaysa, çalışma alanının verilerinize erişebildiğinden emin olmak için **doğrulamayı atla** işlevini etkinleştirmeniz gerekir. Daha fazla bilgi için bkz. [Azure sanal ağında Azure Machine Learning Studio 'Yu kullanma](how-to-enable-studio-virtual-network.md). 
    
    1. Veri kümeniz için veri dosyasını karşıya yüklemek üzere **Araştır** ' ı seçin. 

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
    Sanal makine önceliği| Düşük öncelikli sanal makineler, daha ucuz ancak işlem düğümlerini garanti etmez. 
    Sanal makine türü| Sanal makine türü için CPU veya GPU seçin.
    Sanal makine boyutu| İşlem için sanal makine boyutunu seçin.
    En az/en fazla düğüm| Veri profili için, 1 veya daha fazla düğüm belirtmeniz gerekir. İşlem için en fazla düğüm sayısını girin. Varsayılan değer bir AML Işlem için 6 düğümünüz.
    Gelişmiş ayarlar | Bu ayarlar, denemeniz için bir kullanıcı hesabı ve var olan bir sanal ağ yapılandırmanıza olanak tanır. 
    
    **Oluştur**’u seçin. Yeni bir işlem oluşturmak birkaç dakika sürebilir.

    >[!NOTE]
    > İşlem adınız, seçtiğiniz işlem *profil oluşturma*/oluşturma işleminin etkin olup olmadığını gösterir. (Daha fazla ayrıntı için bkz. [veri profili oluşturma](#profile) bölümü).

    **İleri**’yi seçin.

1. **Görev türü ve ayarlar** formunda, görev türünü seçin: sınıflandırma, regresyon veya tahmin. Daha fazla bilgi için bkz. [desteklenen görev türleri](concept-automated-ml.md#when-to-use-automl-classify-regression--forecast) .

    1. **Sınıflandırma**için derin öğrenmeyi de etkinleştirebilirsiniz.
    
        Derin öğrenme etkinse, doğrulama _train_validation bölünmüş_ile sınırlıdır. [Doğrulama seçenekleri hakkında daha fazla bilgi edinin](how-to-configure-cross-validation-data-splits.md).


    1. **Tahmin** için şunları yapabilirsiniz 
    
        1. Derin öğrenmeyi etkinleştirin.
    
        1. *Zaman sütununu*seçin: Bu sütun kullanılacak saat verilerini içerir.

        1. *Tahmin ufku*seçin: modelin kaç zaman birimi (dakika/saat/gün/hafta/ay/yıl) gelecek şekilde tahmin edemeyeceğini belirtin. Daha sonra modelin daha iyi tahmin edilmesi gerektiğinde, daha az doğru olacaktır. [Tahmin ve tahmin ufku hakkında daha fazla bilgi edinin](how-to-auto-train-forecast.md).

1. Seçim Ek yapılandırma ayarlarını görüntüle: eğitim işini daha iyi denetleyebilmeniz için kullanabileceğiniz ek ayarlar. Aksi takdirde, denemeler seçimine ve verilerine göre varsayılan ayarlar uygulanır. 

    Ek yapılandırmalar|Açıklama
    ------|------
    Birincil ölçüm| Modelinize Puanlama için kullanılan ana ölçüm. [Model ölçümleri hakkında daha fazla bilgi edinin](how-to-configure-auto-train.md#primary-metric).
    En iyi modeli açıkla | Önerilen en iyi modelin açıklamalarını göstermek için etkinleştirmek veya devre dışı bırakmak için seçin. <br> Bu işlev, [belirli tahmin algoritmaları](how-to-machine-learning-interpretability-automl.md#interpretability-during-training-for-the-best-model)için şu anda kullanılamıyor. 
    Engellenen algoritma| Eğitim işinden dışlamak istediğiniz algoritmaları seçin. <br><br> Algoritmaların izin verilmesi yalnızca [SDK denemeleri](how-to-configure-auto-train.md#supported-models)için kullanılabilir. <br> [Her görev türü için desteklenen modellere](https://docs.microsoft.com/python/api/azureml-automl-core/azureml.automl.core.shared.constants.supportedmodels?view=azure-ml-py&preserve-view=true)bakın.
    Çıkış ölçütü| Bu ölçütlerden herhangi biri karşılandığında eğitim işi durdurulur. <br> *Eğitim işi süresi (saat)*: eğitim işinin ne kadar süreyle çalışmasına izin verme. <br> *Ölçüm puan eşiği*: tüm işlem hatları için en düşük ölçüm puanı. Bu, ulaşmak istediğiniz tanımlı bir hedef ölçüsünün olması durumunda eğitim işinde gerekli olandan daha fazla zaman harcamamanızı sağlar.
    Doğrulama| Eğitim işinde kullanmak için çapraz doğrulama seçeneklerinden birini seçin. <br> [Çapraz doğrulama hakkında daha fazla bilgi edinin](how-to-configure-cross-validation-data-splits.md#prerequisites).<br> <br>Tahmin yalnızca k katlama çapraz doğrulamayı destekler.
    Eşzamanlılık| *Maksimum eşzamanlı yineleme*: eğitim işinde sınanacak maksimum işlem hattı sayısı (yineleme). İş, belirtilen sayıda yinelemeden daha fazla çalıştırmayacak.

1. Seçim Özelliği görüntüleme ayarları: **ek yapılandırma ayarları** formunda **Otomatik** özelliği etkinleştirmeyi seçerseniz, varsayılan özellikler uygulanır. Görünüm özelliği **ayarları** ' nda bu Varsayılanları değiştirebilir ve uygun şekilde özelleştirebilirsiniz. [Korturleri özelleştirmeyi](#customize-featurization)öğrenin. 

    ![Azure Machine Learning Studio görev türü formu](media/how-to-use-automated-ml-for-ml-models/view-featurization-settings.png)

<a name="profile"></a>

## <a name="data-profiling--summary-stats"></a>Veri profili oluşturma & Özet istatistikleri

Veri ayarlanmış olup olmadığını doğrulamak için veri kümesi genelinde çok sayıda Özet istatistiği alabilir. Sayısal olmayan sütunlarda yalnızca min, Max ve Error Count gibi temel istatistikler bulunur. Sayısal sütunlarda, istatistiksel ve tahmini quantiles değerlerini de gözden geçirebilirsiniz. Veri profiliniz özellikle şunları içerir:

>[!NOTE]
> İlgisiz türler içeren özellikler için boş girdiler görüntülenir.

İstatistik|Açıklama
------|------
Öne çıkan özelliği| Özetlenen sütunun adı.
Profil| Çıkarılan türe göre satır içi görselleştirme. Örneğin, dizeler, Boole değerleri ve tarihler değer sayılarına sahip olacaktır, ancak Ondalıklar (Numerics) de yaklaşık histogramlar olur. Bu, verilerin dağıtımını hızlı bir şekilde anlayabilmeniz için size izin verir.
Tür dağılımı| Bir sütun içindeki türlerin satır içi değer sayısı. Null değerler kendi türlerdir, bu nedenle bu görselleştirme tek veya eksik değerleri algılamak için yararlıdır.
Tür|Sütunun Çıkarsanan türü. Olası değerler şunlardır: dizeler, Boole değerleri, tarihler ve ondalıklar.
Min| Sütunun minimum değeri. Boş girdiler, türü bir devralınan sıralamaya (örn. Boolean) sahip olmayan özellikler için görünür.
En yüksek değer| Sütunun en büyük değeri. 
Count| Sütundaki eksik ve eksik olmayan girdilerin toplam sayısı.
Eksik sayı yok| Sütundaki eksik girdi sayısı. Boş dizeler ve hatalar değer olarak değerlendirilir ve bu nedenle "eksik sayısı" öğesine katkıda bulunmazlar.
Dağılım Dilimleri| Verilerin dağıtılması hakkında bir fikir sağlamak için her bir satışla yaklaşık değerler.
Ortalama| Sütunun aritmetik ortalaması veya ortalaması.
Standart sapma| Bu sütun verisinin dağılım veya değişim miktarının ölçümü.
Varyans| Bu sütunun verilerinin ne kadar yayıldığı, ortalama değerinden oluşan ölçüdür. 
Eğrilikleri| Bu sütun verilerinin normal bir dağılıma göre ne kadar farklı olduğunu gösteren ölçü.
Sıklık| Bu sütun verilerinin ne kadar süden bir normal dağıtımla karşılaştırıldığı ölçüdür.

## <a name="customize-featurization"></a>Özelleştirmeleri özelleştirme

**Korleştirme** formunda, otomatik özelliği etkinleştirebilir/devre dışı bırakabilir ve denemenizin otomatik hale getirme ayarlarını özelleştirebilirsiniz. Bu formu açmak için, [deneme oluşturma ve çalıştırma](#create-and-run-experiment) bölümünde 10. adıma bakın. 

Aşağıdaki tabloda, şu anda Studio aracılığıyla kullanılabilen özelleştirmeler özetlenmektedir. 

Sütun| Özelleştirme
---|---
Dahil | Eğitim için hangi sütunların ekleneceğini belirtir.
Özellik türü| Seçili sütun için değer türünü değiştirin.
Impute with| Verilerinizde hangi değerin eksik olduğunu belirlemek için değerleri seçin.

![Azure Machine Learning Studio özel özelliği](media/how-to-use-automated-ml-for-ml-models/custom-featurization.png)

## <a name="run-experiment-and-view-results"></a>Deneme çalıştırma ve sonuçları görüntüleme

Denemenizi çalıştırmak için **son** ' u seçin. Denemeyi hazırlama işlemi 10 dakika kadar sürebilir. Eğitim işlerinde her işlem hattının çalıştırılmasını tamamlamak fazladan 2-3 dakika daha sürebilir.

### <a name="view-experiment-details"></a>Deneme ayrıntılarını görüntüleme

**Ayrıntı Çalıştır** ekranı **Ayrıntılar** sekmesinde açılır. Bu ekranda, çalışma numarasının yanında bir durum çubuğu dahil olmak üzere deneme çalıştırmasının bir özeti gösterilir. 

**Modeller** sekmesi oluşturulan ve ölçüm puanına göre sıralanan model listesini içerir. Varsayılan olarak, seçilen ölçüme göre en yüksek puana sahip olan model listenin en üstünde yer alır. Eğitim işi daha fazla model denedikçe, bu modeller listeye eklenir. Bunu kullanarak, şimdiye kadar oluşturulan modeller için ölçümlerin hızlı bir karşılaştırmasını elde edebilirsiniz.

[![Çalışma ayrıntıları panosu](media/how-to-use-automated-ml-for-ml-models/run-details.png)](media/how-to-use-automated-ml-for-ml-models/run-details-expanded.png#lightbox)

### <a name="view-training-run-details"></a>Eğitim çalışma ayrıntılarını görüntüle

**Model** sekmesindeki model Özeti veya **ölçümler** sekmesindeki performans ölçümü grafikleri gibi eğitim çalıştırma ayrıntılarını görmek için tamamlanan modellerin herhangi birinde detaya gidin. [grafikler hakkında daha fazla bilgi edinin](how-to-understand-automated-ml.md).

[![Yineleme ayrıntıları](media/how-to-use-automated-ml-for-ml-models/iteration-details.png)](media/how-to-use-automated-ml-for-ml-models/iteration-details-expanded.png)

## <a name="deploy-your-model"></a>Modelinizi dağıtın

En iyi modeli belirledikten sonra yeni verilerle ilgili tahminde bulunmak için web hizmeti olarak dağıtabilirsiniz.

Otomatik ML, modeli kod yazmadan dağıtmanıza yardımcı olur:

1. Dağıtım için birkaç seçeneğiniz vardır. 

    + 1. seçenek: en iyi modeli, tanımladığınız ölçüm ölçütlerine göre dağıtın. 
        1. Deneme tamamlandıktan sonra, ekranın üst kısmındaki **Çalıştır 1** ' i seçerek üst çalışma sayfasına gidin. 
        1.  **En iyi model Özeti** bölümünde listelenen modeli seçin. 
        1. Pencerenin sol üst kısmında **Dağıt** ' ı seçin. 

    + 2. seçenek: belirli bir model yinelemesini bu deneyime dağıtmak Için.
        1. **Modeller** sekmesinden istediğiniz modeli seçin
        1. Pencerenin sol üst kısmında **Dağıt** ' ı seçin.

1. **Modeli dağıt** bölmesini doldurun.

    Alan| Değer
    ----|----
    Ad| Dağıtımınız için benzersiz bir ad girin.
    Açıklama| Bu dağıtımın ne için olduğunu daha iyi tanımlamak için bir açıklama girin.
    İşlem türü| Dağıtmak istediğiniz uç nokta türünü seçin: *Azure Kubernetes hizmeti (AKS)* veya *Azure Container Instance (acı)*.
    İşlem adı| *Yalnızca AKS Için geçerlidir:* Dağıtmak istediğiniz AKS kümesinin adını seçin.
    Kimlik doğrulamayı etkinleştir | Belirteç tabanlı veya anahtar tabanlı kimlik doğrulamasına izin vermek için seçin.
    Özel dağıtım varlıklarını kullanma| Kendi Puanlama betiğinizi ve ortam dosyanızı karşıya yüklemek istiyorsanız bu özelliği etkinleştirin. [Puanlama betikleri hakkında daha fazla bilgi edinin](how-to-deploy-and-where.md).

    >[!Important]
    > Dosya adları 32 karakterden oluşmalıdır ve alfasayısal karakterlerle başlamalı ve bitmelidir. Ad içinde kısa çizgi, alt çizgi, nokta ve alfasayısal karakterler kullanılabilir. Boşluk kullanılamaz.

    *Gelişmiş* menüsünde [veri toplama](how-to-enable-app-insights.md) ve kaynak kullanımı ayarları gibi varsayılan dağıtım özellikleri yer alır. Varsayılan değerleri değiştirmek için bu menüyü kullanabilirsiniz.

1. **Dağıt**'ı seçin. Dağıtımın tamamlanması yaklaşık 20 dakika sürer.
    Dağıtım başladıktan sonra **Model özeti** sekmesi görüntülenir. Dağıtımın ilerleme durumunu **Dağıtım durumu** bölümünde görebilirsiniz. 

Artık tahminde bulunmak için kullanabileceğiniz çalışan bir web hizmetiniz var! Hizmeti [Power BI'ın yerleşik Azure Machine Learning desteği](how-to-consume-web-service.md#consume-the-service-from-power-bi) ile sorgulayarak tahminleri test edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Bir Web hizmetini kullanmayı öğrenin](https://docs.microsoft.com/azure/machine-learning/how-to-consume-web-service).
* [Otomatik makine öğrenimi sonuçlarını anlayın](how-to-understand-automated-ml.md).
* [Otomatik makine öğrenimi ve Azure Machine Learning hakkında daha fazla bilgi edinin](concept-automated-ml.md) .
