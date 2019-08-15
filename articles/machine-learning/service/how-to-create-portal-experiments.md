---
title: Makine öğrenimi modellerini derlemek ve dağıtmak için otomatik ML kullanma
titleSuffix: Azure Machine Learning service
description: Azure portal otomatik makine öğrenimi denemeleri oluşturun, yönetin ve dağıtın
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: nibaccam
author: tsikiksr
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 08/02/2019
ms.openlocfilehash: 2f6d45613120d02dd96a9fe0a14ce388d20cf0c6
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68990580"
---
# <a name="create-explore-and-deploy-automated-machine-learning-experiments-in-the-azure-portal-preview"></a>Azure portal (Önizleme) içinde otomatik makine öğrenimi denemeleri oluşturma, araştırma ve dağıtma

 Bu makalede, tek bir kod satırı olmadan Azure portal otomatik makine öğrenimi denemeleri oluşturmayı, keşfetmeye ve dağıtmayı öğreneceksiniz. Otomatik makine öğrenimi, belirli verileriniz için kullanılacak en iyi algoritmayı seçme sürecini otomatikleştirir, böylece bir makine öğrenimi modelini hızlıca oluşturabilirsiniz. [Otomatik makine öğrenimi hakkında daha fazla bilgi edinin](concept-automated-ml.md).

 Daha fazla kod tabanlı bir deneyim tercih ediyorsanız, [otomatik makine öğrenimi denemeleri](how-to-configure-auto-train.md) [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)ile Python 'da da yapılandırabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning Service 'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* Bir Azure Machine Learning hizmeti çalışma alanı. Bkz. [Azure Machine Learning hizmet çalışma alanı oluşturma](how-to-manage-workspace.md).

## <a name="get-started"></a>başlarken

Çalışma alanınızın sol bölmesine gidin. Yazma (Önizleme) bölümünün altındaki otomatik Machine Learning seçin.

![Azure portal gezinti bölmesi](media/how-to-create-portal-experiments/nav-pane.png)

 Bu denemeleri ilk kez varsa **otomatik Machine Learning ekranına hoş geldiniz** ' i görürsünüz. 

Aksi halde, SDK ile oluşturulanlar da dahil olmak üzere otomatik makine öğrenimi panonuzun tüm otomatik makine öğrenimi denemeleri hakkında genel bir bakış içeren bir genel bakış görürsünüz. Burada, çalıştırmaları Tarih, deneme adı ve çalışma durumu ile filtreleyebilir ve keşfedebilirsiniz.

## <a name="create-an-experiment"></a>Deneme oluşturma

**Deneme oluştur** ' u seçin ve **Yeni bir otomatik makine öğrenimi oluşturma deneme** formu doldurun.

1. Benzersiz bir deneme adı girin.

1. Veri profili oluşturma ve eğitim işi için bir işlem seçin. Mevcut hesaplamalarınızın listesi açılan menüde bulunabilir. Yeni bir işlem oluşturmak için adım 3 ' teki yönergeleri izleyin.

1. Bu deneme için işlem bağlamını yapılandırmak üzere **Yeni bir Işlem oluştur** ' u seçin.

    Alan|Açıklama
    ---|---
    İşlem adı| İşlem bağlamını tanımlayan benzersiz bir ad girin.
    Sanal makine boyutu| İşlem için sanal makine boyutunu seçin.
    Ek ayarlar| *En az düğüm*: İşlem için en az düğüm sayısını girin. AML işlem için düğüm sayısı alt sınırı 0 ' dır. Veri profilini oluşturmayı etkinleştirmek için, 1 veya daha fazla düğüme sahip olmanız gerekir. <br> *En fazla düğüm*: İşlem için en fazla düğüm sayısını girin. Varsayılan değer bir AML Işlem için 6 düğümünüz.

      **Oluştur**’u seçin. Yeni bir işlem oluşturmak birkaç dakika sürebilir.

      >[!NOTE]
      > İşlem adınız, seçtiğiniz işlem *profil oluşturma*/oluşturma işleminin etkin olup olmadığını gösterir. (Veri profili oluşturma hakkında daha fazla bilgi için bkz. 7B).

