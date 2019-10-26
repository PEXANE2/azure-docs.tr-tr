---
title: İlk otomatik ML sınıflandırma denemenizi oluşturun
titleSuffix: Azure Machine Learning
description: Azure Machine Learning çalışma alanı giriş sayfasında (Önizleme) otomatik makine öğrenimi ile bir sınıflandırma modelini eğitme ve dağıtmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: tzvikei
author: tsikiksr
ms.reviewer: nibaccam
ms.date: 09/26/2019
ms.openlocfilehash: 726ac906b5f237cfc40a8399b7b1bef3071f98f3
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2019
ms.locfileid: "72901796"
---
# <a name="tutorial-create-your-first-classification-model-with-automated-machine-learning"></a>Öğretici: otomatik makine öğrenimi ile ilk sınıflandırma modelinizi oluşturma

Bu öğreticide, tek bir kod satırı yazmadan çalışma alanı giriş sayfasında (Önizleme) ilk otomatik makine öğrenimi denemenizi oluşturmayı öğreneceksiniz. Bu örnek, bir istemcinin bir mali kurum ile sabit bir depozito 'e abone olup olmadığını tahmin etmek için bir sınıflandırma modeli oluşturur.

Otomatik makine öğrenimi sayesinde yoğun zamanda yoğun görevleri otomatik hale getirebilirsiniz. Otomatikleştirilmiş makine öğrenimi, seçtiğiniz başarı ölçümünü temel alarak en iyi modeli bulmanıza yardımcı olmak üzere birçok algoritma ve hiper parametre kombinasyonu üzerinde hızlı bir şekilde yinelenir.

Bu öğreticide, aşağıdaki görevleri nasıl gerçekleştireceğinizi öğreneceksiniz:

> [!div class="checklist"]
> * Azure Machine Learning çalışma alanı oluşturun.
> * Otomatik makine öğrenimi denemesinin çalıştırın.
> * Deneme ayrıntılarını görüntüleyin.
> * Modeli dağıtın.

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

Aşağıdaki deneme kurulumunu tamamlar ve çalışma alanı giriş sayfasında, tüm beceri seviyelerinin veri bilimi senaryolarına yönelik veri bilimi senaryoları gerçekleştirmek için makine öğrenimi araçlarını içeren birleştirilmiş bir arabirim olan çalışma alanı giriş sayfasında adımları gerçekleştirin. Çalışma alanı giriş sayfası Internet Explorer tarayıcılarında desteklenmez.

