---
title: Portal 'da denemeleri oluşturma ve araştırma
titleSuffix: Azure Machine Learning service
description: Portalda otomatik makine öğrenimi denemeleri oluşturma ve yönetme hakkında bilgi edinin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: cgronlun
author: tsikiksr
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 05/02/2019
ms.openlocfilehash: 5eb3e94ff65e8a8b74f357a4cb8a517fd3837c5a
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67871821"
---
# <a name="create-and-explore-automated-machine-learning-experiments-in-the-azure-portal-preview"></a>Azure portal (Önizleme) içinde otomatik makine öğrenimi denemeleri oluşturma ve araştırma

 Bu makalede, tek bir kod satırı olmadan Azure portal otomatik makine öğrenimi denemeleri oluşturmayı, çalıştırmayı ve araştırmanızı öğrenirsiniz. Otomatik makine öğrenimi, belirli verileriniz için kullanılacak en iyi algoritmayı seçme sürecini otomatikleştirir, böylece bir makine öğrenimi modelini hızlıca oluşturabilirsiniz. [Otomatik makine öğrenimi hakkında daha fazla bilgi edinin](concept-automated-ml.md).

 Daha fazla kod tabanlı bir deneyim tercih ediyorsanız, [otomatik makine öğrenimi denemeleri](how-to-configure-auto-train.md) [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)ile Python 'da da yapılandırabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning Service 'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* Bir Azure Machine Learning hizmeti çalışma alanı. Bkz. [Azure Machine Learning hizmet çalışma alanı oluşturma](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace).

## <a name="get-started"></a>başlarken

Çalışma alanınızın sol bölmesine gidin. Yazma (Önizleme) bölümünün altındaki otomatik Machine Learning seçin.

![Azure portal gezinti bölmesi](media/how-to-create-portal-experiments/nav-pane.png)

 Otomatik Machine Learning ile ilk kez denemeleri, şunları görürsünüz:

![Azure portal deneme giriş sayfası](media/how-to-create-portal-experiments/landing-page.png)

Aksi takdirde, SDK ile oluşturulanlar da dahil olmak üzere otomatik makine öğrenimi panonuzun tüm otomatik makine öğrenimi denemeleri bir genel bakışı olduğunu göreceksiniz. Burada, çalıştırmaları Tarih, deneme adı ve çalışma durumu ile filtreleyebilir ve keşfedebilirsiniz.

![Azure portal deneme panosu](media/how-to-create-portal-experiments/dashboard.png)

## <a name="create-an-experiment"></a>Deneme oluşturma

Aşağıdaki formu doldurmak için deneme Oluştur düğmesini seçin.

![Deneme formu oluştur](media/how-to-create-portal-experiments/create-exp-name-compute.png)

1. Deneme adınızı girin.

1. Veri profili oluşturma ve eğitim işi için bir işlem seçin. Mevcut hesaplamalarınızın listesi açılan menüde bulunabilir. Yeni bir işlem oluşturmak için adım 3 ' teki yönergeleri izleyin.

1. Aşağıdaki bölmeyi açmak için yeni bir işlem Oluştur düğmesini seçin ve bu deneme için işlem bağlamını yapılandırın.

    ![Deneme için yeni işlem oluştur](media/how-to-create-portal-experiments/create-new-compute.png)

    Alan|Açıklama
    ---|---
    İşlem adı| İşlem bağlamını tanımlayan benzersiz bir ad girin.
    Sanal makine boyutu| İşlem için sanal makine boyutunu seçin.
    Ek ayarlar| *En az düğüm*: İşlem için en az düğüm sayısını girin. AML işlem için düğüm sayısı alt sınırı 0 ' dır. Veri profilini oluşturmayı etkinleştirmek için, 1 veya daha fazla düğüme sahip olmanız gerekir. <br> *En fazla düğüm*: İşlem için en fazla düğüm sayısını girin. Varsayılan değer bir AML Işlem için 6 düğümünüz.

      Yeni işlem oluşturma işlemine başlamak için **Oluştur**' u seçin. Bu işlem birkaç dakika sürebilir.

      >[!NOTE]
      > İşlem adınız, seçtiğiniz işlem *profil oluşturma*/oluşturma işleminin etkin olup olmadığını gösterir. (Veri profili oluşturma hakkında daha fazla bilgi için bkz. 7B).

