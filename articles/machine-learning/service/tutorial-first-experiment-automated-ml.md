---
title: İlk otomatik makine öğrenimi denemenizi oluşturun
titleSuffix: Azure Machine Learning service
description: Azure portal otomatik makine öğrenimi ile bir sınıflandırma modelini eğitme ve dağıtmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: tsikiksr
author: tsikiksr
ms.reviewer: nibaccam
ms.date: 07/23/2019
ms.openlocfilehash: 30dc731efdb6016f505b7a16860e0cb9c6480333
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68716590"
---
# <a name="tutorial-use-automated-machine-learning-to-train-and-deploy-your-first-classification-model-preview"></a>Öğretici: Otomatik makine öğrenimini kullanarak ilk sınıflandırma modelinizi eğitme ve dağıtma (Önizleme)

Bu öğreticide, Azure portal ilk otomatik makine öğrenimi denemenizi oluşturmayı öğreneceksiniz. Bu örnek, bir istemcinin bankaya ait bir terime yatırıp abone olup olmayacağını tahmin etmek için bir sınıflandırma modeli oluşturur. 

Hizmetin otomatik makine öğrenimi özelliklerini ve Azure portal kullanarak otomatik makine öğrenimi sürecini başlatın ve algoritma seçimi ve hiper parametre ayarı sizin için gerçekleşir. Otomatik makine öğrenimi tekniği, tek bir kod satırı yazmak zorunda kalmadan, kriterinize göre en iyi modeli bulana kadar birçok algoritma ve hiper parametre kombinasyonu üzerinde yinelenir.

Bu öğreticide, aşağıdaki görevleri öğreneceksiniz:

> [!div class="checklist"]
> * Azure Machine Learning bir hizmet çalışma alanı yapılandırın.
> * Deneme oluşturma.
> * Bir sınıflandırma modelini otomatik eğitme.
> * Eğitim çalışma ayrıntılarını görüntüleyin.
> * Model dağıtma.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning Service 'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* **Bankmarketing_train. csv** veri dosyası. İndirdiği [burada](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv).

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="create-an-experiment"></a>Deneme oluşturma

1. Çalışma alanınızın sol bölmesine gidin. **Yazma (Önizleme)** bölümünün altındaki **Otomatik Machine Learning** seçin.

    ![Azure portal gezinti bölmesi](media/tutorial-1st-experiment-automated-ml/nav-pane.png)

    Otomatik Machine Learning ilk denemenizin olduğu **Için otomatik Machine Learning ekranına hoş geldiniz** ' i görürsünüz. 

1.  **Deneme oluştur**' u seçin. Ardından deneme adı olarak **My-1-oto ml-deney** i girin.

1. **Yeni Işlem oluştur** ' u seçin ve işlem bağlamını bu deneme için yapılandırın.

    Alan| Value
    ---|---
    İşlem adı| İşlem bağlamını tanımlayan benzersiz bir ad girin. Bu örnekte, **oto ml-COMPUTE**kullanıyoruz.
    Sanal makine boyutu| İşlem için sanal makine boyutunu seçin. **Standard_DS12_V2**kullanıyoruz.
    Ek ayarlar| *En az düğüm*: 1. Veri profilini oluşturmayı etkinleştirmek için, 1 veya daha fazla düğüme sahip olmanız gerekir. <br> *En fazla düğüm*: 6. 

    Yeni işlem oluşturma işlemine başlamak için **Oluştur**' u seçin. Bu işlem birkaç dakika sürebilir. 

    Oluşturma işlemi tamamlandıktan sonra, açılan listeden yeni işlem ' ı seçin ve ardından **İleri**' yi seçin.

1. Bu öğretici için, varsayılan depolama hesabını ve yeni hesaplamanıza göre oluşturulan kapsayıcıyı kullanırız. Bu otomatik olarak formunda doldurulur.

