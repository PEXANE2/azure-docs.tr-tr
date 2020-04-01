---
title: Otomatik ML denemesi ile tahmin bisiklet paylaşımı talebi
titleSuffix: Azure Machine Learning
description: Azure Machine Learning stüdyosunda otomatik makine öğrenimi ile bir talep tahmin modelini nasıl eğitip dağıtılayarak nasıl dağıtılamayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: sacartac
ms.reviewer: nibaccam
author: cartacioS
ms.date: 01/27/2020
ms.openlocfilehash: 11e0a8a0076fb2e68c379b279f471ff74846df2e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77088245"
---
# <a name="tutorial-forecast-bike-sharing-demand-with-automated-machine-learning"></a>Öğretici: Otomatik makine öğrenimi ile tahmin bisiklet paylaşımı talebi
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Bu eğitimde, azure machine learning stüdyosunda otomatik makine öğrenimi veya otomatik ML kullanarak bisiklet paylaşım hizmetiiçin kiralama talebini tahmin etmek için bir zaman serisi tahmin modeli oluşturursunuz.

Bu öğreticide, aşağıdaki görevleri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir veri kümesi oluşturun ve yükleyin.
> * Otomatik bir ML denemeyi yapılandırın ve çalıştırın.
> * Deneme sonuçlarını keşfedin.
> * En iyi modeli dağıtın.

## <a name="prerequisites"></a>Ön koşullar

* Kurumsal sürüm Azure Makine Öğrenimi çalışma alanı. Çalışma alanınız yoksa, Kurumsal [sürüm çalışma alanı oluşturun.](how-to-manage-workspace.md) 
    * Azure Machine Learning stüdyosunda otomatik makine öğrenimi yalnızca Enterprise sürümü çalışma alanları için kullanılabilir. 
* [Bike-no.csv](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-bike-share/bike-no.csv) veri dosyasını indirin

## <a name="get-started-in-azure-machine-learning-studio"></a>Azure Machine Learning stüdyosunda işe başlayın

Bu öğretici için, tüm beceri düzeylerindeki veri bilimi uygulayıcıları için veri bilimi senaryoları gerçekleştirmek için makine öğrenimi araçlarını içeren birleştirilmiş arabirim olan Azure Machine Learning stüdyosunda otomatik ML deneme nizi oluşturursunuz. Stüdyo Internet Explorer tarayıcılarında desteklenmez.