1. Verileriniz için bir depolama hesabı seçin. Genel Önizleme yalnızca yerel dosya yüklemelerini ve Azure Blob depolama hesaplarını destekler.

1. Bir depolama kapsayıcısı seçin.

1. Depolama kapsayıcıınızdan bir veri dosyası seçin veya yerel bilgisayarınızdaki bir dosyayı kapsayıcıya yükleyin.

    ![Deneme için veri dosyası seçin](media/how-to-create-portal-experiments/select-file.png)

1. Bu deneme için verilerinizi daha fazla yapılandırmak üzere önizleme ve profil sekmelerini kullanın.

    1. Önizleme sekmesinde, verilerinizin üstbilgiler içerip içermeyeceğini belirtin ve her bir özellik sütunundaki **dahil edilen** anahtar düğmelerini kullanarak eğitim için özellikleri (sütunlar) seçin.

        ![Veri önizleme](media/how-to-create-portal-experiments/data-preview.png)

    1. Profil sekmesinde, her birinin dağıtım, tür ve Özet istatistikleri (ortalama, ortanca, en fazla/en düşük vb.) ile [veri profilini](#profile) de görüntüleyebilirsiniz.

        ![Veri profili sekmesi](media/how-to-create-portal-experiments/data-profile.png)

        >[!NOTE]
        > İşlem **içeriğiniz profil oluşturma** etkinleştirilmemişse aşağıdaki hata iletisi görüntülenir: *Veri profili oluşturma yalnızca zaten çalışmakta olan işlem hedefleri için kullanılabilir*.

1. Eğitim işi türünü seçin: sınıflandırma, regresyon veya tahmin.

1. Hedef sütun ' u seçin. Tahmin yapmak istediğiniz sütun.

1. Tahmin için:
    1. Zaman sütununu seçin: Bu sütun kullanılacak saat verilerini içerir.
    1. Tahmin ufkunu seçin: Modelin kaç zaman birimi (dakika/saat/gün/hafta/ay/yıl) gelecek şekilde tahmin edemeyeceğini belirtin. Daha sonra modelin daha iyi tahmin edilmesi gerektiğinde, daha az doğru olacaktır. [Tahmin ve tahmin ufku hakkında daha fazla bilgi edinin](how-to-auto-train-forecast.md).

1. Seçim Gelişmiş ayarlar: eğitim işini daha iyi denetleyebilmeniz için kullanabileceğiniz ek ayarlar.

    Gelişmiş ayarlar|Açıklama
    ------|------
    Birincil ölçüm| Modelinize Puanlama için kullanılan ana ölçüm. [Model ölçümleri hakkında daha fazla bilgi edinin](how-to-configure-auto-train.md#explore-model-metrics).
    Çıkış kriterleri| Bu ölçütlerden herhangi biri karşılandığında, eğitim işi tam tamamlanmadan önce sonlanır. <br> *Eğitim iş süresi (dakika)* : Eğitim işinin ne kadar süreyle çalışmasına izin verme.  <br> *En fazla yineleme sayısı*: Eğitim işinde sınanacak maksimum işlem hattı sayısı (yineleme). İş, belirtilen sayıda yinelemeden daha fazla çalıştırmayacak. <br> *Ölçüm puan eşiği*:  Tüm işlem hatları için en düşük ölçüm puanı. Bu, ulaşmak istediğiniz tanımlı bir hedef ölçüsünün olması durumunda eğitim işinde gerekli olandan daha fazla zaman harcamamanızı sağlar.
    Ön| Otomatik makine öğrenimi tarafından gerçekleştirilen ön işleme özelliğini etkinleştirmek veya devre dışı bırakmak için seçin. Ön işleme, yapay özellikler oluşturmak için otomatik veri temizleme, hazırlama ve dönüştürme içerir. [Ön işleme hakkında daha fazla bilgi edinin](#preprocess).
    Doğrulama| Eğitim işinde kullanmak için çapraz doğrulama seçeneklerinden birini seçin. [Çapraz doğrulama hakkında daha fazla bilgi edinin](how-to-configure-auto-train.md).
    Eşzamanlılık| Çok çekirdekli işlem kullanırken kullanmak istediğiniz çok çekirdekli limitleri seçin.
    Engellenen algoritma| Eğitim işinden dışlamak istediğiniz algoritmaları seçin.

   ![Gelişmiş ayarlar formu](media/how-to-create-portal-experiments/advanced-settings.png)

> [!NOTE]
> Alanlar hakkında daha fazla bilgi için bilgi araç ipucu ' na tıklayın.

<a name="profile"></a>

### <a name="data-profiling"></a>Veri profili oluşturma

Veri ayarlanmış olup olmadığını doğrulamak için veri kümesi genelinde çok sayıda Özet istatistiği alabilir. Sayısal olmayan sütunlarda yalnızca min, Max ve Error Count gibi temel istatistikler bulunur. Sayısal sütunlarda, istatistiksel ve tahmini quantiles değerlerini de gözden geçirebilirsiniz. Veri profiliniz özellikle şunları içerir:

* **Özellik**: özetlenen sütunun adı.

* **Profil**: gösterilen türü temel alan satır içi görselleştirme. Örneğin, dizeler, Boole değerleri ve tarihler değer sayılarına sahip olacaktır, ancak Ondalıklar (Numerics) de yaklaşık histogramlar olur. Bu, verilerin dağıtımını hızlı bir şekilde anlayabilmeniz için size izin verir.

* **Tür dağıtımı**: bir sütun içindeki türlerin satır içi değer sayısı. Null değerler kendi türlerdir, bu nedenle bu görselleştirme tek veya eksik değerleri algılamak için yararlıdır.

* **Tür**: sütunun Çıkarsanan türü. Olası değerler şunlardır: dizeler, Boole değerleri, tarihler ve ondalıklar.

* **Min**: sütunun minimum değeri. Boş girdiler, türü bir devralınan sıralamaya (örn. Boolean) sahip olmayan özellikler için görünür.

* **Max**: sütunun en büyük değeri. "Enaz" gibi boş girişler, ilgisiz türler içeren özellikler için görünür.

* **Sayı**: sütundaki eksik ve eksik olmayan girdilerin toplam sayısı.

* **Eksik sayı**: sütundaki girdi sayısı eksik. Boş dizelerin ve hataların değer olarak değerlendirildiğine ve bu nedenle "eksik saymayacak" öğesine katkıda bulunmayacağını unutmayın.

* **Quantiles** (0,1, 1, 5, 25, 50, 75, 95, 99 ve 99,9% aralıklarında): her bir satışla, verilerin dağıtımına ilişkin bir fikir sağlamak için yaklaşık değerler. İlgisiz türler içeren özellikler için boş girdiler görüntülenir.

* **Ortalama**: sütunun aritmetik ortalaması. İlgisiz türler içeren özellikler için boş girdiler görüntülenir.

* **Standart sapma**: sütunun standart sapması. İlgisiz türler içeren özellikler için boş girdiler görüntülenir.

* **Varyans**: sütunun varyansı. İlgisiz türler içeren özellikler için boş girdiler görüntülenir.

* **Çarpıklık**: sütunun çarpıklığı. İlgisiz türler içeren özellikler için boş girdiler görüntülenir.

* **Basıklık**: sütunun basıklık. İlgisiz türler içeren özellikler için boş girdiler görüntülenir.

<a name="preprocess"></a>

### <a name="advanced-preprocessing"></a>Gelişmiş ön işleme

Denemeleri 'nizi yapılandırırken gelişmiş ayarı `Preprocess`etkinleştirebilirsiniz. Bunun yapılması, aşağıdaki veri ön işleme ve korleştirme adımlarının otomatik olarak gerçekleştirildiği anlamına gelir.

|Ön&nbsp;işleme adımları| Açıklama |
| ------------- | ------------- |
|Yüksek bir kardinalite veya herhangi bir fark özellik bırakın|Tüm değerleri eksik olan özellikler de dahil olmak üzere eğitim ve doğrulama kümelerinden bunları bırakın, tüm satırlarda veya çok yüksek kardinalite (örneğin, karma, kimlik veya GUID) ile aynı değeri kullanın.|
|Eksik değerleri impute|Sayısal özellikler için sütunundaki değerleri ortalama olan ımpute.<br/><br/>Kategorik özellikler için en sık değer içeren ımpute.|
|Ek özellikler oluşturma|Tarih saat özellikleri için: Yıl, ay, gün, haftanın günü, yılın günü, çeyrek, yılın haftası, saat, dakika, saniye.<br/><br/>Metin özellikleri için: Tekli gram, Çift gram ve üçlü karakter-gram temelinde Dönem sıklığı.|
|Dönüştür ve kodla |Birkaç benzersiz değer içeren sayısal özellikler kategorik özelliklere dönüştürülür.<br/><br/>Düşük kardinalite kategorik için tek bir etkin kodlama gerçekleştirilir; yüksek kardinalite, tek bir sıcak karma kodlama için.|
|Sözcük katıştırlamaları|Metin belirteçlerinin vektörlerini, önceden eğitilen bir model kullanarak tümce vektörlerine dönüştüren metin korleştirici. Belge özellik vektörü oluşturmak için bir belgedeki her bir sözcüğün katıştırma vektörü birlikte toplanır.|
|Hedef kodlamalar|Kategorik özellikler için, her bir kategoriyi gerileme sorunları için Ortalama hedef değeriyle ve sınıflandırma sorunları için her bir sınıfın sınıf olasılığa eşler. Sıklık tabanlı ağırlığa ve k katlamalı çapraz doğrulama, seyrek veri kategorilerinin neden olduğu eşlemenin ve gürültü üzerine gitmek için geçerlidir.|
|Metin hedefi kodlaması|Metin girişi için, her bir sınıfın olasılığını oluşturmak için kelimeleri olan bir yığılmış Doğrusal model kullanılır.|
|Kanıt ağırlığı (WoE)|, Kategorik sütunların bağıntısı olarak hedef sütuna bir ölçü olarak, WoE hesaplar. Sınıf içi ve sınıf dışı olasılıkların oranının günlüğü olarak hesaplanır. Bu adım, her sınıf için bir sayısal özellik sütunu verir ve eksik değerler ve aykırı değer işleme gereksinimini ortadan kaldırır.|
|Küme uzaklığı|K. a, tüm sayısal sütunlarda kümeleme modeli anlamına gelir.  Her bir örneğin her bir kümenin centroıd değerine her bir örnek arasındaki mesafeyi içeren, her küme için yeni bir sayısal özellik olan yeni özellik çıkışları.|

## <a name="run-experiment-and-view-results"></a>Deneme çalıştırma ve sonuçları görüntüleme

Denemeyi çalıştırmak için Başlat ' a tıklayın. Deneme hazırlama işlemi birkaç dakika sürer.

### <a name="view-experiment-details"></a>Deneme ayrıntılarını görüntüle

Deneme hazırlığı aşaması tamamlandıktan sonra, ayrıntıları Çalıştır ekranını görürsünüz. Bu size oluşturulan modellerin tam bir listesini sağlar. Varsayılan olarak, parametrelerinizi temel alarak en yüksek düzeyde puan veren model listenin en üstünde yer alır. Eğitim işi daha fazla model denediğinde yineleme listesine ve grafiğe eklenir. Şimdiye kadar üretilen modellerin ölçümlerinin hızlı bir şekilde karşılaştırılmasını sağlamak için yineleme grafiğini kullanın.

Eğitim işleri, her işlem hattının çalışmayı tamamlaması biraz zaman alabilir.

![Çalışma ayrıntıları panosu](media/how-to-create-portal-experiments/run-details.png)

### <a name="view-training-run-details"></a>Eğitim çalışma ayrıntılarını görüntüle

Performans ölçümleri ve dağıtım grafikleri gibi eğitim çalıştırma ayrıntılarını görmek için çıkış modellerinin herhangi birinin detayına gidin. [Grafikler hakkında daha fazla bilgi edinin](how-to-track-experiments.md#understanding-automated-ml-charts).

![Yineleme ayrıntıları](media/how-to-create-portal-experiments/iteration-details.png)

## <a name="deploy-model"></a>Model dağıtma

En iyi modeli elde etmeniz durumunda, yeni verileri tahmin etmek için bunu bir Web hizmeti olarak dağıtmanız zaman alır.

Otomatikleştirilmiş ML, kodu yazmadan modeli dağıtmanıza yardımcı olur:

1. Dağıtım için birkaç seçeneğiniz vardır. 
    1. Deneme için ayarladığınız ölçüm ölçütlerine göre en iyi modeli dağıtmak istiyorsanız, **ayrıntıları Çalıştır** sayfasından **en iyi modeli dağıt** ' ı seçin.

        ![Modeli dağıt düğmesi](media/how-to-create-portal-experiments/deploy-model-button.png)

    1. Belirli bir model yinelemesini dağıtmak istiyorsanız, modelin detayına gidin ve özel çalışma ayrıntısı sayfasını açın ve **modeli dağıt**' ı seçin.

        ![Modeli dağıt düğmesi](media/how-to-create-portal-experiments/deploy-model-button2.png)

1. İlk adım, modeli hizmete kaydettirebilir. "Modeli Kaydet" i seçin ve kayıt işleminin tamamlanmasını bekleyin.

    ![Model dağıtma dikey penceresi](media/how-to-create-portal-experiments/deploy-model-blade.png)

1. Model kaydedildikten sonra, dağıtım sırasında kullanılacak Puanlama betiğini (scoring.py) ve ortam betiğini (Condadenv. yıml) indirebilirsiniz.

1. Puanlama betiği ve ortam betiği indirildiğinde sol gezinti bölmesinin **varlıklar** dikey penceresine gidin ve **modeller**' ı seçin.

    ![Gezinti Bölmesi modelleri](media/how-to-create-portal-experiments/nav-pane-models.png)

1. Kaydettiğiniz modeli seçin ve "görüntü oluştur" ' u seçin.

    Aşağıdaki biçimde çalışma KIMLIĞI, yineleme numarası dahil olmak üzere modeli açıklamasına göre tanımlayabilirsiniz: *< Run_ID > _ < Iteration_number > _Model*

    ![Modelde Görüntü oluştur](media/how-to-create-portal-experiments/model-create-image.png)

1. Görüntü için bir ad girin. 
1. Daha önce indirdiğiniz Puanlama dosyasını (scoring.py) karşıya yüklemek için "Puanlama dosyası" kutusunun yanındaki **Git düğmesini seçin** .

1. Daha önce indirdiğiniz ortam dosyasını (Condadenv. yıml) karşıya yüklemek için "Conda File" kutusunun yanındaki **göz at** düğmesini seçin.

    Kendi Puanlama betiğinizi ve Conda dosyanızı kullanabilir ve ek dosyaları karşıya yükleyebilirsiniz. [Puanlama betiği hakkında daha fazla bilgi edinin](how-to-deploy-and-where.md#script).

      >[!Important]
      > Dosya adları 32 karakterden oluşmalıdır ve alfasayısal karakterlerle başlamalı ve bitmelidir. Arasına tire, alt çizgi, nokta ve alfasayısal karakterler içerebilir. Boşluklara izin verilmez.

    ![Görüntü oluştur](media/how-to-create-portal-experiments/create-image.png)

1. Görüntü oluşturmayı başlatmak için "Oluştur" düğmesini seçin. Bunun tamamlanması birkaç dakika sürer, bu işlem tamamlandığında üstteki çubukta bir ileti görürsünüz.
1. "Görüntüler" sekmesine gidin, dağıtmak istediğiniz görüntünün yanındaki onay kutusunu işaretleyin ve "dağıtım oluştur" ' u seçin. [Dağıtımlar hakkında daha fazla bilgi edinin](how-to-deploy-and-where.md).

    Dağıtım için 2 seçenek vardır.
     + Azure Container Instance (acı)-Bu, ölçekte işlemsel dağıtım yerine test amaçlı daha fazla kullanılır. _CPU Reserve kapasitesi_için en az bir çekirdek ve _bellek ayırma kapasitesi_ için en az bir gigabayt (GB) değerlerini doldurduğunuzdan emin olun
     + Azure Kubernetes hizmeti (AKS))-Bu seçenek ölçekli dağıtım içindir. AKS tabanlı bir işlem için hazırlık yapmanız gerekir.

     ![Yansımasını Dağıtım oluşturma](media/how-to-create-portal-experiments/images-create-deployment.png)

1. İşiniz bittiğinde **Oluştur**’u seçin. Modelin dağıtımı, her bir işlem hattının çalışmayı tamamlaması birkaç dakika sürebilir.

1. İşte bu kadar! Tahmin oluşturmak için işlemsel bir Web hizmetiniz vardır.

## <a name="next-steps"></a>Sonraki adımlar

* [Otomatik makine öğrenimi ve Azure Machine Learning hakkında daha fazla bilgi edinin](concept-automated-ml.md) .
* [Bir Web hizmetini kullanmayı öğrenin](https://docs.microsoft.com/azure/machine-learning/service/how-to-consume-web-service).
