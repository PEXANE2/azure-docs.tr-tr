---
title: Otomatik ML sınıflandırma modelleri oluşturma
titleSuffix: Azure Machine Learning
description: Sınıflandırma modellerini Azure Machine Learning otomatik makine öğrenimi (otomatik ML) arabirimiyle nasıl eğeceğinizi &.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: tzvikei
author: tsikiksr
ms.reviewer: nibaccam
ms.date: 02/04/2020
ms.openlocfilehash: 96af942ab68d4ae738df56bf94d8410ee5d8cc34
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79129675"
---
# <a name="tutorial-create-a-classification-model-with-automated-ml-in-azure-machine-learning"></a>Öğretici: Azure Machine Learning otomatik ML ile sınıflandırma modeli oluşturma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Bu öğreticide, Azure Machine Learning otomatik makine öğrenimi arabirimini kullanarak tek bir kod satırı yazmadan temel bir sınıflandırma modeli oluşturmayı öğreneceksiniz. Bu sınıflandırma modeli, bir istemci bir mali kurum ile sabit bir havale 'e abone olurken tahmin edilir.

Otomatik makine öğrenimi sayesinde yoğun zamanda yoğun görevleri otomatik hale getirebilirsiniz. Otomatikleştirilmiş makine öğrenimi, seçtiğiniz başarı ölçümünü temel alarak en iyi modeli bulmanıza yardımcı olmak üzere birçok algoritma ve hiper parametre kombinasyonu üzerinde hızlı bir şekilde yinelenir.

Bu öğreticide, aşağıdaki görevleri nasıl gerçekleştireceğinizi öğreneceksiniz:

> [!div class="checklist"]
> * Bir Azure Machine Learning çalışma alanı oluşturun.
> * Otomatik makine öğrenimi denemesinin çalıştırın.
> * Deneme ayrıntılarını görüntüleyin.
> * Model dağıtma.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://aka.ms/AMLFree) oluşturun.

* [**Bankmarketing_train. csv**](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) veri dosyasını indirin. **Y** sütunu, bir müşterinin, daha sonra bu öğreticide tahmine yönelik hedef sütun olarak tanımlanan sabit bir dönem yatırma abone olup olmadığını gösterir. 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Azure Machine Learning çalışma alanı, bulutta makine öğrenimi modellerini denemek, eğmek ve dağıtmak için kullandığınız temel bir kaynaktır. Azure aboneliğiniz ve kaynak grubunuz, hizmette kolayca tüketilen bir nesne ile aynı olur. 

Azure kaynaklarınızı yönetmek için Web tabanlı bir konsol olan Azure portal bir çalışma alanı oluşturursunuz.

[!INCLUDE [aml-create-portal](../../includes/aml-create-in-portal-enterprise.md)]

>[!IMPORTANT] 
> **Çalışma alanınızı** ve **aboneliğinizi**bir yere göz atın. Denemenizin doğru yerde oluşturulmasını sağlamak için bunlara ihtiyacınız olacaktır. 

## <a name="create-and-run-the-experiment"></a>Deneme oluşturma ve çalıştırma

Aşağıdaki deneme kurulumunu tamamlayıp, tüm beceri seviyelerinin veri bilimi senaryolarına yönelik veri bilimi senaryoları gerçekleştirmek için Machine Learning araçları 'nı içeren birleştirilmiş bir Web arabirimi olan https://ml.azure.comAzure Machine Learning aracılığıyla adımları gerçekleştirin. Bu arabirim Internet Explorer tarayıcılarında desteklenmez.

1. https://ml.azure.comAzure Machine Learning için oturum açın.

1. Aboneliğinizi ve oluşturduğunuz çalışma alanını seçin.

1. **Kullanmaya**başlayın ' ı seçin.

1. Sol bölmede **Yazar** bölümü altında **Otomatik ml** ' yi seçin.

   Bu ilk otomatik ML denemenize ait olduğundan, boş bir liste ve belge bağlantıları görürsünüz.

   ![Başlarken sayfası](./media/tutorial-first-experiment-automated-ml/get-started.png)

1. **Yeni OTOMATIK ml çalıştırması**' nı seçin. 

