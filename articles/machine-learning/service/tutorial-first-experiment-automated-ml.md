---
title: İlk otomatik makine öğrenimi denemenizi oluşturun
titleSuffix: Azure Machine Learning
description: Azure Machine Learning çalışma alanı giriş sayfasında (Önizleme) otomatik makine öğrenimi ile bir sınıflandırma modelini eğitme ve dağıtmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: tzvikei
author: tsikiksr
ms.reviewer: nibaccam
ms.date: 09/09/2019
ms.openlocfilehash: 2422a4525c94f3997dd0a9a0859135e9acf59ffa
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71092000"
---
# <a name="tutorial-create-your-first-classification-model-with-automated-machine-learning"></a>Öğretici: Otomatik makine öğrenimi ile ilk sınıflandırma modelinizi oluşturma

Bu öğreticide, tek bir kod satırı yazmadan çalışma alanı giriş sayfasında (Önizleme) ilk otomatik makine öğrenimi denemenizi oluşturmayı öğreneceksiniz. Bu örnek, bir istemcinin bir mali kurum ile sabit bir depozito 'e abone olup olmadığını tahmin etmek için bir sınıflandırma modeli oluşturur.

Otomatik makine öğrenimi sayesinde yoğun zamanda yoğun görevleri otomatik hale getirebilirsiniz. Otomatikleştirilmiş makine öğrenimi, seçtiğiniz başarı ölçümünü temel alarak en iyi modeli bulmanıza yardımcı olmak üzere birçok algoritma ve hiper parametre kombinasyonu üzerinde hızlı bir şekilde yinelenir.

Bu öğreticide, aşağıdaki görevleri nasıl gerçekleştireceğinizi öğreneceksiniz:

> [!div class="checklist"]
> * Bir Azure Machine Learning çalışma alanı oluşturun.
> * Otomatik makine öğrenimi denemesinin çalıştırın.
> * Deneme ayrıntılarını görüntüleyin.
> * Model dağıtma.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://aka.ms/AMLFree)oluşturun.

* [**Bankmarketing_train. csv**](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) veri dosyasını indirin. **Y** sütunu, bir müşterinin, daha sonra bu öğreticide tahmine yönelik hedef sütun olarak tanımlanan sabit bir dönem yatırma abone olup olmadığını gösterir. 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Azure Machine Learning çalışma alanı, bulutta makine öğrenimi modellerini denemek, eğmek ve dağıtmak için kullandığınız temel bir kaynaktır. Azure aboneliğiniz ve kaynak grubunuz, hizmette kolayca tüketilen bir nesne ile aynı olur. 

Azure kaynaklarınızı yönetmek için Web tabanlı bir konsol olan Azure portal bir çalışma alanı oluşturursunuz. 

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

>[!IMPORTANT] 
> **Çalışma alanınızı** ve **aboneliğinizi**bir yere göz atın. Denemenizin doğru yerde oluşturulmasını sağlamak için bunlara ihtiyacınız olacaktır. 

## <a name="create-and-run-the-experiment"></a>Deneme oluşturma ve çalıştırma

Aşağıdaki deneme kurulumunu tamamlar ve çalışma alanı giriş sayfasında, tüm beceri seviyelerinin veri bilimi senaryolarına yönelik veri bilimi senaryoları gerçekleştirmek için makine öğrenimi araçlarını içeren birleştirilmiş bir arabirim olan çalışma alanı giriş sayfasında adımları gerçekleştirin.