1. [Azure Machine Learning stüdyosunda](https://ml.azure.com)oturum açın.

1. Aboneliğinizi ve oluşturduğunuz çalışma alanını seçin.

1. **Başlat'ı**seçin.

1. Sol bölmede, **Yazar** bölümünün altındaki **Otomatik ML'yi** seçin.

1. **+Yeni otomatik ML çalıştır'ı**seçin. 

## <a name="create-and-load-dataset"></a>Veri kümesi oluşturma ve yükleme

Denemenizi yapılandırmadan önce, veri dosyanızı azure machine learning veri kümesi biçiminde çalışma alanınıza yükleyin. Bunu yapmak, verilerinizin denemeniz için uygun biçimlendirilmiş olduğundan emin olabilirsiniz.

1. Veri **kümesini Seç** formunda,+Create **dataset** açılır dosyasından **yerel dosyalardan** seçin. 

    1. Temel **bilgi** formunda, veri setinize bir ad verin ve isteğe bağlı bir açıklama sağlayın. Azure Machine Learning stüdyosundaki otomatik ML şu anda yalnızca tabular veri kümelerini desteklediğinden, veri kümesi türü **Tabular**varsayılan olmalıdır.
    
    1. Sol altta **İleri'yi** seçin

    1. **Datastore ve dosya seçim** formunda, çalışma alanı oluşturma sırasında otomatik olarak ayarlanan varsayılan veri deposunu, **çalışma alanıblobstore'u (Azure Blob Depolama)** seçin. Bu, veri dosyanızı yükleyeceğiniz depolama konumudur. 

    1. **Gözat**'ı seçin. 
    
    1. Yerel bilgisayarınızdaki **bisiklet no.csv** dosyasını seçin. Bu, [ön koşul](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-bike-share/bike-no.csv)olarak indirdiğiniz dosyadır.

    1. **Sonraki'ni** Seçin

       Yükleme tamamlandığında, Ayarlar ve önizleme formu dosya türüne bağlı olarak önceden doldurulur. 
       
    1. **Ayarlar ve önizleme** formunun aşağıdaki gibi doldurulup doldurulan ı doğrulayın ve **İleri'yi**seçin.
        
        Alan|Açıklama| Öğretici için değer
        ---|---|---
        Dosya biçimi|Bir dosyada depolanan verilerin düzenini ve türünü tanımlar.| Sınırlandırılmış
        Sınırlayıcı|Düz metinde veya diğer veri&nbsp; akışlarında ayrı, bağımsız bölgeler arasındaki sınırı belirtmek için bir veya daha fazla karakter. |Virgül
        Encoding|Veri kümenizi okumak için kullanılacak şema tablosunun hangi bitini tanımlar.| UTF-8
        Sütun başlıkları| Varsa veri kümesinin üstbilgilerinin nasıl işleneceklerini gösterir.| İlk dosyadaki üstbilgi kullanma
        Satırları atla | Veri kümesinde kaç satırAtıldı gösterir.| None

    1. **Şema** formu, bu deneme için verilerinizin daha fazla yapılandırmasına olanak tanır. 
    
        1. Bu örnekte, **gündelik** ve **kayıtlı** sütunları yoksaymayı seçin. Bu sütunlar **cnt** sütununun bir dökümü, bu nedenle bunları dahil etmiyoruz.

        1. Ayrıca bu örnek için, **Özellikler** ve **Türü**için varsayılanbırakın. 
        
        1. **Sonraki'ni**seçin.

    1. Ayrıntıları **Onayla** formunda, bilgilerin **Temel bilgiler** ve Ayarlar ve **önizleme** formlarında daha önce doldurulan bilgilerle eşleştiğini doğrulayın.

    1. Veri setinizin oluşturulmasını tamamlamak için **Oluştur'u** seçin.

    1. Listede göründükten sonra veri kümenizi seçin.

    1. **Sonraki'ni**seçin.

## <a name="configure-experiment-run"></a>Deneme çalışmasını yapılandırma

Verilerinizi yükledikten ve yapılandırmadıktan sonra, uzak bilgi işlem hedefinizi ayarlayın ve verilerinizde tahmin etmek istediğiniz sütunu seçin.

1. **Yapılışı çalıştır biçimini** aşağıdaki gibi doldurun:
    1. Bir deneme adı girin:`automl-bikeshare`

    1. Hedef sütun olarak **cnt'yi** seçin, tahmin etmek istediğiniz şeyi. Bu sütun, toplam bisiklet payı kiralama sayısını gösterir.

    1. **Yeni bir bilgi işlem oluştur'u** seçin ve bilgi işlem hedefinizi yapılandırın. Otomatik ML yalnızca Azure Machine Learning bilgi işlem bilginliğini destekler. 

        Alan | Açıklama | Öğretici için değer
        ----|---|---
        İşlem adı |Bilgi işlem bağlamınızı tanımlayan benzersiz bir ad.|bisiklet-compute
        Sanal&nbsp;&nbsp;makine boyutu| İşleminiz için sanal makine boyutunu seçin.|Standard_DS12_V2
        Min / Max düğümleri (Gelişmiş Ayarlar' da)| Profil verileri için 1 veya daha fazla düğüm belirtmeniz gerekir.|Min düğümleri: 1<br>Maksimum düğümler: 6
  
        1. İşlem hedefini almak için **Oluştur'u** seçin. 

            **Bu işlem birkaç dakika sürer.** 

        1. Oluşturmadan sonra, açılan listeden yeni işlem hedefinizi seçin.

    1. **Sonraki'ni**seçin.

## <a name="select-task-type-and-settings"></a>Görev türünü ve ayarlarını seçin

Makine öğrenimi görev türünü ve yapılandırma ayarlarını belirterek otomatik ML denemenizin kurulumunu tamamlayın.

1. Görev **türü ve ayarları** formunda, makine öğrenimi görev türü olarak **Zaman serisi tahmini'ni** seçin.

1. **Saati** Saat **sütununolarak** seçin ve **Grubu sütuna göre** boş bırakın. 

    1. **Ek yapılandırma ayarlarını görüntüle'yi** seçin ve alanları aşağıdaki gibi doldurun. Bu ayarlar, eğitim işini daha iyi kontrol etmek içindir. Aksi takdirde, varsayılanlar deneme seçimi ve verilere göre uygulanır.

  
        Ek&nbsp;yapılandırmalar|Açıklama|Öğretici&nbsp;&nbsp;için değer
        ------|---------|---
        Birincil metrik| Makine öğrenme algoritmasının ölçüleceği değerlendirme ölçüsü.|Normalleştirilmiş kök ortalama kare hata
        Otomatik featurization| Ön işleme sağlar. Bu, sentetik özellikler oluşturmak için otomatik veri temizleme, hazırlama ve dönüştürmeyi içerir.| Etkinleştirme
        En iyi modeli açıklayın (önizleme)| Otomatik ML tarafından oluşturulan en iyi modelde açıklanabilirliği otomatik olarak gösterir.| Etkinleştirme
        Engellenen algoritmalar | Eğitim işinden hariç tutmak istediğiniz algoritmalar| Aşırı Rastgele Ağaçlar
        Ek tahmin ayarları| Bu ayarlar, modelinizin doğruluğunu artırmaya yardımcı olur <br><br> _**Tahmin ufku**_: tahmin etmek istediğiniz geleceğe doğru zaman uzunluğu <br> _**Tahmin hedefi gecikmeleri:**_ hedef değişkenin gecikmelerini ne kadar geriye yapmak istediğiniz <br> Hedef haddeleme penceresi : *üzerinde max, min* ve *toplam*gibi özelliklerin oluşturulacağı yuvarlanma _**penceresinin**_ boyutunu belirtir. |Tahmin ufku: 14 <br> Tahmin&nbsp;&nbsp;hedef gecikmeleri: Yok <br> Hedef&nbsp;&nbsp;haddeleme&nbsp;penceresi boyutu: Yok
        Çıkış kriteri| Bir ölçüt karşılanırsa, eğitim işi durdurulur. |Eğitim&nbsp;&nbsp;iş süresi (saat): 3 <br> Metrik&nbsp;&nbsp;puan eşiği: Yok
        Doğrulama | Çapraz doğrulama türü ve test sayısı seçin.|Doğrulama türü:<br>&nbsp;k-kat&nbsp;çapraz doğrulama <br> <br> Doğrulama sayısı: 5
        Eşzamanlılık| Yineleme başına yürütülen en fazla paralel yineleme sayısı| Maksimum&nbsp;eşzamanlı&nbsp;yinelemeler: 6
        
        **Kaydet'i**seçin.

## <a name="run-experiment"></a>Denemeyi çalıştır

Denemenizi çalıştırmak için **Finish'i**seçin. **Çalıştır ayrıntıları** ekranı, çalıştırma numarasının yanındaki üstteki **Çalıştır durumuyla** açılır. Deneme ilerledikçe bu durum güncellenir.

>[!IMPORTANT]
> Deneme koşusunu hazırlamak **10-15 dakika** sürer.
> Bir kez çalıştırdıktan sonra, **her yineleme için 2-3 dakika daha fazla**sürer.  <br> <br>
> Üretimde, bu işlem zaman aldığından, büyük olasılıkla bir süre için yürüyüp gitmek istiyorum. Beklerken, **modeller** sekmesinde test edilen algoritmaları tamamlandıkça keşfetmeye başlamanızı öneririz. 

##  <a name="explore-models"></a>Modelleri keşfedin

Algoritmaların (modellerin) test edilmiş olduğunu görmek için **Modeller** sekmesine gidin. Varsayılan olarak, modeller tamamlandıkça metrik puana göre sıralanır. Bu öğretici için, seçilen **Normalleştirilmiş kök ortalama kare hata** ölçümüne göre en yüksek puanları alan model listenin en üstündedir.

Tüm deneme modellerinin tamamlanmasını beklerken, performans ayrıntılarını keşfetmek için tamamlanmış bir modelin **Algoritma adını** seçin. 

Aşağıdaki örnek, seçili modelin özelliklerini, ölçümlerini ve performans grafiklerini görüntülemek için **Model ayrıntıları** ve **Görselleştirmeler** sekmelerinde gezinir. 

![Ayrıntıyı çalıştır](./media/tutorial-automated-ml-forecast/explore-models-ui.gif)

## <a name="deploy-the-model"></a>Modeli dağıtma

Azure Machine Learning stüdyosunda otomatik makine öğrenimi, en iyi modeli birkaç adımda web hizmeti olarak dağıtmanıza olanak tanır. Dağıtım, yeni veriler üzerinde tahmin de yapabilmesi ve olası fırsat alanlarını belirleyebilmeleri için modelin entegrasyonudur. 

Bu deneme için, bir web hizmetine dağıtım, bisiklet paylaşım şirketinin artık bisiklet payı kiralama talebini tahmin etmek için yinelemeli ve ölçeklenebilir bir web çözümüne sahip olduğu anlamına gelir. 

Çalıştırma tamamlandıktan **sonra, Ayrıntı Çalıştır** sayfasına geri gidin ve **Modeller** sekmesini seçin.

Bu deneme bağlamında **StackEnsemble,** **Normalleştirilmiş kök ortalaması kareli hata** ölçümüne dayalı olarak en iyi model olarak kabul edilir.  Bu modeli dağıtıyoruz, ancak dağıtımın tamamlanması yaklaşık 20 dakika sürüyor. Dağıtım işlemi, modeli kaydetme, kaynak oluşturma ve bunları web hizmeti için yapılandırma dahil olmak üzere birkaç adım gerektirir.

1. Sol alt köşedeki **en iyi modeli dağıt** düğmesini seçin.

1. Bir model bölmesini aşağıdaki gibi **dağıtın:**

    Alan| Değer
    ----|----
    Dağıtım adı| bikeshare-dağıtmak
    Dağıtım açıklaması| bisiklet payı talep dağıtımı
    İşlem türü | Azure İşlem Örneği (ACI) seçin
    Kimlik doğrulamayı etkinleştirme| Devre dışı bırakmak. 
    Özel dağıtım varlıklarını kullanma| Devre dışı bırakmak. Devre dışı bırakma, varsayılan sürücü dosyasının (komut dosyası puanlama) ve ortam dosyasının otomatik olarak oluşturulmasını sağlar. 
    
    Bu örnekte, *Gelişmiş* menüde sağlanan varsayılanları kullanırız. 

1. **Dağıt**'ı seçin.  

    **Çalıştır** ekranının üst kısmında yeşil bir başarı iletisi belirir ve dağıtımın başarıyla başlatıldı. Dağıtımın ilerlemesi bulunabilir  
    **Dağıt durumu**altında **Önerilen model** bölmesinde.
    
Dağıtım başarılı olduktan sonra, öngörüler oluşturmak için operasyonel bir web hizmetine sahip olursunuz. 

Yeni web hizmetinizi nasıl tüketirdiye daha fazla bilgi edinmek için [**Sonraki adımlara**](#next-steps) gidin ve Power BI'nin Azure Machine Learning desteğinde yerleşik olarak tasarlanan tahminlerini test edin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Dağıtım dosyaları veri ve deneme dosyalarını daha büyüktür, bu nedenle depolamak için daha pahalıdır. Hesabınızdaki maliyetleri en aza indirmek veya çalışma alanınızı ve deneme dosyalarınızı tutmak istiyorsanız yalnızca dağıtım dosyalarını silin. Aksi takdirde, dosyalardan herhangi birini kullanmayı düşünmüyorsanız, kaynak grubunun tamamını silin.  

### <a name="delete-the-deployment-instance"></a>Dağıtım örneğini silme

Kaynak grubunu ve çalışma alanını diğer öğreticiler ve keşifler için tutmak istiyorsanız, azure Machine Learning stüdyosundan dağıtım örneğini silin. 

1. [Azure Machine Learning stüdyosuna](https://ml.azure.com/)gidin. Çalışma alanınıza gidin ve **Varlıklar** bölmesinin altında **solda, Uç Noktaları'nı**seçin. 

1. Silmek istediğiniz dağıtımı seçin ve **Sil'i**seçin. 

1. **Devam Et'i**seçin.

### <a name="delete-the-resource-group"></a>Kaynak grubunu silme

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure Machine Learning stüdyosunda otomatik ML kullanarak bisiklet paylaşımı kiralama talebini öngören bir zaman serisi tahmin modeli oluşturup dağıttın. 

Yeni dağıtılan web hizmetinizin tüketimini kolaylaştırmak için Power BI destekli şema oluşturma adımlarını öğrenmek için bu makaleye bakın:

> [!div class="nextstepaction"]
> [Bir web hizmetini kullanma](how-to-consume-web-service.md#consume-the-service-from-power-bi)


>[!NOTE]
> Bu bisiklet paylaşımı veri kümesi bu öğretici için değiştirilmiştir. Bu dataset bir [Kaggle yarışmasının](https://www.kaggle.com/c/bike-sharing-demand/data) bir parçası olarak kullanılabilir hale getirildi ve başlangıçta [Capital Bikeshare](https://www.capitalbikeshare.com/system-data)üzerinden kullanılabilir oldu. Ayrıca [UCI Machine Learning Database](http://archive.ics.uci.edu/ml/datasets/Bike+Sharing+Dataset)içinde bulunabilir.<br><br>
> Kaynak: Fanaee-T, Hadi ve Gama, Joao, Topluluk dedektörleri ve arka plan bilgisi birleştiren olay etiketleme, Yapay Zeka İlerleme (2013): s. 1-15, Springer Berlin Heidelberg.
