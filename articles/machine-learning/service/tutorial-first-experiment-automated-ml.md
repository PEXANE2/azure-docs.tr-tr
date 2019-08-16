---
title: İlk otomatik makine öğrenimi denemenizi oluşturun
titleSuffix: Azure Machine Learning service
description: Azure portal otomatik makine öğrenimi ile bir sınıflandırma modelini eğitme ve dağıtmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: tzvikei
author: tsikiksr
ms.reviewer: nibaccam
ms.date: 08/14/2019
ms.openlocfilehash: e53cd92a9dfd8f823918fb38e14c2b73c2ce071f
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69534398"
---
# <a name="tutorial-create-your-first-classification-model-with-automated-machine-learning"></a>Öğretici: Otomatik makine öğrenimi ile ilk sınıflandırma modelinizi oluşturma

Bu öğreticide, tek bir kod satırı yazmadan Azure portal (Önizleme) içinde ilk otomatik makine öğrenimi denemenizi oluşturmayı öğreneceksiniz. Bu örnek, bir istemcinin bir mali kurum ile sabit bir depozito 'e abone olup olmadığını tahmin etmek için bir sınıflandırma modeli oluşturur.

Azure Machine Learning hizmetinin ve Azure portal otomatik makine öğrenimi özelliklerini kullanarak otomatik makine öğrenimi işlemine başlarsınız. Algoritma seçimi ve hyperparameter ayarı sizin için yapılır. Otomatik makine öğrenimi tekniği, kriterinize göre en iyi modeli bulana kadar birçok algoritma ve hiper parametre kombinasyonu üzerinde yinelenir.

Bu öğreticide, aşağıdaki görevleri öğreneceksiniz:

> [!div class="checklist"]
> * Azure Machine Learning bir hizmet çalışma alanı yapılandırın.
> * Deneme oluşturma.
> * Sınıflandırma modelini otomatik eğitme.
> * Eğitim çalışma ayrıntılarını görüntüleyin.
> * Model dağıtma.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://aka.ms/AMLFree)oluşturun.

* [ **Bankmarketing_train. csv** ](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) veri dosyasını indirin. **Y** sütunu, bir müşterinin, daha sonra bu öğreticide tahmine yönelik hedef sütun olarak tanımlanan sabit bir dönem yatırma abone olup olmadığını gösterir. 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="create-an-experiment"></a>Deneme oluşturma

Bu adımlar, birincil ölçümünüzün ve model türünün seçilmesi için veri seçiminden ayarlanan denemenize kılavuzluk sağlar. 

1. Çalışma alanınızın sol bölmesine gidin. **Yazma (Önizleme)** bölümünde **otomatik makine öğrenimi** ' ni seçin.
Otomatik Machine Learning için ilk denemenizin olduğu **Için otomatik Machine Learning ekranına hoş geldiniz** ' i görürsünüz.

    ![Azure portal gezinti bölmesi](media/tutorial-1st-experiment-automated-ml/nav-pane.png)



1. **Deneme oluştur**' u seçin. Ardından deneme adı olarak **My-1-oto ml-deney** i girin.

1. **Yeni Işlem oluştur** ' u seçin ve işlem bağlamını bu deneme için yapılandırın.

    Alan| Value
    ---|---
    İşlem adı| İşlem bağlamını tanımlayan benzersiz bir ad girin. Bu örnekte, **oto ml-COMPUTE**kullanırız.
    Sanal makine boyutu| İşlem için sanal makine boyutunu seçin. **Standard_DS12_V2**kullanıyoruz.
    Ek ayarlar| *En az düğüm*: 1. Veri profilini oluşturmayı etkinleştirmek için bir veya daha fazla düğüme sahip olmanız gerekir. <br> *En fazla düğüm*: 6. 

    Yeni işlem oluşturmak için **Oluştur**' u seçin. Bu birkaç dakika sürer. 

    Oluşturma işlemi tamamlandığında, açılan listeden yeni işlem ' ı seçin ve ardından **İleri**' yi seçin.

    >[!NOTE]
    >Bu öğretici için, varsayılan depolama hesabını ve yeni hesaplamanıza göre oluşturulan kapsayıcıyı kullanırız. Otomatik olarak formda doldurulur.