1. [Çalışma alanı giriş sayfasında](https://ml.azure.com/workspaceportal/)oturum açın.

1. Aboneliğinizi ve oluşturduğunuz çalışma alanını seçin.

1. **Kullanmaya**başlayın ' ı seçin.

1. Sol bölmede **Yazar** bölümü altında **Otomatik ml** ' yi seçin.

   Bu ilk otomatik ML denemenize ait olduğundan Başlarken ekranını görürsünüz.

   ![Azure Machine Learning Studio](media/tutorial-1st-experiment-automated-ml/get-started.png)

1. **Deneme oluştur**' u seçin. 

1. Bu deneme adını girin: `my-1st-automl-experiment`

1. **Yeni Işlem oluştur** ' u seçin ve işlem hedefini yapılandırın. İşlem hedefi, eğitim betiğinizi çalıştırmak veya hizmet dağıtımınızı barındırmak için kullanılan yerel veya bulut tabanlı bir kaynak ortamıdır. Bu deneme için bulut tabanlı bir işlem kullanıyoruz. 

   Alan | Açıklama | Öğretici için değer
   ----|---|---
   İşlem adı |İşlem bağlamını tanımlayan benzersiz bir ad.|Oto ml-işlem
   Sanal &nbsp;machine &nbsp;size| İşlem için sanal makine boyutunu seçin.|Standard_DS12_V2
   En az/en fazla düğüm (Gelişmiş ayarlarda)| Veri profili için, 1 veya daha fazla düğüm belirtmeniz gerekir.|En az düğümler: 1<br>En fazla düğüm: 6

   >[!NOTE]
   >Bu öğretici için, varsayılan depolama hesabını ve yeni hesaplamanıza göre oluşturulan kapsayıcıyı kullanacaksınız. Otomatik olarak formda doldurulur.
    
1. İşlem hedefini almak için **Oluştur** ' u seçin. 

   **Bu, tamamlanacak birkaç dakika sürer.** 

1. Oluşturulduktan sonra, açılan listeden yeni işlem hedefini seçin ve **İleri ' yi**seçin.

1. Yeni bir veri kümesi oluşturmaya başlamak için **yerel dosyadan karşıya yükle** ' yi seçin. 

    1. **Gözat**’ı seçin.
    
    1. Yerel bilgisayarınızda **bankmarketing_train. csv** dosyasını seçin. Bu, bir [Önkoşul](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv)olarak indirdiğiniz dosyadır.

    1. Veri kümenize benzersiz bir ad verin ve isteğe bağlı bir açıklama sağlayın. 

    1. Çalışma alanı oluşturma sırasında otomatik olarak ayarlanan varsayılan kapsayıcıya yüklemek için sol alt kısımdaki **İleri** ' yi seçin. Genel Önizleme yalnızca yerel dosya yüklemelerini destekler. 
    
       Karşıya yükleme tamamlandığında, ayarlar ve önizleme formu dosya türüne göre önceden doldurulur. 
       
    1. **Ayarlar ve önizleme** formunun aşağıdaki gibi doldurulduğunu doğrulayın ve **İleri ' yi**seçin.
        
        Alan|Açıklama| Öğretici için değer
        ---|---|---
        Dosya biçimi|Bir dosyada depolanan verilerin yerleşimini ve türünü tanımlar.| Ted
        Ayırıcı|Düz metin veya diğer veri akışlarında &nbsp; ayrı, bağımsız bölgeler arasındaki sınırı belirtmek için bir veya daha fazla karakter. |Virgülle
        Encoding|Veri kümenizi okumak için kullanılacak bit karakter şeması tablosunu belirler.| UTF-8
        Sütun başlıkları| Veri kümesinin üst bilgilerinin (varsa) nasıl değerlendirileceğini gösterir.| Tüm dosyaların aynı üst bilgileri var
        Satırları atla | Veri kümesinde kaç tane, ne varsa satırların atlandığını gösterir.| Hiçbiri
    
        ![Önizleme sekmesi yapılandırması](media/tutorial-1st-experiment-automated-ml/schema-tab-config.gif)

1. Tahmin görevi olarak **Sınıflandırmayı** seçin.

1. Hedef sütun olarak **y** ' yi, ne tahmin etmek istediğinizi seçin. Bu sütun, istemcinin yatırma bir terime abone olup olmadığını gösterir.

1. **Gelişmiş ayarlar** ' ı genişletin ve alanları aşağıdaki gibi doldurun. Bu ayarlar, eğitim işini daha iyi denetliyor. Aksi takdirde, denemeler seçimine ve verilerine göre varsayılan ayarlar uygulanır.

   >[!NOTE]
   > Bu öğreticide, yineleme eşiğine göre ölçüm puanı veya en fazla çekirdek ayarlayamayacağız. Ya da algoritmaların test edilmeye engel olursunuz.
   
   Gelişmiş &nbsp;settings|Açıklama|Değer &nbsp;for &nbsp;tutorial
   ------|---------|---
   Birincil ölçüm| Makine öğrenimi algoritmasının ölçülecek değerlendirme ölçümü.|AUC_weighted
   Çıkış kriterleri| Bir kriterle karşılanırsa eğitim işi durdurulur. |Eğitim &nbsp;job &nbsp;time: 5 <br> <br> En büyük &nbsp; # &nbsp;of&#58;&nbsp;iterations 10
   Ön| Otomatik makine öğrenimi tarafından gerçekleştirilen ön işleme etkinleştirilir. Bu, yapay özellikler oluşturmak için otomatik veri temizleme, hazırlama ve dönüştürmeyi içerir.| Etkinleştirme
   Doğrulama türü | Bir çapraz doğrulama türü seçin.|Çapraz doğrulamayı yana kesme
   Doğrulama sayısı | Test sayısı. | 2 çapraz doğrulama 
   Eşzamanlılık| Maksimum eşzamanlı yineleme sayısı.|5
   
1. Denemeyi çalıştırmak için **Başlat** ' ı seçin. Deneme hazırlığı başladığında bir durum iletisi içeren bir ekran görüntülenir.

>[!IMPORTANT]
> Hazırlık, deneme çalıştırmasının hazırlanmasına **10-15 dakika** sürer. Çalışmaya başladıktan sonra, **her yinelemede 2-3 dakika daha**sürer.  
>
> Üretimde, büyük olasılıkla biraz daha fazla yol göstereceğiz. Ancak bu öğreticide, diğer kullanıcılar hala çalışırken yineleme sonuçlarını keşfetmeye başlayacağız. 

##  <a name="explore-iteration-results"></a>Yineleme sonuçlarını keşfet

Deneme ilerledikçe ekran, **yineleme grafiği** ve **yineleme listesini** , tamamlandıkça oluşturulan farklı yinelemeler (modeller) ile güncelleştirir. Varsayılan olarak, yinelemeler ölçüm puanına göre sıralanır. Bu öğretici için, seçilen **AUC_weighted** ölçüsüne göre en yüksek düzeyde puan veren model listenin en üstünde yer alır.

Deneme tekrarlarının tümünün bitmesini beklerken, performans ayrıntılarını araştırmak için tamamlanmış bir yinelemenin **adını** seçin. 

Aşağıda, bir duyarlık geri çağırma eğrisi, karışıklık matrisi, ağırlıklı doğruluk puanları vb. gibi her bir yineleme için oluşturulan grafikler ve çalıştırma ölçümleri gösterilmektedir. 

![Yineleme ayrıntısı Çalıştır](media/tutorial-1st-experiment-automated-ml/run-detail.gif)

## <a name="deploy-the-model"></a>Modeli dağıtma

Çalışma alanı giriş sayfasında otomatik makine öğrenimi, birkaç adımda Web hizmeti olarak en iyi modeli dağıtmanıza olanak tanır. Dağıtım, yeni verileri tahmin etmek ve potansiyel fırsat bölgelerini belirlemek için modelin Tümleştirmesidir. Bu deneme için, bir Web hizmetine dağıtım, artık, potansiyel sabit depozito müşterilerinin tanımlanmasından sonra, mali kurumda yinelenen ve ölçeklenebilir bir Web çözümüne sahip olduğu anlamına gelir. 

Çalıştırma tamamlandıktan sonra, **yineleme grafiği** ve **yinelemeler listesi** ayrıntı sayfasına geri gidin. 

Bu deneme bağlamında, **Votingensebir** **AUC_weighted** ölçüsüne göre en iyi model olarak kabul edilir.  Bu modeli dağıyoruz ancak yapmanız önerilir, dağıtımın tamamlaması yaklaşık 20 dakika sürer. Dağıtım işlemi, modeli kaydetme, kaynakları oluşturma ve bunları Web hizmeti için yapılandırma dahil olmak üzere birkaç adım gerektirir.

1. Sağ üst köşedeki **En Iyi modeli dağıt** düğmesini seçin.

1. **Dağıtım En Iyi modeli** bölmesini aşağıdaki gibi doldurun:

    Alan| Değer
    ----|----
    Dağıtım adı| My-Oto ml-Deploy
    Dağıtım açıklaması| İlk otomatik makine öğrenimi deneme dağıtımı
    Puanlama betiği| Otomatik olarak açıklamalı
    Ortam betiği| Otomatik olarak açıklamalı
    
1. **Dağıt**'ı seçin.  

    Dağıtım başarıyla tamamlandığında dağıtım tamamlandı iletisi görüntülenir.
    
Artık tahminleri oluşturmak için işlemsel bir Web hizmetiniz vardır.

Yeni Web hizmetinizi kullanma hakkında daha fazla bilgi edinmek için [**sonraki adımlara**](#next-steps) ilerleyin ve Power BI yerleşik Azure Machine Learning desteğini kullanarak tahminlerinizi test edin.

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
> [Web hizmeti kullanma](how-to-consume-web-service.md#consume-the-service-from-power-bi)

+ [Ön işleme](how-to-create-portal-experiments.md#preprocess)hakkında daha fazla bilgi edinin.
+ [Veri profili oluşturma](how-to-create-portal-experiments.md#profile)hakkında daha fazla bilgi edinin.
+ [Otomatik makine öğrenimi](concept-automated-ml.md)hakkında daha fazla bilgi edinin.
+ Sınıflandırma ölçümleri ve grafikler hakkında daha fazla bilgi için [otomatik makine öğrenimi sonuçlarını anlama](how-to-understand-automated-ml.md#classification) makalesini inceleyin.

>[!NOTE]
> Bu banka pazarlama veri kümesi, [Creative Commons (CCO: genel etki alanı) lisansı](https://creativecommons.org/publicdomain/zero/1.0/)altında kullanılabilir hale getirilir. Veritabanının bireysel içeriklerinde her türlü hak, [veritabanı Içeriği lisansı](https://creativecommons.org/publicdomain/zero/1.0/) kapsamında lisanslanır ve [kada](https://www.kaggle.com/janiobachmann/bank-marketing-dataset)mevcuttur. Bu veri kümesi, ilk olarak [UCI Machine Learning veritabanı](https://archive.ics.uci.edu/ml/datasets/bank+marketing)dahilinde kullanılabilir.<br><br>
> [Moro et al., 2014] S. Moro, P. Cortez ve P. Rita. Banka telefonla Bansının başarısını tahmin etmeye yönelik veri odaklı bir yaklaşım. Karar destek sistemleri, Elsevier, 62:22-31 Haziran 2014.