1. **+ Veri kümesi oluştur** açılır listesinden **yerel dosyalardan** seçim yaparak yeni bir veri kümesi oluşturun. 

    1. **Temel bilgi** formunda, veri kümenize bir ad verin ve isteğe bağlı bir açıklama sağlayın. Otomatik ML arabirimi şu anda yalnızca Tabulardataset 'leri desteklediğinden veri kümesi türü *tablosal*olmalıdır.

    1. Sol alt kısımdaki **İleri ' yi** seçin

    1. **Veri deposu ve dosya seçimi** formunda, çalışma alanı oluşturma, çalışma alanı **BlobStore (Azure Blob depolama)** sırasında otomatik olarak ayarlanan varsayılan veri deposunu seçin. Bu, çalışma alanınız için kullanılabilir hale getirmek üzere veri dosyanızı karşıya yükleyeceksiniz.

    1. **Gözat**’ı seçin.
    
    1. Yerel bilgisayarınızda **bankmarketing_train. csv** dosyasını seçin. Bu, bir [Önkoşul](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv)olarak indirdiğiniz dosyadır.

    1. Veri kümenize benzersiz bir ad verin ve isteğe bağlı bir açıklama sağlayın. 

    1. Çalışma alanı oluşturma sırasında otomatik olarak ayarlanan varsayılan kapsayıcıya yüklemek için sol alt kısımdaki **İleri** ' yi seçin.  
    
       Karşıya yükleme tamamlandığında, ayarlar ve önizleme formu dosya türüne göre önceden doldurulur. 
       
    1. **Ayarlar ve önizleme** formunun aşağıdaki gibi doldurulduğunu doğrulayın ve **İleri ' yi**seçin.
        
        Alan|Açıklama| Öğretici için değer
        ---|---|---
        Dosya biçimi|Bir dosyada depolanan verilerin yerleşimini ve türünü tanımlar.| Ted
        Sınırlayıcı|Düz metin veya diğer veri akışlarında&nbsp; ayrı, bağımsız bölgeler arasındaki sınırı belirtmek için bir veya daha fazla karakter. |Virgülle
        Encoding|Veri kümenizi okumak için kullanılacak bit karakter şeması tablosunu belirler.| UTF-8
        Sütun üstbilgileri| Veri kümesinin üst bilgilerinin (varsa) nasıl değerlendirileceğini gösterir.| Tüm dosyaların aynı üst bilgileri var
        Satırları atla | Veri kümesinde kaç tane, ne varsa satırların atlandığını gösterir.| Yok

    1. **Şema** formu, bu deneme için verilerinizin daha fazla yapılandırılmasını sağlar. Bu örnek için **day_of_week** özelliği için geçiş anahtarını seçin. bu nedenle, bu deneme için dahil edilmez. **İleri**’yi seçin.

        ![Önizleme sekmesi yapılandırması](./media/tutorial-first-experiment-automated-ml/schema-tab-config.gif)

    1. **Ayrıntıları Onayla** formunda, bilgilerin daha önce **temel bilgi** ve **Ayarlar ve önizleme** formlarında doldurulduğu ile eşleştiğini doğrulayın.
    1. Veri kümenizin oluşturulmasını gerçekleştirmek için **Oluştur** ' u seçin.
    1. Listede göründükten sonra veri kümenizi seçin.
    1. **Day_of_week** dahil etmediğinizden emin olmak için **veri önizlemeyi** gözden geçirin ve **Tamam**' ı seçin.

    1. **İleri**’yi seçin.

1. **Yapılandırma çalıştırması** formunu aşağıdaki gibi doldurun:
    1. Bu deneme adını girin: `my-1st-automl-experiment`

    1. Hedef sütun olarak **y** ' yi, ne tahmin etmek istediğinizi seçin. Bu sütun, istemcinin yatırma bir terime abone olup olmadığını gösterir.
    1. **Yeni Işlem oluştur** ' u seçin ve işlem hedefini yapılandırın. İşlem hedefi, eğitim betiğinizi çalıştırmak veya hizmet dağıtımınızı barındırmak için kullanılan yerel veya bulut tabanlı bir kaynak ortamıdır. Bu deneme için bulut tabanlı bir işlem kullanıyoruz. 

        Alan | Açıklama | Öğretici için değer
        ----|---|---
        İşlem adı |İşlem bağlamını tanımlayan benzersiz bir ad.|Oto ml-işlem
        Sanal&nbsp;makine&nbsp;boyutu| İşlem için sanal makine boyutunu seçin.|Standard_DS12_V2
        En az/en fazla düğüm (Gelişmiş ayarlarda)| Veri profili için, 1 veya daha fazla düğüm belirtmeniz gerekir.|En az düğümler: 1<br>En fazla düğüm: 6
  
        1. İşlem hedefini almak için **Oluştur** ' u seçin. 

            **Bu, tamamlanacak birkaç dakika sürer.** 

        1. Oluşturulduktan sonra, açılan listeden yeni işlem hedefini seçin.

    1. **İleri**’yi seçin.