1. Verileriniz için bir depolama hesabı seçin. 

1. Bir depolama kapsayıcısı seçin.

1. Depolama kapsayıcıınızdan bir veri dosyası seçin veya yerel bilgisayarınızdaki bir dosyayı kapsayıcıya yükleyin. Genel Önizleme yalnızca yerel dosya yüklemelerini ve Azure Blob depolama hesaplarını destekler.
    >[!Important]
    > Eğitim verileri için gereksinimler:
    >* Verilerin tablolu biçimde olması gerekir.
    >* Tahmin etmek istediğiniz değer (hedef sütun) verilerde bulunmalıdır.

    [![Veri dosyası seç](media/tutorial-1st-experiment-automated-ml/select-data-file.png)](media/tutorial-1st-experiment-automated-ml/select-data-file-expanded.png#lightbox)

1. Bu deneme için verilerinizi daha fazla yapılandırmak üzere önizleme ve profil sekmelerini kullanın.

    1. **Önizleme** sekmesinde, verilerinizin üstbilgiler içerip içermeyeceğini belirtin ve her bir özellik sütunundaki **dahil edilen** anahtar düğmelerini kullanarak eğitim için özellikleri (sütunlar) seçin.

    1. **Profil** sekmesinde, her birinin dağıtım, tür ve Özet istatistikleri (ortalama, ortanca, en fazla/en düşük vb.) ile [veri profilini](#profile) de görüntüleyebilirsiniz.

        >[!NOTE]
        > İşlem içeriğiniz profil oluşturma etkinleştirilmemişse aşağıdaki hata iletisi görüntülenir: *Veri profili oluşturma yalnızca zaten çalışmakta olan işlem hedefleri için kullanılabilir*.

1. Eğitim işi türünü seçin: sınıflandırma, regresyon veya tahmin.

1. Hedef sütunu seçin; Bu, tahminlerde yapmak istediğiniz sütundur.

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
    Eş Zamanlılık| Çok çekirdekli işlem kullanırken kullanmak istediğiniz çok çekirdekli limitleri seçin.
    Engellenen algoritma| Eğitim işinden dışlamak istediğiniz algoritmaları seçin.

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
Type|Sütunun Çıkarsanan türü. Olası değerler şunlardır: dizeler, Boole değerleri, tarihler ve ondalıklar.
Min| Sütunun minimum değeri. Boş girdiler, türü bir devralınan sıralamaya (örn. Boolean) sahip olmayan özellikler için görünür.
Maks| Sütunun en büyük değeri. 
Count| Sütundaki eksik ve eksik olmayan girdilerin toplam sayısı.
Eksik sayı yok| Sütundaki eksik girdi sayısı. Boş dizeler ve hatalar değer olarak değerlendirilir ve bu nedenle "eksik sayısı" öğesine katkıda bulunmazlar.
Quantiles| Verilerin dağıtılması hakkında bir fikir sağlamak için her bir satışla yaklaşık değerler.
Ortalama| Sütunun aritmetik ortalaması veya ortalaması.
Standart sapma| Bu sütun verisinin dağılım veya değişim miktarının ölçümü.
Varyans| Bu sütunun verilerinin ne kadar yayıldığı, ortalama değerinden oluşan ölçüdür. 
Komutunu| Bu sütun verilerinin normal bir dağılıma göre ne kadar farklı olduğunu gösteren ölçü.
Basıklık| Bu sütun verilerinin ne kadar süden bir normal dağıtımla karşılaştırıldığı ölçüdür.

<a name="preprocess"></a>

## <a name="advanced-preprocessing-options"></a>Gelişmiş ön işleme seçenekleri

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

Denemenizi çalıştırmak için **Başlat** ' ı seçin. Deneme hazırlama işlemi birkaç dakika sürer.

### <a name="view-experiment-details"></a>Deneme ayrıntılarını görüntüle

Deneme hazırlığı aşaması tamamlandıktan sonra, ayrıntıları Çalıştır ekranının doldurulmaya başlayacağı görürsünüz. Bu ekran size oluşturulan modellerin tam bir listesini sağlar. Varsayılan olarak, seçili ölçüm temelinde en yüksek düzeyde puan veren model listenin en üstünde yer alır. Eğitim işi daha fazla model denediğinde yineleme listesine ve grafiğe eklenir. Şimdiye kadar üretilen modellerin ölçümlerinin hızlı bir şekilde karşılaştırılmasını sağlamak için yineleme grafiğini kullanın.

Eğitim işleri, her işlem hattının çalışmayı tamamlaması biraz zaman alabilir.

[![Çalışma ayrıntıları panosu](media/how-to-create-portal-experiments/run-details.png)](media/how-to-create-portal-experiments/run-details-expanded.png#lightbox)

### <a name="view-training-run-details"></a>Eğitim çalışma ayrıntılarını görüntüle

Performans ölçümleri ve dağıtım grafikleri gibi eğitim çalıştırma ayrıntılarını görmek için çıkış modellerinin herhangi birinin detayına gidin. [Grafikler hakkında daha fazla bilgi edinin](how-to-understand-automated-ml.md).

![Yineleme ayrıntıları](media/how-to-create-portal-experiments/iteration-details.png)

## <a name="deploy-your-model"></a>Modelinizi dağıtın

En iyi modeli elde etmeniz durumunda, yeni verileri tahmin etmek için bunu bir Web hizmeti olarak dağıtmanız zaman alır.

Otomatikleştirilmiş ML, kodu yazmadan modeli dağıtmanıza yardımcı olur:

1. Dağıtım için birkaç seçeneğiniz vardır. 

    + Seçenek 1: En iyi modeli dağıtmak için (tanımladığınız ölçüm ölçütlerine göre), ayrıntıları Çalıştır sayfasından En Iyi modeli Dağıt ' ı seçin.

    + Seçenek 2: Bu deneyime ait belirli bir model yinelemesini dağıtmak için, modelin detayına gidin ve çalışma ayrıntısı sayfasını açın ve modeli Dağıt ' ı seçin.
1. **Modeli dağıt** bölmesini doldurun,

    Alan| Value
    ----|----
    Dağıtım adı| Dağıtımınız için benzersiz bir ad girin.
    Dağıtım açıklaması| Bu dağıtımın ne için olduğunu daha iyi tanımlamak için bir açıklama girin.
    Puanlama betiği| Kendi Puanlama dosyanızı AutoGenerate veya karşıya yükleyin. [Puanlama betiği hakkında daha fazla bilgi edinin](how-to-deploy-and-where.md#script)
    Ortam betiği| Kendi ortam dosyanızı AutoGenerate veya karşıya yükleyin.
    >[!Important]
    > Dosya adları 32 karakterden oluşmalıdır ve alfasayısal karakterlerle başlamalı ve bitmelidir. Arasına tire, alt çizgi, nokta ve alfasayısal karakterler içerebilir. Boşluklara izin verilmez.

1. **Dağıt**'ı seçin. Dağıtımın tamamlanması yaklaşık 20 dakika sürebilir.

    Dağıtım başarıyla tamamlandığında aşağıdaki ileti görünür.

    ![Dağıtım Tamam](media/tutorial-1st-experiment-automated-ml/deploy-complete-status.png) 

Artık tahmin oluşturmak için işlemsel bir Web hizmetiniz vardır!

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Machine Learning ilk OTOMATIK ml denemenizin oluşturulması için uçtan uca öğreticiyi](tutorial-first-experiment-automated-ml.md)deneyin. 
* [Otomatik makine öğrenimi ve Azure Machine Learning hakkında daha fazla bilgi edinin](concept-automated-ml.md) .
* [Otomatik makine öğrenimi sonuçlarını anlayın](how-to-understand-automated-ml.md).
* [Bir Web hizmetini kullanmayı öğrenin](https://docs.microsoft.com/azure/machine-learning/service/how-to-consume-web-service).