1. **Karşıya yükle** ' yi seçin ve varsayılan kapsayıcıya yüklemek için yerel bilgisayarınızdan **bankmarketing_train. csv** dosyasını seçin. Genel Önizleme yalnızca yerel dosya yüklemelerini ve Azure Blob depolama hesaplarını destekler. Karşıya yükleme tamamlandığında, listeden dosyayı seçin. 

    [![Veri dosyası seç](media/tutorial-1st-experiment-automated-ml/select-data-file.png)](media/tutorial-1st-experiment-automated-ml/select-data-file-expanded.png#lightbox)

1. **Önizleme** sekmesi, bu deneme için verilerimizi daha fazla yapılandırmamızı sağlar.

    **Önizleme** sekmesinde, verilerin üstbilgiler içerdiğini belirtin. Hizmet varsayılan olarak eğitimin tüm özelliklerini (sütunları) dahil eder. Bu örnekte, sağa kaydırın ve **day_of_week** özelliğini **yoksayın** .

    ![Önizleme sekmesi yapılandırması](media/tutorial-1st-experiment-automated-ml/preview-tab-config.gif)


    >[!NOTE]
    > Sıfır minimum düğümü olan hesaplalarla veri profili oluşturma kullanılamıyor.

1. Tahmin görevi olarak **Sınıflandırmayı** seçin.

1. Tahmin yapmak istediğimiz hedef sütun olarak **y** 'yi seçin. Bu sütun, istemcinin yatırma bir terime abone olup olmadığını gösterir.

1. **Gelişmiş ayarlar** ' ı genişletin ve alanları aşağıdaki gibi doldurun.

    Gelişmiş ayarlar|Value
    ------|------
    Birincil ölçüm| AUC_weighted 
    Çıkış kriterleri| Bu ölçütlerden herhangi biri karşılandığında, eğitim işi tam tamamlamayı bitmeden önce sonlanır: <br> *Eğitim iş süresi (dakika)* : 5  <br> *En fazla yineleme sayısı*: 10 
    Ön| Otomatik makine öğrenimi tarafından gerçekleştirilen ön işleme etkinleştirilir. Bu, yapay özellikler oluşturmak için otomatik veri temizleme, hazırlama ve dönüştürmeyi içerir.
    Doğrulama| Çapraz doğrulama sayısı için K-katlama çapraz doğrulamayı ve **2** ' yi seçin. 
    Eş Zamanlılık| Maksimum eşzamanlı yineleme sayısı için **5** ' i seçin.

   >[!NOTE]
   > Bu deneme için, yineleme eşiğine göre ölçüm veya en fazla çekirdek ayarlayamıyoruz. Ayrıca algoritmaların test edilmeye engel olmaz.

1. Denemeyi çalıştırmak için **Başlat** ' ı seçin.

   Deneme başladığında, en üstte aşağıdaki durumu içeren bir boş **çalıştırma ayrıntısı** ekranı görürsünüz. 

      ![Hazırlama Çalıştır](media/tutorial-1st-experiment-automated-ml/run-preparing.png)
      
Deneme hazırlama işlemi birkaç dakika sürer. İşlem tamamlandığında, durum iletisi **çalışmaya çalışıyor**olarak değişir.

##  <a name="view-experiment-details"></a>Deneme ayrıntılarını görüntüle

Deneme ilerledikçe, **çalışma ayrıntısı** ekranı, yineleme grafiğini ve listesini çalıştırılan farklı yinelemelerle (modeller) güncelleştirir. Yineleme listesi, ölçüm puanına göre sıraya göre yapılır. Varsayılan olarak, **AUC_weighted** ölçemize göre en yüksek düzeyde puan veren model listenin en üstünde yer alır.

>[!TIP]
> Eğitim işleri her bir işlem hattının çalışmayı tamamlaması için birkaç dakika sürer.

[![Çalışma ayrıntıları panosu](media/tutorial-1st-experiment-automated-ml/run-details.png)](media/tutorial-1st-experiment-automated-ml/run-details-expanded.png#lightbox)

## <a name="deploy-the-model"></a>Modeli dağıtma

Azure portal otomatik makine öğrenimi kullanarak, yeni verileri tahmin etmek ve potansiyel fırsat bölgelerini belirlemek için en iyi modeli bir Web hizmeti olarak dağıtabiliriz. Bu deneme için, dağıtım, mali kurumun potansiyel olarak sabit vadeli depozito müşterilerini tanımlamaya yönelik yinelemeli ve ölçeklenebilir bir çözüme sahip olduğu anlamına gelir.

Bu deneme bağlamında, **Votingensebir** **AUC_weighted** ölçüsüne göre en iyi model olarak kabul edilir.  Bu modeli dağıyoruz ancak yapmanız önerilir, dağıtımın tamamlaması yaklaşık 20 dakika sürer.

1. **Ayrıntıları Çalıştır** sayfasında, **en iyi modeli dağıt** düğmesini seçin.

1. **Dağıtım En Iyi modeli** bölmesini aşağıdaki gibi doldurun:

    Alan| Value
    ----|----
    Dağıtım adı| My-Oto ml-Deploy
    Dağıtım açıklaması| İlk otomatik makine öğrenimi deneme dağıtımı
    Puanlama betiği| Otomatik olarak açıklamalı
    Ortam betiği| Otomatik olarak açıklamalı
    
1. **Dağıt**'ı seçin.

    Dağıtım başarıyla tamamlandığında aşağıdaki ileti görünür:

    ![Dağıtım Tamam](media/tutorial-1st-experiment-automated-ml/deploy-complete-status.png)
    
    Artık tahminleri oluşturmak için işlemsel bir Web hizmetiniz vardır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Dağıtım dosyaları veri ve deneme dosyalarından daha büyüktür, bu nedenle daha fazla depolama alanı maliyetlidir. Hesap maliyetlerini en aza indirmek için yalnızca dağıtım dosyalarını silin veya çalışma alanınızı ve deneme dosyalarını korumak istiyorsanız. Aksi takdirde, herhangi bir dosyayı kullanmayı planlamıyorsanız tüm kaynak grubunu silin.  

### <a name="delete-the-deployment-instance"></a>Dağıtım örneğini silme

Diğer öğreticiler ve araştırmayla ilgili kaynak grubunu ve çalışma alanını tutmak istiyorsanız, yalnızca Azure portal dağıtım örneğini silin. 

1. Sol taraftaki **varlıklar** bölmesine gidin ve **dağıtımlar**' ı seçin. 

1. Silmek istediğiniz dağıtımı seçin ve **Sil**' i seçin. 

1. **Devam**' ı seçin.

### <a name="delete-the-resource-group"></a>Kaynak grubunu silme

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu otomatik makine öğrenimi öğreticisinde, bir sınıflandırma modeli oluşturmak ve dağıtmak için Azure portal kullandınız. Daha fazla bilgi ve sonraki adımlar için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Web hizmeti kullanma](how-to-consume-web-service.md)


+ [Ön işleme](how-to-create-portal-experiments.md#preprocess)hakkında daha fazla bilgi edinin.
+ [Veri profili oluşturma](how-to-create-portal-experiments.md#profile)hakkında daha fazla bilgi edinin.
+ [Otomatik makine öğrenimi](concept-automated-ml.md)hakkında daha fazla bilgi edinin.

>[!NOTE]
> Bu banka pazarlama veri kümesi, [Creative Commons (CCO) altında kullanıma sunulmuştur. Genel etki alanı)](https://creativecommons.org/publicdomain/zero/1.0/)lisansı. Veritabanının bireysel içeriklerinde her türlü hak, [veritabanı Içeriği lisansı](https://creativecommons.org/publicdomain/zero/1.0/) kapsamında lisanslanır ve kada mevcuttur [](https://www.kaggle.com/janiobachmann/bank-marketing-dataset). Bu veri kümesi, ilk olarak [UCI Machine Learning veritabanı](https://archive.ics.uci.edu/ml/datasets/bank+marketing)dahilinde kullanılabilir.<br><br>
> Lütfen aşağıdaki işi belirtin: <br> [Moro et al., 2014] S. Moro, P. Cortez ve P. Rita. Banka telefonla Bansının başarısını tahmin etmeye yönelik veri odaklı bir yaklaşım. Karar destek sistemleri, Elsevier, 62:22-31 Haziran 2014.