1. **Görev türü ve ayarlar** formunda makine öğrenimi görev türü olarak **Sınıflandırma** ' yı seçin.

    1. **Ek yapılandırma ayarlarını görüntüle** ' yi seçin ve alanları aşağıdaki gibi doldurun. Bu ayarlar, eğitim işini daha iyi denetliyor. Aksi takdirde, denemeler seçimine ve verilerine göre varsayılan ayarlar uygulanır.

        >[!NOTE]
        > Bu öğreticide, yineleme eşiğine göre ölçüm puanı veya en fazla çekirdek ayarlayamayacağız. Ya da algoritmaların test edilmeye engel olursunuz.
   
        Ek&nbsp;yapılandırması|Açıklama|&nbsp;öğreticisi için değer&nbsp;
        ------|---------|---
        Birincil ölçüm| Makine öğrenimi algoritmasının ölçülecek değerlendirme ölçümü.|AUC_weighted
        Otomatik olarak korleştirme| Ön işleme etkinleştirilir. Bu, yapay özellikler oluşturmak için otomatik veri temizleme, hazırlama ve dönüştürmeyi içerir.| Etkinleştirme
        Engellenen algoritmalar | Eğitim işinden dışlamak istediğiniz algoritmalar| Yok
        Çıkış ölçütü| Bir kriterle karşılanırsa eğitim işi durdurulur. |Eğitim&nbsp;işi&nbsp;süresi (saat): 1 <br> Ölçüm&nbsp;puanı&nbsp;eşiği: yok
        Doğrulama | Çapraz doğrulama türü ve test sayısı seçin.|Doğrulama türü:<br>çapraz doğrulama&nbsp;&nbsp;k katlama <br> <br> Doğrulama sayısı: 2
        Eşzamanlılık| Yineleme başına yürütülen en fazla paralel yineleme sayısı| En fazla&nbsp;eşzamanlı&nbsp;yineleme: 5
        
        **Kaydet**’i seçin.

1. Denemeyi çalıştırmak için **son** ' u seçin. **Çalışma ayrıntısı** ekranı, deneme hazırlığı başladığında en üstteki **çalıştırma durumuyla** birlikte açılır.

>[!IMPORTANT]
> Hazırlık, deneme çalıştırmasının hazırlanmasına **10-15 dakika** sürer.
> Çalışmaya başladıktan sonra, **her yinelemede 2-3 dakika daha**sürer.  
> Deneme ilerledikçe çalıştırmanın durumunu görmek için düzenli aralıklarla **Yenile** ' yi seçin.
>
> Üretimde, büyük olasılıkla biraz daha fazla yol göstereceğiz. Ancak bu öğreticide, diğer kullanıcılar çalışmaya devam ederken, **modeller** sekmesinde sınanan algoritmaları keşfetmeye başlayacağız. 

##  <a name="explore-models"></a>Modelleri keşfet

Test edilen algoritmaları (modeller) görmek için **modeller** sekmesine gidin. Modeller, varsayılan olarak, tamamlandığı gibi ölçüm puanına göre sıralanır. Bu öğretici için, seçili **AUC_weighted** ölçüsüne göre en yüksek düzeyde puan veren model listenin en üstünde yer alır.

Deneme modellerinin tümünün bitmesini beklerken, performans ayrıntılarını araştırmak için tamamlanmış bir modelin **algoritma adını** seçin. 

Aşağıdaki model ayrıntıları, ölçümleri ve performans grafiklerini görüntülemek için **model ayrıntıları** ve **görselleştirmeler** sekmelerinde gezinir. 

![Yineleme ayrıntısı Çalıştır](./media/tutorial-first-experiment-automated-ml/run-detail.gif)

## <a name="deploy-the-best-model"></a>En iyi modeli dağıtma

Otomatik makine öğrenimi arabirimi, birkaç adımda Web hizmeti olarak en iyi modeli dağıtmanıza olanak tanır. Dağıtım, yeni verileri tahmin etmek ve potansiyel fırsat bölgelerini belirlemek için modelin Tümleştirmesidir. 

Bu deneme için, bir Web hizmetine dağıtım, artık, potansiyel sabit depozito müşterilerinin tanımlanmasından sonra, mali kurumda yinelenen ve ölçeklenebilir bir Web çözümüne sahip olduğu anlamına gelir. 

Çalıştırma tamamlandıktan sonra, **ayrıntıları Çalıştır** sayfasına dönün ve **modeller** sekmesini seçin.

Bu deneme bağlamında, **Votingensediskte** **AUC_weighted** ölçüsüne göre en iyi model kabul edilir.  Bu modeli dağıyoruz ancak yapmanız önerilir, dağıtımın tamamlaması yaklaşık 20 dakika sürer. Dağıtım işlemi, modeli kaydetme, kaynakları oluşturma ve bunları Web hizmeti için yapılandırma dahil olmak üzere birkaç adım gerektirir.