1. [Çalışma alanı giriş sayfasında](https://ml.azure.com/workspaceportal/)oturum açın.

1. Aboneliğinizi ve oluşturduğunuz çalışma alanını seçin.

1. **Kullanmaya**başlayın ' ı seçin.

1.  Sol taraftaki bölmedeki **yazma** bölümünün altında **Otomatik ml** ' yi seçin.
Otomatik Machine Learning ilk denemeniz bu olduğundan **Başlarken** ekranını görürsünüz.

    ![Azure Machine Learning Studio](media/tutorial-1st-experiment-automated-ml/get-started.png)

1. **Deneme oluştur**' u seçin. 

1. Deneme adı olarak **My-1-oto ml-deney** i girin.

1. **Yeni Işlem oluştur**' u seçin. İşlem, eğitim betiğinizi çalıştırmak veya hizmet dağıtımınızı barındırmak için kullanılan yerel veya bulut tabanlı bir kaynak ortamıdır. Bu deneme için bulut tabanlı bir işlem kullanıyoruz. 

    1. Bu deneme için işlem bağlamını yapılandırın.
        
        Alan | Value
        ----|---
        İşlem adı |  İşlem bağlamını tanımlayan benzersiz bir ad girin. Bu örnek için, **oto ml-COMPUTE**kullanın.
        Sanal makine boyutu| İşlem için sanal makine boyutunu seçin. Varsayılan, **Standard_DS12_V2**kullanın.
        Ek ayarlar| *En az düğüm*: 1. Veri profilini oluşturmayı etkinleştirmek için bir veya daha fazla düğüme sahip olmanız gerekir. <br> *En fazla düğüm*: 6.
 
    1. Yeni işlem oluşturmak için **Oluştur**' u seçin. Bu, tamamlanacak birkaç dakika sürer. 

    1. Oluşturma işlemi tamamlandığında, açılan listeden yeni işlem ' ı seçin ve ardından **İleri**' yi seçin.

    >[!NOTE]
    >Bu öğretici için, varsayılan depolama hesabını ve yeni hesaplamanıza göre oluşturulan kapsayıcıyı kullanacaksınız. Otomatik olarak formda doldurulur.

1. **Yerel dosyadan karşıya yükle**' yi seçin. Buradan, bu öğreticide daha önce indirdiğiniz **bankmarketing_train. csv** dosyasını içeren yeni bir veri kümesi oluşturursunuz. 

    1. **Araştır** ' ı seçin ve ardından yerel bilgisayarınızdaki **bankmarketing_train. csv** dosyasını seçin. 

    1. Veri kümenize benzersiz bir ad verin ve isteğe bağlı bir açıklama sağlayın. 

    1. Çalışma alanı oluşturma sırasında otomatik olarak ayarlanan varsayılan kapsayıcıya yüklemek için **İleri ' yi** seçin. Genel Önizleme yalnızca yerel dosya yüklemelerini destekler. 

    1. Karşıya yükleme tamamlandığında, **Ayarlar ve önizleme** formu dosya türüne göre akıllıca doldurulur. Formun aşağıdaki gibi doldurulduğundan emin olun.
        
        Alan|Value
        ---|---
        Dosya biçimi| Ted
        Sınırlayıcı| Virgül
        Encoding| UTF-8
        Sütun üstbilgileri| Tüm dosyaların aynı üst bilgileri var
        Satırları atla | Yok.

        >[!NOTE]
        > Bu formdaki ayarlardan herhangi biri güncelleniyorsa önizleme, buna uygun olarak güncelleştirilir.

        **İleri**’yi seçin.
    

    1. **Şema** formu, bu deneme için verilerinizin daha fazla yapılandırılmasını sağlar. Bu örnek için, **day_of_week** özelliği için geçiş anahtarını seçin. bu nedenle, bu deneme için dahil edilmez. Deneme için veri kümesinin karşıya yüklenmesini ve oluşturulmasını tamamladıktan sonra **bitti**' yi seçin.

        ![Önizleme sekmesi yapılandırması](media/tutorial-1st-experiment-automated-ml/schema-tab-config.gif)

        
1. Tahmin görevi olarak **Sınıflandırmayı** seçin.

1. Hedef sütun olarak **y** ' yi, ne tahmin etmek istediğinizi seçin. Bu sütun, istemcinin yatırma bir terime abone olup olmadığını gösterir.

1. **Gelişmiş ayarlar** ' ı genişletin ve alanları aşağıdaki gibi doldurun.

    Gelişmiş ayarlar|Value
    ------|------
    Birincil ölçüm| AUC_weighted 
    Çıkış kriterleri| Bu ölçütlerden herhangi biri karşılandığında, eğitim işi tam tamamlamayı bitmeden önce sonlanır: <br> *Eğitim iş süresi (dakika)* : 5  <br> *En fazla yineleme sayısı*: 10 
    Ön| Otomatik makine öğrenimi tarafından gerçekleştirilen ön işleme etkinleştirilir. Bu, yapay özellikler oluşturmak için otomatik veri temizleme, hazırlama ve dönüştürmeyi içerir.
    Doğrulama| Çapraz doğrulama sayısı için K-katlama çapraz doğrulamayı ve **2** ' yi seçin. 
    Eş Zamanlılık| Maksimum eşzamanlı yineleme sayısı için **5** ' i seçin.

   >[!NOTE]
   > Bu deneme için, yineleme eşiğine göre ölçüm veya en fazla çekirdek ayarlayamazsınız. Ayrıca algoritmaların test edilmeye engel kalmazsınız.

1. Denemeyi çalıştırmak için **Başlat** ' ı seçin.

   Deneme başladığında, en üstte bir durum iletisi içeren boş bir ekran görürsünüz.

Deneme hazırlama işlemi birkaç dakika sürer. Bu işlem tamamlandığında, durum iletisi **çalışmaya çalışıyor**olarak değişir.

##  <a name="view-experiment-details"></a>Deneme ayrıntılarını görüntüle

Deneme ilerledikçe ekran, **yineleme grafiği** ve **yineleme listesini** çalıştırılan farklı yinelemeler (modeller) ile güncelleştirir. Yineleme listesi, ölçüm puanına göre sıraya göre yapılır. Varsayılan olarak, **AUC_weighted** ölçemize göre en yüksek düzeyde puan veren model listenin en üstünde yer alır.

>[!WARNING]
> Eğitim işleri her bir işlem hattının çalışmayı tamamlaması için birkaç dakika sürer.

[![Çalışma ayrıntıları panosu](media/tutorial-1st-experiment-automated-ml/run-details.png)](media/tutorial-1st-experiment-automated-ml/run-details-expanded.png#lightbox)

## <a name="deploy-the-model"></a>Modeli dağıtma

Çalışma alanı giriş sayfasında otomatik makine öğrenimini kullanarak, birkaç adımda Web hizmeti olarak en iyi modeli dağıtabilirsiniz. Dağıtım, yeni verileri tahmin etmek ve potansiyel fırsat bölgelerini belirlemek için modelin Tümleştirmesidir. Bu deneme için, bir Web hizmetine dağıtım, artık, potansiyel sabit depozito müşterilerinin tanımlanmasından sonra, mali kurumda yinelenen ve ölçeklenebilir bir Web çözümüne sahip olduğu anlamına gelir. 

Bu deneme bağlamında, **Votingensebir** **AUC_weighted** ölçüsüne göre en iyi model olarak kabul edilir.  Bu modeli dağıyoruz ancak yapmanız önerilir, dağıtımın tamamlaması yaklaşık 20 dakika sürer. Dağıtım işlemi, modeli kaydetme, kaynakları oluşturma ve bunları Web hizmeti için yapılandırma dahil olmak üzere birkaç adım gerektirir.

1. **Ayrıntıları Çalıştır** sayfasında sağ üst köşedeki **en iyi modeli dağıt** düğmesini seçin.

1. **Dağıtım En Iyi modeli** bölmesini aşağıdaki gibi doldurun:

    Alan| Value
    ----|----
    Dağıtım adı| My-Oto ml-Deploy
    Dağıtım açıklaması| İlk otomatik makine öğrenimi deneme dağıtımı
    Puanlama betiği| Otomatik olarak açıklamalı
    Ortam betiği| Otomatik olarak açıklamalı
    
1. **Dağıt**'ı seçin.  

    Dağıtım başarıyla tamamlandığında dağıtım tamamlandı iletisi görüntülenir.
    
Artık tahminleri oluşturmak için işlemsel bir Web hizmetiniz vardır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Dağıtım dosyaları veri ve deneme dosyalarından daha büyüktür, bu nedenle daha fazla depolama alanı maliyetlidir. Hesap maliyetlerini en aza indirmek için yalnızca dağıtım dosyalarını silin veya çalışma alanınızı ve deneme dosyalarını korumak istiyorsanız. Aksi takdirde, herhangi bir dosyayı kullanmayı planlamıyorsanız tüm kaynak grubunu silin.  

### <a name="delete-the-deployment-instance"></a>Dağıtım örneğini silme

Diğer öğreticiler ve araştırmayla ilgili kaynak grubunu ve çalışma alanını tutmak istiyorsanız, yalnızca Azure portal dağıtım örneğini silin. 

1. [Azure Portal](https://portal.azure.com//) gidin. Çalışma alanınıza gidin ve **varlıklar** bölmesinin sol tarafında **dağıtımlar**' ı seçin. 

1. Silmek istediğiniz dağıtımı seçin ve **Sil**' i seçin. 

1. **Devam**' ı seçin.

### <a name="delete-the-resource-group"></a>Kaynak grubunu silme

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu otomatik makine öğrenimi öğreticisinde, bir sınıflandırma modeli oluşturmak ve dağıtmak için çalışma alanı giriş sayfasını kullandınız. Daha fazla bilgi ve sonraki adımlar için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Web hizmeti kullanma](how-to-consume-web-service.md)

+ [Ön işleme](how-to-create-portal-experiments.md#preprocess)hakkında daha fazla bilgi edinin.
+ [Veri profili oluşturma](how-to-create-portal-experiments.md#profile)hakkında daha fazla bilgi edinin.
+ [Otomatik makine öğrenimi](concept-automated-ml.md)hakkında daha fazla bilgi edinin.

>[!NOTE]
> Bu banka pazarlama veri kümesi, [Creative Commons (CCO) altında kullanıma sunulmuştur. Genel etki alanı)](https://creativecommons.org/publicdomain/zero/1.0/)lisansı. Veritabanının bireysel içeriklerinde her türlü hak, [veritabanı Içeriği lisansı](https://creativecommons.org/publicdomain/zero/1.0/) kapsamında lisanslanır ve [kada](https://www.kaggle.com/janiobachmann/bank-marketing-dataset)mevcuttur. Bu veri kümesi, ilk olarak [UCI Machine Learning veritabanı](https://archive.ics.uci.edu/ml/datasets/bank+marketing)dahilinde kullanılabilir.<br><br>
> [Moro et al., 2014] S. Moro, P. Cortez ve P. Rita. Banka telefonla Bansının başarısını tahmin etmeye yönelik veri odaklı bir yaklaşım. Karar destek sistemleri, Elsevier, 62:22-31 Haziran 2014.
