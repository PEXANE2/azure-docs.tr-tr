---
title: Otomatik ML sınıflandırma modelleri oluşturun
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'in otomatik makine öğrenimi (otomatik ML) arabirimiyle sınıflandırma modellerini nasıl eğitecek & dağıtacaklarını öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: cartacioS
ms.author: sacartac
ms.reviewer: nibaccam
ms.date: 03/04/2020
ms.openlocfilehash: b5a335a3f215ad5883b1b223245ca9d3f9967c3b
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366518"
---
# <a name="tutorial-create-a-classification-model-with-automated-ml-in-azure-machine-learning"></a>Öğretici: Azure Machine Learning'de otomatik ML ile bir sınıflandırma modeli oluşturun
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Bu eğitimde, Azure Machine Learning'in otomatik makine öğrenimi arabirimini kullanarak tek bir kod satırı yazmadan temel bir sınıflandırma modeli oluşturmayı öğrenirsiniz. Bu sınıflandırma modeli, bir müşterinin bir finans kurumuna sabit vadeli bir mevduat abonesi olup olmadığını öngörmektedir.

Otomatik makine öğrenimi ile, zaman yoğun görevleri otomatikleştirebilirsiniz. Otomatik makine öğrenimi, seçtiğiniz başarı ölçütüne göre en iyi modeli bulmanıza yardımcı olmak için birçok algoritma ve hiperparametre kombinasyonuüzerinde hızla yineler.

Bu öğreticide, aşağıdaki görevleri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Azure Machine Learning çalışma alanı oluşturun.
> * Otomatik makine öğrenmesi denemesi çalıştırma.
> * Deneme ayrıntılarını görüntüleyin.
> * Modeli dağıtma.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://aka.ms/AMLFree)oluşturun.

* [**bankmarketing_train.csv**](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) veri dosyasını indirin. **Y** sütunu, bir müşterinin daha sonra bu öğreticideki öngörüler için hedef sütun olarak tanımlanan sabit vadeli bir depozitoya abone olup olmadığını gösterir. 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Azure Machine Learning çalışma alanı, makine öğrenimi modellerini denemek, eğitmek ve dağıtmak için bulutta kullandığınız temel bir kaynaktır. Azure aboneliğinizi ve kaynak grubunuzu hizmette kolayca tüketilen bir nesneye bağlar. 

Azure kaynaklarınızı yönetmek için web tabanlı bir konsol olan Azure portalı üzerinden bir çalışma alanı oluşturursunuz.

[!INCLUDE [aml-create-portal](../../includes/aml-create-in-portal-enterprise.md)]

>[!IMPORTANT] 
> **Çalışma alanınızı** ve **aboneliğinizi**not alın. Denemenizi doğru yerde oluşturduğunuzdan emin olmak için bunlara ihtiyacınız olacak. 

## <a name="create-and-run-the-experiment"></a>Denemeoluşturma ve çalıştırma

Tüm beceri düzeylerindeki veri bilimi uygulayıcıları için https://ml.azure.comveri bilimi senaryoları gerçekleştirmek için makine öğrenimi araçlarını içeren birleştirilmiş web arabirimi olan Azure Machine learning aracılığıyla aşağıdaki deneme kurulum ve çalıştırma adımlarını tamamlarsınız. Bu arabirim Internet Explorer tarayıcılarında desteklenmez.