1. Sol alt köşedeki **en iyi modeli dağıt** düğmesini seçin.

1. **Model dağıt** bölmesini aşağıdaki gibi doldurun:

    Alan| Değer
    ----|----
    Dağıtım adı| My-Oto ml-Deploy
    Dağıtım açıklaması| İlk otomatik makine öğrenimi deneme dağıtımı
    İşlem türü | Azure Işlem örneği (acı) seçin
    Kimlik doğrulamasını etkinleştir| Dıı. 
    Özel dağıtımlar kullanın| Dıı. Varsayılan sürücü dosyası (Puanlama betiği) ve ortam dosyasının yeniden oluşturulmasına izin verir. 
    
    Bu örnekte, *Gelişmiş* menüsünde belirtilen Varsayılanları kullanırız. 

1. **Dağıt**'ı seçin.  

    **Çalıştır** ekranının üst kısmında yeşil başarı iletisi görünür ve **Önerilen model** bölmesinde, **dağıtım durumu**altında bir durum iletisi görüntülenir. Dağıtım durumunu denetlemek için düzenli aralıklarla **Yenile** ' yi seçin.
    
Artık tahminleri oluşturmak için işlemsel bir Web hizmetiniz vardır. 

Yeni Web hizmetinizi kullanma hakkında daha fazla bilgi edinmek için [**sonraki adımlara**](#next-steps) ilerleyin ve Power BI yerleşik Azure Machine Learning desteğini kullanarak tahminlerinizi test edin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Dağıtım dosyaları veri ve deneme dosyalarından daha büyüktür, bu nedenle daha fazla depolama alanı maliyetlidir. Hesap maliyetlerini en aza indirmek için yalnızca dağıtım dosyalarını silin veya çalışma alanınızı ve deneme dosyalarını korumak istiyorsanız. Aksi takdirde, herhangi bir dosyayı kullanmayı planlamıyorsanız tüm kaynak grubunu silin.  

### <a name="delete-the-deployment-instance"></a>Dağıtım örneğini silme

Diğer öğreticiler ve araştırmayla ilgili kaynak grubunu ve çalışma alanını tutmak istiyorsanız, yalnızca https://ml.azure.com/ konumundaki Azure Machine Learning dağıtım örneğini silin. 

1. https://ml.azure.com/Azure Machine Learning gidin. Çalışma alanınıza gidin ve **varlıklar** bölmesinin sol tarafında **uç noktalar**' ı seçin. 

1. Silmek istediğiniz dağıtımı seçin ve **Sil**' i seçin. 

1. **Devam**' ı seçin.

### <a name="delete-the-resource-group"></a>Kaynak grubunu silme

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu otomatik makine öğrenimi öğreticisinde, bir sınıflandırma modeli oluşturmak ve dağıtmak için Azure Machine Learning otomatik ML arabirimini kullandınız. Daha fazla bilgi ve sonraki adımlar için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Web hizmeti kullanma](how-to-consume-web-service.md#consume-the-service-from-power-bi)

+ [Otomatik makine öğrenimi](concept-automated-ml.md)hakkında daha fazla bilgi edinin.
+ Sınıflandırma ölçümleri ve grafikler hakkında daha fazla bilgi için bkz. [otomatik makine öğrenimi sonuçlarını anlama](how-to-understand-automated-ml.md#classification) makalesi. + [korleştirme](how-to-use-automated-ml-for-ml-models.md#featurization)hakkında daha fazla bilgi edinin.
+ [Veri profili oluşturma](how-to-use-automated-ml-for-ml-models.md#profile)hakkında daha fazla bilgi edinin.


>[!NOTE]
> Bu banka pazarlama veri kümesi, [Creative Commons (CCO: genel etki alanı) lisansı](https://creativecommons.org/publicdomain/zero/1.0/)altında kullanılabilir hale getirilir. Veritabanının bireysel içeriklerinde her türlü hak, [veritabanı Içeriği lisansı](https://creativecommons.org/publicdomain/zero/1.0/) kapsamında lisanslanır ve [kada](https://www.kaggle.com/janiobachmann/bank-marketing-dataset)mevcuttur. Bu veri kümesi, ilk olarak [UCI Machine Learning veritabanı](https://archive.ics.uci.edu/ml/datasets/bank+marketing)dahilinde kullanılabilir.<br><br>
> [Moro et al., 2014] S. Moro, P. Cortez ve P. Rita. Banka telefonla Bansının başarısını tahmin etmeye yönelik veri odaklı bir yaklaşım. Karar destek sistemleri, Elsevier, 62:22-31 Haziran 2014.
