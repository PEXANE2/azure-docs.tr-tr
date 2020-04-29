---
title: Otomatikleştirilmiş ML deneimiyle Bisiklet paylaşma talebini tahmin etme
titleSuffix: Azure Machine Learning
description: Azure Machine Learning Studio 'da otomatik makine öğrenimi ile talep tahmini modeli eğitme ve dağıtmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: sacartac
ms.reviewer: nibaccam
author: cartacioS
ms.date: 01/27/2020
ms.openlocfilehash: 11e0a8a0076fb2e68c379b279f471ff74846df2e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77088245"
---
# <a name="tutorial-forecast-bike-sharing-demand-with-automated-machine-learning"></a>Öğretici: otomatik makine öğrenimi ile Bisiklet paylaşma talebini tahmin etme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Bu öğreticide, bir bisiklet paylaşma hizmeti için Kiralama taleplerini tahmin etmek üzere bir zaman serisi tahmin modeli oluşturmak için Azure Machine Learning Studio 'da otomatik makine öğrenimi veya otomatik ML 'yi kullanırsınız.

Bu öğreticide, aşağıdaki görevleri nasıl gerçekleştireceğinizi öğreneceksiniz:

> [!div class="checklist"]
> * Bir veri kümesi oluşturun ve yükleyin.
> * Otomatik bir ML denemesi yapılandırın ve çalıştırın.
> * Deneme sonuçlarını keşfet.
> * En iyi modeli dağıtın.

## <a name="prerequisites"></a>Ön koşullar

* Enterprise Edition Azure Machine Learning çalışma alanı. Bir çalışma alanınız yoksa, [Enterprise Edition çalışma alanı oluşturun](how-to-manage-workspace.md). 
    * Azure Machine Learning Studio 'da otomatik makine öğrenimi yalnızca Enterprise Edition çalışma alanları için kullanılabilir. 
* [Bike-No. csv](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-bike-share/bike-no.csv) veri dosyasını indirin

## <a name="get-started-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio 'da çalışmaya başlama

Bu öğreticide, tüm beceri seviyelerinin veri bilimi senaryolarına yönelik veri bilimi senaryoları gerçekleştirmek için Machine Learning araçları 'nı içeren bir birleştirilmiş arabirim olan Azure Machine Learning Studio 'da otomatik ML denemenizin çalıştırılması oluşturursunuz. Studio, Internet Explorer tarayıcılarında desteklenmez.