1. Yerel bilgisayarınızdan **bankmarketing_train. csv** dosyasını seçmek Için **karşıya yükle** ' yi seçin ve varsayılan kapsayıcıya yükleyin. Genel Önizleme yalnızca yerel dosya yüklemelerini ve Azure Blob depolama hesaplarını destekler. Karşıya yükleme tamamlandığında, listeden dosyayı seçin. 

    [![Veri dosyası seç](media/tutorial-1st-experiment-automated-ml/select-data-file.png)](media/tutorial-1st-experiment-automated-ml/select-data-file-expanded.png#lightbox)

1. **Önizleme** sekmesi, bu deneme için verilerimizi daha fazla yapılandırmamızı sağlar.

    Önizleme sekmesinde, verilerin üstbilgiler içerdiğini belirtin. Hizmet, eğitimin tüm özelliklerini (sütunları) dahil etmek için varsayılan değerdir. Bu örnekte, sağa kaydırın ve **day_of_week** özelliğini **yoksayın** .

    ![Önizleme sekmesi yapılandırması](media/tutorial-1st-experiment-automated-ml/preview-tab-config.gif)


    >[!NOTE]
    > Veri profili oluşturma, 0 minimum düğümü olan hesaplar ile kullanılamaz.

1. Tahmin görevi olarak **Sınıflandırmayı** seçin.

1. Tahmin yapmak istediğimiz sütunu hedef sütun olarak **y** ' yi seçin. Bu sütun, istemcinin depozito bir terime abone olup olmadığını gösterir.

1. **Gelişmiş ayarları** genişletin ve alanları aşağıdaki gibi doldurun.

    Gelişmiş ayarlar|Value
    ------|------
    Birincil ölçüm| AUC_weighted 
    Çıkış kriterleri| Bu ölçütlerden herhangi biri karşılandığında, eğitim işi tam tamamlanmadan önce sonlanır. <br> *Eğitim iş süresi (dakika)* : 5  <br> *En fazla yineleme sayısı*: 10 
    Ön| Otomatik makine öğrenimi tarafından gerçekleştirilen ön işleme 'yi etkinleştirin. Bu, yapay özellikler oluşturmak için otomatik veri temizleme, hazırlama ve dönüştürmeyi içerir.
    Doğrulama| Çapraz-katlama çapraz doğrulaması ' nı ve çapraz doğrulama sayısı için 2 ' yi seçin. 
    Eş Zamanlılık| En fazla eşzamanlı yineleme sayısı için 5 ' i seçin.

   >[!NOTE]
   > Bu deneme için, ölçüm veya en fazla yineleme eşiğini ayarlayamadığımızda algoritmaların test edildiğini engellemez.

1. Denemeyi çalıştırmak için **Başlat** ' a tıklayın.

   Deneme başladıktan sonra, en üstte aşağıdaki durumu içeren bir boş **çalıştırma ayrıntısı** ekranı görürsünüz. Deneme hazırlama işlemi birkaç dakika sürer. Hazırlama işlemi tamamlandığında, çalıştırılacak durum iletisi **çalışıyor**olarak değişir.

      ![Hazırlama Çalıştır](media/tutorial-1st-experiment-automated-ml/run-preparing.png)

##  <a name="view-experiment-details"></a>Deneme ayrıntılarını görüntüle

Deneme ilerledikçe, **çalışma ayrıntısı** ekranı, yineleme grafiğini ve listesini çalıştırılan farklı yinelemelerle (modeller) güncelleştirir. Yineleme listesi, ölçüm puanına göre sıraya eklenir ve varsayılan olarak, **AUC_weighted** ölçemize göre en üst düzeyde puan veren model listenin en üstünde yer alır.

>[!TIP]
> Eğitim işleri, her bir işlem hattının çalışmayı tamamlaması için birkaç dakika sürebilir.

[![Çalışma ayrıntıları panosu](media/tutorial-1st-experiment-automated-ml/run-details.png)](media/tutorial-1st-experiment-automated-ml/run-details-expanded.png#lightbox)

## <a name="deploy-model"></a>Model dağıtma

Bu deneme için, **Votingensebir** **AUC_weighted** ölçüsüne göre en iyi model olarak kabul edilir. Azure portal otomatik makine öğrenimi sayesinde, tek tıklamayla yeni verileri tahmin etmek için bu modeli bir Web hizmeti olarak dağıtabiliriz. 

1. **Ayrıntıları Çalıştır** sayfasında, **en iyi modeli dağıt** düğmesini seçin.

1. **Dağıtım En Iyi modeli** bölmesini aşağıda gösterildiği gibi doldurun.

    Alan| Value
    ----|----
    Dağıtım adı| My-Oto ml-Deploy
    Dağıtım açıklaması| İlk otomatik makine öğrenimi deneme dağıtımı
    Puanlama betiği| Otomatik olarak açıklamalı
    Ortam betiği| Otomatik olarak açıklamalı
    
1. **Dağıt**'ı seçin. Dağıtımın tamamlanması yaklaşık 20 dakika sürebilir.

    Dağıtım başarıyla tamamlandığında aşağıdaki ileti görünür.

    ![Dağıtım Tamam](media/tutorial-1st-experiment-automated-ml/deploy-complete-status.png)
    
    İşte bu kadar! Tahmin oluşturmak için işlemsel bir Web hizmetiniz vardır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Dağıtım dosyaları veri ve deneme dosyalarından daha büyüktür, bu nedenle depolama için daha fazla ücret alırlar. Hesap maliyetlerini en aza indirmek için yalnızca dağıtım dosyalarını silin ve çalışma alanınızı ve deneme dosyalarını sürdürmek istiyorsanız. Aksi takdirde, herhangi bir dosyayı kullanmayı planlamıyorsanız tüm kaynak grubunu silin.  

### <a name="delete-deployment-instance"></a>Dağıtım örneğini Sil

Diğer öğreticiler ve araştırmayla ilgili kaynak grubunu ve çalışma alanını tutmak istiyorsanız, yalnızca Azure portal dağıtım örneğini silin. 

1. Sol taraftaki **varlıklar** bölmesine gidin ve **dağıtımlar**' ı seçin. 

1. Silmek istediğiniz dağıtımı seçin ve **Sil**' i seçin. 

1. **Devam**' ı seçin.

### <a name="delete-resource-group"></a>Kaynak grubunu sil

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu otomatik makine öğrenimi öğreticisinde, bir sınıflandırma modeli oluşturmak ve dağıtmak için Azure portal kullandınız. Daha fazla bilgi ve sonraki adımlar için bu makalelere göz atın.

+ [Web hizmetini](how-to-consume-web-service.md)kullanma.
+ [Ön işleme](how-to-create-portal-experiments.md#preprocess)hakkında daha fazla bilgi edinin.
+ [Veri profili oluşturma](how-to-create-portal-experiments.md#profile)hakkında daha fazla bilgi edinin.
+ [Otomatik makine öğrenimi](concept-automated-ml.md)hakkında daha fazla bilgi edinin.

>[!NOTE]
> Bu banka pazarlama veri kümesi, [Creative Commons (CCO) altında kullanıma sunulmuştur. Genel etki alanı)](https://creativecommons.org/publicdomain/zero/1.0/)lisansı. Veritabanının bireysel içeriklerinde herhangi bir hak, [veritabanı Içeriği lisansı](https://creativecommons.org/publicdomain/zero/1.0/) kapsamında lisanslanır ve kada mevcuttur. [](https://www.kaggle.com/janiobachmann/bank-marketing-dataset) Bu veri kümesi, ilk olarak [UCI Machine Learning veritabanı](https://archive.ics.uci.edu/ml/datasets/bank+marketing)dahilinde kullanılabilir.<br><br>
>  Lütfen aşağıdaki işi belirtin: <br> [Moro et al., 2014] S. Moro, P. Cortez ve P. Rita. Banka telefonla Bansının başarısını tahmin etmeye yönelik veri odaklı bir yaklaşım. Karar destek sistemleri, Elsevier, 62:22-31 Haziran 2014.