1. [Azure Machine Learning'de](https://ml.azure.com)oturum açın.

1. Aboneliğinizi ve oluşturduğunuz çalışma alanını seçin.

1. **Başlat'ı**seçin.

1. Sol bölmede, **Yazar** bölümünün altındaki **Otomatik ML'yi** seçin.

   Bu ilk otomatik ML deneme olduğundan, boş bir liste ve belgelere bağlantılar görürsünüz.

   ![Başlarken sayfası](./media/tutorial-first-experiment-automated-ml/get-started.png)

1. **Yeni otomatik ML çalıştır'ı**seçin. 

1. **+Create dataset** açılır tarihinden **yerel dosyalardan** birini seçerek yeni bir veri kümesi oluşturun. 

    1. Temel **bilgi** formunda, veri setinize bir ad verin ve isteğe bağlı bir açıklama sağlayın. Otomatik ML arabirimi şu anda yalnızca TabularDatasets destekler, bu nedenle veri kümesi türü *Tabular*varsayılan olmalıdır.

    1. Sol altta **İleri'yi** seçin

    1. **Datastore ve dosya seçim** formunda, çalışma alanı oluşturma sırasında otomatik olarak ayarlanan varsayılan veri deposunu, **çalışma alanıblobstore'u (Azure Blob Depolama)** seçin. Burası, çalışma alanınızın kullanılabilir hale getirmek için veri dosyanızı yükleyeceğiniz yerdir.

    1. **Gözat**'ı seçin.
    
    1. Yerel bilgisayarınızdaki **bankmarketing_train.csv** dosyasını seçin. Bu, [ön koşul](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv)olarak indirdiğiniz dosyadır.

    1. Veri setinize benzersiz bir ad verin ve isteğe bağlı bir açıklama sağlayın. 

    1. Sol **altta,** çalışma alanı oluşturma sırasında otomatik olarak ayarlanan varsayılan kapsayıcıya yüklemek için Sol altta İleri'yi seçin.  
    
       Yükleme tamamlandığında, Ayarlar ve önizleme formu dosya türüne bağlı olarak önceden doldurulur. 
       
    1. **Ayarlar ve önizleme** formunun aşağıdaki gibi doldurulup doldurulan ı doğrulayın ve **İleri'yi**seçin.
        
        Alan|Açıklama| Öğretici için değer
        ---|---|---
        Dosya biçimi|Bir dosyada depolanan verilerin düzenini ve türünü tanımlar.| Sınırlandırılmış
        Sınırlayıcı|Düz metinde veya diğer veri&nbsp; akışlarında ayrı, bağımsız bölgeler arasındaki sınırı belirtmek için bir veya daha fazla karakter. |Virgül
        Encoding|Veri kümenizi okumak için kullanılacak şema tablosunun hangi bitini tanımlar.| UTF-8
        Sütun başlıkları| Varsa veri kümesinin üstbilgilerinin nasıl işleneceklerini gösterir.| Tüm dosyalar aynı üstbilgiye sahiptir
        Satırları atla | Veri kümesinde kaç satırAtıldı gösterir.| None

    1. **Şema** formu, bu deneme için verilerinizin daha fazla yapılandırmasına olanak tanır. Bu örnekte, bu denemeiçin içermemek için **day_of_week** özelliği için geçiş anahtarını seçin. **Sonraki'ni**seçin.

        ![Önizleme sekmesi yapılandırması](./media/tutorial-first-experiment-automated-ml/schema-tab-config.gif)

    1. Ayrıntıları **Onayla** formunda, bilgilerin **Temel bilgiler** ve Ayarlar ve **önizleme** formlarında daha önce doldurulan bilgilerle eşleştiğini doğrulayın.
    1. Veri setinizin oluşturulmasını tamamlamak için **Oluştur'u** seçin.
    1. Listede göründükten sonra veri kümenizi seçin.
    1. Day_of_week sonra **eklemediğinizden** emin olmak için **Veri önizlemesini** gözden geçirin, **Tamam'ı**seçin.

    1. **Sonraki'ni**seçin.

1. **Yapılışı Çalıştır** formunu aşağıdaki gibi doldurun:
    1. Bu deneme adını girin:`my-1st-automl-experiment`

    1. Tahmin etmek istediğiniz hedef sütun olarak **y'yi** seçin. Bu sütun, istemcinin vadeli bir depozitoya abone olup olmadığını gösterir.
    1. **Yeni bir bilgi işlem oluştur'u** seçin ve bilgi işlem hedefinizi yapılandırın. Bilgi işlem hedefi, eğitim komut dosyanızı çalıştırmak veya hizmet dağıtımınızı barındırmak için kullanılan yerel veya bulut tabanlı bir kaynak ortamıdır. Bu deneme için bulut tabanlı bir işlem kullanıyoruz. 

        Alan | Açıklama | Öğretici için değer
        ----|---|---
        İşlem adı |Bilgi işlem bağlamınızı tanımlayan benzersiz bir ad.|automl-compute
        Sanal&nbsp;&nbsp;makine boyutu| İşleminiz için sanal makine boyutunu seçin.|Standard_DS12_V2
        Min / Max düğümleri (Gelişmiş Ayarlar' da)| Profil verileri için 1 veya daha fazla düğüm belirtmeniz gerekir.|Min düğümleri: 1<br>Maksimum düğümler: 6
  
        1. İşlem hedefini almak için **Oluştur'u** seçin. 

            **Bu işlem birkaç dakika sürer.** 

        1. Oluşturmadan sonra, açılan listeden yeni işlem hedefinizi seçin.

    1. **Sonraki'ni**seçin.

1. Görev **türü ve ayarları** formunda, makine öğrenimi görev türü olarak **Sınıflandırma'yı** seçin.

    1. **Ek yapılandırma ayarlarını görüntüle'yi** seçin ve alanları aşağıdaki gibi doldurun. Bu ayarlar, eğitim işini daha iyi kontrol etmek içindir. Aksi takdirde, varsayılanlar deneme seçimi ve verilere göre uygulanır.

        >[!NOTE]
        > Bu öğreticide, yinelemeler başına bir metrik puan veya maksimum çekirdek belirlemezsiniz. Algoritmaların test edilmelerini de engellemezsiniz.
   
        Ek&nbsp;yapılandırmalar|Açıklama|Öğretici&nbsp;&nbsp;için değer
        ------|---------|---
        Birincil metrik| Makine öğrenme algoritmasının ölçüleceği değerlendirme ölçüsü.|AUC_weighted
        Otomatik featurization| Ön işleme sağlar. Bu, sentetik özellikler oluşturmak için otomatik veri temizleme, hazırlama ve dönüştürmeyi içerir.| Etkinleştirme
        Engellenen algoritmalar | Eğitim işinden hariç tutmak istediğiniz algoritmalar| None
        Çıkış kriteri| Bir ölçüt karşılanırsa, eğitim işi durdurulur. |Eğitim&nbsp;&nbsp;iş süresi (saat): 1 <br> Metrik&nbsp;&nbsp;puan eşiği: Yok
        Doğrulama | Çapraz doğrulama türü ve test sayısı seçin.|Doğrulama türü:<br>&nbsp;k-kat&nbsp;çapraz doğrulama <br> <br> Doğrulama sayısı: 2
        Eşzamanlılık| Yineleme başına yürütülen en fazla paralel yineleme sayısı| Maksimum&nbsp;eşzamanlı&nbsp;yinelemeler: 5
        
        **Kaydet'i**seçin.

1. Denemeyi çalıştırmak için **Finish'i** seçin. Deneme hazırlığı **başladığında, Ayrıntıyı Çalıştır** ekranı en üstte **Çalıştır durumuyla** açılır.

>[!IMPORTANT]
> Deneme koşusunu hazırlamak **10-15 dakika** sürer.
> Bir kez çalıştırdıktan sonra, **her yineleme için 2-3 dakika daha fazla**sürer.  
> Deneme ilerledikçe çalıştırmanın durumunu görmek için düzenli aralıklarla **Yenile'yi** seçin.
>
> Prodüksiyonda, büyük ihtimalle biraz yürüyüp gidersin. Ancak bu öğretici için, diğerleri hala çalışırken tamamlanırken **Modeller** sekmesinde test edilmiş algoritmaları keşfetmeye başlamanızı öneririz. 

##  <a name="explore-models"></a>Modelleri keşfedin

Algoritmaların (modellerin) test edilmiş olduğunu görmek için **Modeller** sekmesine gidin. Varsayılan olarak, modeller tamamlandıkça metrik puana göre sıralanır. Bu öğretici için, seçilen **AUC_weighted** ölçümüne göre en yüksek puanı alan model listenin en üstündedir.

Tüm deneme modellerinin tamamlanmasını beklerken, performans ayrıntılarını keşfetmek için tamamlanmış bir modelin **Algoritma adını** seçin. 

Aşağıdaki, seçili modelin özelliklerini, ölçümlerini ve performans grafiklerini görüntülemek için **Model ayrıntıları** ve **Görselleştirmeler** sekmelerinde gezinir. 

![Yineleme ayrıntılarını çalıştırma](./media/tutorial-first-experiment-automated-ml/run-detail.gif)

## <a name="deploy-the-best-model"></a>En iyi modeli dağıtın

Otomatik makine öğrenme arayüzü birkaç adımda bir web hizmeti olarak en iyi modeli dağıtmak için izin verir. Dağıtım, yeni veriler üzerinde tahmin de yapabilmesi ve olası fırsat alanlarını belirleyebilmeleri için modelin entegrasyonudur. 

Bu deneme için, bir web hizmetine dağıtım, finans kurumunun artık potansiyel sabit vadeli mevduat müşterilerini tanımlamak için yinelemeli ve ölçeklenebilir bir web çözümüne sahip olduğu anlamına gelir. 

Çalıştırma tamamlandıktan **sonra, Ayrıntıyı Çalıştır** sayfasına geri gidin ve **Modeller** sekmesini seçin.

Bu deneme bağlamında, **VotingEnsemble** **AUC_weighted** metrik dayalı en iyi model olarak kabul edilir.  Bu modeli dağıtıyoruz, ancak dağıtımın tamamlanması yaklaşık 20 dakika sürüyor. Dağıtım işlemi, modeli kaydetme, kaynak oluşturma ve bunları web hizmeti için yapılandırma dahil olmak üzere birkaç adım gerektirir.

1. Sol alt köşedeki **en iyi modeli dağıt** düğmesini seçin.

1. Bir model bölmesini aşağıdaki gibi **dağıtın:**

    Alan| Değer
    ----|----
    Dağıtım adı| my-automl-deploy
    Dağıtım açıklaması| İlk otomatik makine öğrenimi deneme dağıtımım
    İşlem türü | Azure İşlem Örneği (ACI) seçin
    Kimlik doğrulamayı etkinleştirme| Devre dışı bırakmak. 
    Özel dağıtımları kullanma| Devre dışı bırakmak. Varsayılan sürücü dosyasının (komut dosyası puanlama) ve ortam dosyasının otomatik olarak oluşturulmasına izin verir. 
    
    Bu örnekte, *Gelişmiş* menüde sağlanan varsayılanları kullanırız. 

1. **Dağıt**'ı seçin.  

    **Çalıştır** ekranının üst kısmında yeşil bir başarı iletisi ve **Önerilen model** bölmesinde, **Dağıt durumu**altında bir durum iletisi görüntülenir. Dağıtım durumunu kontrol etmek için düzenli aralıklarla **Yenile'yi** seçin.
    
Şimdi tahminler oluşturmak için operasyonel bir web hizmeti var. 

Yeni web hizmetinizi nasıl tüketirdiye daha fazla bilgi edinmek için [**Sonraki Adımlar'a**](#next-steps) gidin ve Power BI'nin Azure Machine Learning desteğinde yerleşik olarak tasarlanan öngörülerinizi test edin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Dağıtım dosyaları veri ve deneme dosyalarını daha büyüktür, bu nedenle depolamak için daha pahalıdır. Hesabınızdaki maliyetleri en aza indirmek veya çalışma alanınızı ve deneme dosyalarınızı tutmak istiyorsanız yalnızca dağıtım dosyalarını silin. Aksi takdirde, dosyalardan herhangi birini kullanmayı düşünmüyorsanız, kaynak grubunun tamamını silin.  

### <a name="delete-the-deployment-instance"></a>Dağıtım örneğini silme

Kaynak grubunu ve çalışma alanını diğer\/öğreticiler ve keşifler için tutmak istiyorsanız, https:/ml.azure.com/ adresindeki Azure Machine Learning'den dağıtım örneğini silin. 

1. Azure [Machine Learning'e](https://ml.azure.com/)gidin. Çalışma alanınıza gidin ve **Varlıklar** bölmesinin altında **solda, Uç Noktaları'nı**seçin. 

1. Silmek istediğiniz dağıtımı seçin ve **Sil'i**seçin. 

1. **Devam Et'i**seçin.

### <a name="delete-the-resource-group"></a>Kaynak grubunu silme

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu otomatik makine öğrenimi öğreticisinde, bir sınıflandırma modeli oluşturmak ve dağıtmak için Azure Machine Learning'in otomatik ML arabirimini kullandınız. Daha fazla bilgi ve sonraki adımlar için bu makalelere bakın:

> [!div class="nextstepaction"]
> [Bir web hizmetini kullanma](how-to-consume-web-service.md#consume-the-service-from-power-bi)

+ [Otomatik makine öğrenimi](concept-automated-ml.md)hakkında daha fazla bilgi edinin.
+ Sınıflandırma ölçümleri ve grafiklerhakkında daha fazla bilgi için otomatik [makine öğrenimi sonuçlarını anlayın](how-to-understand-automated-ml.md#classification) makalesine bakın.+ [Featurization](how-to-use-automated-ml-for-ml-models.md#featurization)hakkında daha fazla bilgi edinin.
+ [Veri profilleme](how-to-use-automated-ml-for-ml-models.md#profile)hakkında daha fazla bilgi edinin.


>[!NOTE]
> Bu Banka Pazarlama veri seti [Creative Commons (CCO: Public Domain) Lisansı](https://creativecommons.org/publicdomain/zero/1.0/)altında kullanıma sunulmuştur. Veritabanının bireysel içeriğindeki tüm haklar [Veritabanı İçeriği Lisansı](https://creativecommons.org/publicdomain/zero/1.0/) altında lisanslanmıştır ve [Kaggle'da](https://www.kaggle.com/janiobachmann/bank-marketing-dataset)mevcuttur. Bu veri seti ilk olarak [UCI Machine Learning Database'te](https://archive.ics.uci.edu/ml/datasets/bank+marketing)mevcuttu.<br><br>
> [Moro ve ark., 2014] S. Moro, P. Cortez ve P. Rita. A Data-Driven Approach to Predict the Success of Bank Telemarketing. Decision Support Systems, Elsevier, 62:22-31, Haziran 2014.