1. [Azure Machine Learning Studio](https://ml.azure.com)'da oturum açın.

1. Aboneliğinizi ve oluşturduğunuz çalışma alanını seçin.

1. **Kullanmaya**başlayın ' ı seçin.

1. Sol bölmede **Yazar** bölümü altında **Otomatik ml** ' yi seçin.

1. **+ Yeni OTOMATIK ml Çalıştır**' ı seçin. 

## <a name="create-and-load-dataset"></a>Veri kümesi oluşturma ve yükleme

Denemenizi yapılandırmadan önce, veri dosyanızı Azure Machine Learning veri kümesi biçiminde çalışma alanınıza yükleyin. Bunun yapılması, verilerinizin denemenize uygun şekilde biçimlendirildiğinden emin olmanızı sağlar.

1. **Veri kümesi Seç** formunda, **+ veri kümesi oluştur** açılır listesinden **yerel dosyalar** ' ı seçin. 

    1. **Temel bilgi** formunda, veri kümenize bir ad verin ve isteğe bağlı bir açıklama sağlayın. Azure Machine Learning Studio 'daki otomatik ML Şu anda yalnızca tablo veri kümelerini desteklediğinden veri kümesi türü varsayılan olarak **tablosal**olmalıdır.
    
    1. Sol alt kısımdaki **İleri ' yi** seçin

    1. **Veri deposu ve dosya seçimi** formunda, çalışma alanı oluşturma, çalışma alanı **BlobStore (Azure Blob depolama)** sırasında otomatik olarak ayarlanan varsayılan veri deposunu seçin. Bu, veri dosyanızı karşıya yükleyeceğiniz depolama konumudur. 

    1. **Gözat**'ı seçin. 
    
    1. Yerel bilgisayarınızda **Bike-No. csv** dosyasını seçin. Bu, bir [Önkoşul](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-bike-share/bike-no.csv)olarak indirdiğiniz dosyadır.

    1. **İleri** Seç

       Karşıya yükleme tamamlandığında, ayarlar ve önizleme formu dosya türüne göre önceden doldurulur. 
       
    1. **Ayarlar ve önizleme** formunun aşağıdaki gibi doldurulduğunu doğrulayın ve **İleri ' yi**seçin.
        
        Alan|Açıklama| Öğretici için değer
        ---|---|---
        Dosya biçimi|Bir dosyada depolanan verilerin yerleşimini ve türünü tanımlar.| Ted
        Sınırlayıcı|Düz metin veya diğer veri akışlarında ayrı, bağımsız&nbsp; bölgeler arasındaki sınırı belirtmek için bir veya daha fazla karakter. |Virgül
        Encoding|Veri kümenizi okumak için kullanılacak bit karakter şeması tablosunu belirler.| UTF-8
        Sütun başlıkları| Veri kümesinin üst bilgilerinin (varsa) nasıl değerlendirileceğini gösterir.| İlk dosyadaki üst bilgileri kullan
        Satırları atla | Veri kümesinde kaç tane, ne varsa satırların atlandığını gösterir.| Hiçbiri

    1. **Şema** formu, bu deneme için verilerinizin daha fazla yapılandırılmasını sağlar. 
    
        1. Bu örnekte, **sıradan** ve **kayıtlı** sütunları yok saymayı seçin. Bu sütunlar, **sayisi** sütununun bir dökümününüdir, bu nedenle bunları dahil ettik.

        1. Ayrıca, bu örnek için, **Özellikler** ve **tür**için varsayılan değerleri bırakın. 
        
        1. **İleri**’yi seçin.

    1. **Ayrıntıları Onayla** formunda, bilgilerin daha önce **temel bilgi** ve **Ayarlar ve önizleme** formlarında doldurulduğu ile eşleştiğini doğrulayın.

    1. Veri kümenizin oluşturulmasını gerçekleştirmek için **Oluştur** ' u seçin.

    1. Listede göründükten sonra veri kümenizi seçin.

    1. **İleri ' yi**seçin.

## <a name="configure-experiment-run"></a>Deneme çalıştırmasını Yapılandır

Verilerinizi yükleyip yapılandırdıktan sonra, uzaktan işlem hedefini ayarlayın ve verilerinizde tahmin etmek istediğiniz sütunu seçin.

1. **Yapılandırma çalıştırması** formunu aşağıdaki gibi doldurun:
    1. Deneme adı girin:`automl-bikeshare`

    1. Hedef sütun olarak **sayisi** , ne tahmin etmek istediğinizi seçin. Bu sütun, toplam Bisiklet paylaşma sayısını gösterir.

    1. **Yeni Işlem oluştur** ' u seçin ve işlem hedefini yapılandırın. Otomatikleştirilmiş ML yalnızca Azure Machine Learning işlem destekler. 

        Alan | Açıklama | Öğretici için değer
        ----|---|---
        İşlem adı |İşlem bağlamını tanımlayan benzersiz bir ad.|Bisiklet-işlem
        Sanal&nbsp;makine&nbsp;boyutu| İşlem için sanal makine boyutunu seçin.|Standard_DS12_V2
        En az/en fazla düğüm (Gelişmiş ayarlarda)| Veri profili için, 1 veya daha fazla düğüm belirtmeniz gerekir.|En az düğümler: 1<br>En fazla düğüm: 6
  
        1. İşlem hedefini almak için **Oluştur** ' u seçin. 

            **Bu, tamamlanacak birkaç dakika sürer.** 

        1. Oluşturulduktan sonra, açılan listeden yeni işlem hedefini seçin.

    1. **İleri**’yi seçin.

## <a name="select-task-type-and-settings"></a>Görev türü ve ayarlarını seçin

Machine Learning görev türünü ve yapılandırma ayarlarını belirterek otomatik ML denemenizin kurulumunu doldurun.

1. **Görev türü ve ayarlar** formunda, makine öğrenimi görev türü olarak **zaman serisi tahmini** ' ni seçin.

1. **Zaman sütununuz** olarak **Tarih** ' i seçin ve **grubu sütunlara göre** boş bırakın. 

    1. **Ek yapılandırma ayarlarını görüntüle** ' yi seçin ve alanları aşağıdaki gibi doldurun. Bu ayarlar, eğitim işini daha iyi denetliyor. Aksi takdirde, denemeler seçimine ve verilerine göre varsayılan ayarlar uygulanır.

  
        Ek&nbsp;yapılandırma|Açıklama|Öğretici&nbsp;için&nbsp;değer
        ------|---------|---
        Birincil ölçüm| Makine öğrenimi algoritmasının ölçülecek değerlendirme ölçümü.|Normalleştirilmiş kök ortalama kare hatası
        Otomatik olarak korleştirme| Ön işleme etkinleştirilir. Bu, yapay özellikler oluşturmak için otomatik veri temizleme, hazırlama ve dönüştürmeyi içerir.| Etkinleştirme
        En iyi modeli açıkla (Önizleme)| Otomatik ML tarafından oluşturulan en iyi modelde explainability 'yi otomatik olarak gösterir.| Etkinleştirme
        Engellenen algoritmalar | Eğitim işinden dışlamak istediğiniz algoritmalar| Aşırı rastgele ağaçlar
        Ek tahmin ayarları| Bu ayarlar, modelinizin doğruluğunu artırmaya yardımcı olur <br><br> _**Tahmin ufku**_: gelecekte tahmin etmek istediğiniz sürenin uzunluğu <br> _**Tahmin hedefi lags:**_ hedef değişkenin lags 'ı ne kadar geri oluşturmak istiyorsunuz <br> _**Hedef sıralı pencere**_: *maksimum, en düşük* ve *Toplam*gibi özelliklerin oluşturulacağı pencere sayısını belirtir. |Tahmin ufuk: 14 <br> Tahmin&nbsp;hedefi&nbsp;lags: None <br> Hedef&nbsp;hareketli&nbsp;pencere&nbsp;boyutu: yok
        Çıkış ölçütü| Bir kriterle karşılanırsa eğitim işi durdurulur. |Eğitim&nbsp;işi&nbsp;süresi (saat): 3 <br> Ölçüm&nbsp;puan&nbsp;eşiği: yok
        Doğrulama | Çapraz doğrulama türü ve test sayısı seçin.|Doğrulama türü:<br>&nbsp;&nbsp;çapraz doğrulamayı yana kesme <br> <br> Doğrulama sayısı: 5
        Eşzamanlılık| Yineleme başına yürütülen en fazla paralel yineleme sayısı| En&nbsp;fazla&nbsp;eşzamanlı yineleme: 6
        
        **Kaydet**’i seçin.

## <a name="run-experiment"></a>Deneme Çalıştır

Denemenizi çalıştırmak için **son**' u seçin. Çalıştırma **ayrıntıları** ekranı, çalışma numarasının yanında bulunan **çalıştırma durumuyla** birlikte açılır. Deneme ilerledikçe bu durum güncellenir.

>[!IMPORTANT]
> Hazırlık, deneme çalıştırmasının hazırlanmasına **10-15 dakika** sürer.
> Çalışmaya başladıktan sonra, **her yinelemede 2-3 dakika daha**sürer.  <br> <br>
> Üretimde, bu işlem zaman alacağından biraz daha fazla ilerleyelim. Beklerken, tüm **modeller** sekmesinde, sınanan algoritmaları keşfetmeye başlayacağız. 

##  <a name="explore-models"></a>Modelleri keşfet

Test edilen algoritmaları (modeller) görmek için **modeller** sekmesine gidin. Modeller, varsayılan olarak, tamamlandığı gibi ölçüm puanına göre sıralanır. Bu öğreticide, seçilen **normalleştirilmiş kök anlamına** gelen en yüksek değeri gösteren model, listenin en üstünde yer alır.

Deneme modellerinin tümünün bitmesini beklerken, performans ayrıntılarını araştırmak için tamamlanmış bir modelin **algoritma adını** seçin. 

Aşağıdaki örnek, seçili modelin özelliklerini, ölçümlerini ve performans grafiklerini görüntülemek için **model ayrıntıları** ve **görselleştirmeler** sekmelerinde gezinir. 

![Ayrıntıları Çalıştır](./media/tutorial-automated-ml-forecast/explore-models-ui.gif)

## <a name="deploy-the-model"></a>Modeli dağıtma

Azure Machine Learning Studio 'da otomatik makine öğrenimi, birkaç adımda Web hizmeti olarak en iyi modeli dağıtmanıza olanak tanır. Dağıtım, yeni verileri tahmin etmek ve potansiyel fırsat bölgelerini belirlemek için modelin Tümleştirmesidir. 

Bu deneme için, bir Web hizmetine dağıtım, Bisiklet payı şirketinin tahmin Bisiklet payını sağlamak için artık yinelemeli ve ölçeklenebilir bir Web çözümüne sahip olduğu anlamına gelir. 

Çalıştırma tamamlandıktan sonra, **ayrıntıları Çalıştır** sayfasına dönün ve **modeller** sekmesini seçin.

Bu deneme bağlamında, **Stackensediskte** **normalleştirilmiş kök ortalama kare hata** ölçüsüne göre en iyi model kabul edilir.  Bu modeli dağıyoruz ancak yapmanız önerilir, dağıtımın tamamlaması yaklaşık 20 dakika sürer. Dağıtım işlemi, modeli kaydetme, kaynakları oluşturma ve bunları Web hizmeti için yapılandırma dahil olmak üzere birkaç adım gerektirir.

1. Sol alt köşedeki **en iyi modeli dağıt** düğmesini seçin.

1. **Model dağıt** bölmesini aşağıdaki gibi doldurun:

    Alan| Değer
    ----|----
    Dağıtım adı| bıkespaylaş-dağıt
    Dağıtım açıklaması| Bisiklet payı talep dağıtımı
    İşlem türü | Azure Işlem örneği (acı) seçin
    Kimlik doğrulamasını etkinleştir| Dıı. 
    Özel dağıtım varlıklarını kullanma| Dıı. Devre dışı bırakmak varsayılan sürücü dosyası (Puanlama betiği) ve ortam dosyasının yeniden oluşturulmasına izin verir. 
    
    Bu örnekte, *Gelişmiş* menüsünde belirtilen Varsayılanları kullanırız. 

1. **Dağıt**'ı seçin.  

    **Çalıştırma** ekranının üst kısmında, dağıtımın başarıyla başlatıldığını belirten yeşil başarı iletisi görüntülenir. Dağıtımın ilerlemesi bulunabilir  
    **dağıtım durumu**altında **Önerilen model** bölmesinde.
    
Dağıtım başarılı olduktan sonra, tahminleri oluşturmak için işlemsel bir Web hizmetiniz vardır. 

Yeni Web hizmetinizi kullanma hakkında daha fazla bilgi edinmek için [**sonraki adımlara**](#next-steps) ilerleyin ve Power BI yerleşik Azure Machine Learning desteğini kullanarak tahminlerinizi test edin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Dağıtım dosyaları veri ve deneme dosyalarından daha büyüktür, bu nedenle daha fazla depolama alanı maliyetlidir. Hesap maliyetlerini en aza indirmek için yalnızca dağıtım dosyalarını silin veya çalışma alanınızı ve deneme dosyalarını korumak istiyorsanız. Aksi takdirde, herhangi bir dosyayı kullanmayı planlamıyorsanız tüm kaynak grubunu silin.  

### <a name="delete-the-deployment-instance"></a>Dağıtım örneğini silme

Diğer öğreticiler ve araştırmayla ilgili kaynak grubunu ve çalışma alanını tutmak istiyorsanız, yalnızca Azure Machine Learning Studio 'dan dağıtım örneğini silin. 

1. [Azure Machine Learning Studio](https://ml.azure.com/)'ya gidin. Çalışma alanınıza gidin ve **varlıklar** bölmesinin sol tarafında **uç noktalar**' ı seçin. 

1. Silmek istediğiniz dağıtımı seçin ve **Sil**' i seçin. 

1. **Devam**' ı seçin.

### <a name="delete-the-resource-group"></a>Kaynak grubunu silme

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Bisiklet payını tahmin eden bir zaman serisi tahmin modeli oluşturup dağıtmak için Azure Machine Learning Studio 'da otomatik ML kullandınız. 

Yeni dağıtılan Web hizmetinizin tüketimini kolaylaştırmak için Power BI desteklenen bir şema oluşturma adımları için bu makaleye bakın.

> [!div class="nextstepaction"]
> [Bir web hizmetini kullanma](how-to-consume-web-service.md#consume-the-service-from-power-bi)


>[!NOTE]
> Bu bisiklet payı veri kümesi, bu öğretici için değiştirilmiştir. Bu veri kümesi, bir [kale yarışmanın](https://www.kaggle.com/c/bike-sharing-demand/data) bir parçası olarak sunulmuştur ve başlangıçta [büyük bikespaylaşımı](https://www.capitalbikeshare.com/system-data)aracılığıyla sunuldu. Ayrıca, [uMachine Learning veritabanı](http://archive.ics.uci.edu/ml/datasets/Bike+Sharing+Dataset)'nda da bulunabilir.<br><br>
> Kaynak: Fanaee-T, hadi ve gama, Joao, olay etiketleme birleştirme algılayıcıları ve arka plan bilgilerini birleştiren yapay zeka (2013): PP. 1-15, Sprümlberg.
