---
title: Üretim modelleriniz hakkında veri toplama
titleSuffix: Azure Machine Learning
description: Azure Makine Öğrenimi giriş modeli verilerini Azure Blob depolama alanında nasıl topladığınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: laobri
ms.author: copeters
author: lostmygithubaccount
ms.date: 11/12/2019
ms.custom: seodec18
ms.openlocfilehash: 44acc81df9eb6dc6a6af28b5b0f4730aa93adffc
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80475436"
---
# <a name="collect-data-for-models-in-production"></a>Üretimdeki modeller için veri toplama

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

>[!IMPORTANT]
> Azure Machine Learning Monitoring SDK yakında kullanımdan kaldırılacak. SDK, şu anda modellerdeki veri sürüklenmelerini izlemek için SDK'yı kullanan geliştiriciler için hala uygundur. Ancak yeni müşteriler için, [Basitleştirilmiş veri izlemeyi Application Insights ile](https://docs.microsoft.com/azure/machine-learning/how-to-enable-app-insights)kullanmanızı öneririz.

Bu makalede, Azure Machine Learning'den giriş modeli verilerinin nasıl toplandığı gösterilmektedir. Ayrıca, giriş verilerinin bir Azure Kubernetes Hizmeti (AKS) kümesine nasıl dağıtılanın ve çıktı verilerini Azure Blob depolama alanında nasıl depolayacak gerektiğini de gösterir.

Toplama etkinleştirildikten sonra, topladığınız veriler size yardımcı olur:

* Üretim verileri modelinize girerken [veri sürüklenmelerini izleyin.](how-to-monitor-data-drift.md)

* Modelinizi ne zaman yeniden eğitecek veya optimize edeceğiz konusunda daha iyi kararlar ver.

* Toplanan verilerle modelinizi yeniden eğitin.

## <a name="what-is-collected-and-where-it-goes"></a>Ne toplanır ve nereye gider

Aşağıdaki veriler toplanabilir:

* AKS kümesinde dağıtılan web hizmetlerinden gelen giriş verilerini modelle. Ses, görüntü ve video *toplanmaz.*
  
* Üretim girdi verilerini kullanarak model tahminleri.

>[!NOTE]
> Bu veriler üzerinde öntoplama ve ön hesaplamalar şu anda toplama hizmetinin bir parçası değildir.

Çıktı Blob depolama kaydedilir. Veriler Blob depolama alanına eklenmiştir, çözümlemeyi çalıştırmak için en sevdiğiniz aracı seçebilirsiniz.

Blob'daki çıktı verilerine giden yol aşağıdaki gibidir:

```
/modeldata/<subscriptionid>/<resourcegroup>/<workspace>/<webservice>/<model>/<version>/<designation>/<year>/<month>/<day>/data.csv
# example: /modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/12/31/data.csv
```

>[!NOTE]
> Python için Azure Machine Learning SDK'nın 0.1.0a16 `designation` sürümünden `identifier`önceki sürümlerinde bağımsız değişkenin adı . Kodunuzu önceki bir sürümle geliştirdiyseniz, buna göre güncelleştirmeniz gerekir.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://aka.ms/AMLFree) bir hesap oluşturun.

- AzureMachine Learning çalışma alanı, komut dosyalarınızı içeren yerel bir dizini ve Python için Azure Machine Learning SDK yüklü olmalıdır. Bunları nasıl yükleyeceklerini öğrenmek için geliştirme [ortamını nasıl yapılandırılatır'a](how-to-configure-environment.md)bakın.

- AKS'ye dağıtılmak üzere eğitimli bir makine öğrenme modeline ihtiyacınız var. Bir modeliniz yoksa, [Train resim sınıflandırma modeli](tutorial-train-models-with-aml.md) öğreticisine bakın.

- Bir AKS kümesine ihtiyacın var. Nasıl oluşturulacağı ve nasıl dağıtılanın acağı hakkında bilgi için nasıl [ve nerede dağıtılanın.](how-to-deploy-and-where.md)

- [Ortamınızı ayarlayın](how-to-configure-environment.md) ve [Azure Machine Learning Monitoring SDK'yı](https://aka.ms/aml-monitoring-sdk)yükleyin.

## <a name="enable-data-collection"></a>Veri toplamayı etkinleştirme

Azure Machine Learning veya diğer araçlar aracılığıyla dağıttığınız modelden bağımsız olarak veri toplamayı etkinleştirebilirsiniz.

Veri toplamayı etkinleştirmek için şunları yapmanız gerekir:

1. Puanlama dosyasını açın.

1. Dosyanın üst kısmında [aşağıdaki kodu](https://aka.ms/aml-monitoring-sdk) ekleyin:

   ```python 
   from azureml.monitoring import ModelDataCollector
   ```

1. İşlevinizde veri toplama `init` değişkenlerinizi bildirin:

    ```python
    global inputs_dc, prediction_dc
    inputs_dc = ModelDataCollector("best_model", designation="inputs", feature_names=["feat1", "feat2", "feat3". "feat4", "feat5", "feat6"])
    prediction_dc = ModelDataCollector("best_model", designation="predictions", feature_names=["prediction1", "prediction2"])
    ```

    *CorrelationId* isteğe bağlı bir parametredir. Modeliniz gerekli değilse kullanmanız gerekmez. *CorrelationId'nin* kullanımı, *LoanNumber* veya *CustomerId*gibi diğer verilerle daha kolay eşlenizi eşlemenize yardımcı olur.
    
    *Tanımlayıcı* parametresi daha sonra blob klasör yapısını oluşturmak için kullanılır. Ham verileri işlenmiş verilerden ayırt etmek için kullanabilirsiniz.

1. `run(input_df)` İşlev için aşağıdaki kod satırlarını ekleyin:

    ```python
    data = np.array(data)
    result = model.predict(data)
    inputs_dc.collect(data) #this call is saving our input data into Azure Blob
    prediction_dc.collect(result) #this call is saving our input data into Azure Blob
    ```

1. AKS'de bir hizmet dağıttığınızda veri toplama otomatik olarak **doğru** *ayarlanmaz.* Aşağıdaki örnekte olduğu gibi yapılandırma dosyanızı güncelleştirin:

    ```python
    aks_config = AksWebservice.deploy_configuration(collect_model_data=True)
    ```

    Ayrıca, bu yapılandırmayı değiştirerek hizmet izleme için Uygulama Öngörüleri'ni etkinleştirebilirsiniz:

    ```python
    aks_config = AksWebservice.deploy_configuration(collect_model_data=True, enable_app_insights=True)
    ```

1. Yeni bir görüntü oluşturmak ve makine öğrenimi modelini dağıtmak [için nasıl ve nerede dağıtılanın.](how-to-deploy-and-where.md)

Ortam dosyanızda ve puanlama dosyanızda yüklü bağımlılıklar içeren bir hizmetiniz varsa, aşağıdaki adımları izleyerek veri toplamayı etkinleştirin:

1. Azure [Machine Learning'e](https://ml.azure.com)gidin.

1. Çalışma alanınızı açın.

1. **Dağıtımları** > Seçin hizmet > **editi****seçin.**

   ![Hizmeti edin](././media/how-to-enable-data-collection/EditService.PNG)

1. **Gelişmiş**Ayarlar'da, **Uygulama Öngörülerini Etkinleştir tanılama ve veri toplama'yı**seçin.

1. Değişiklikleri uygulamak için **Güncelleştir'i** seçin.

## <a name="disable-data-collection"></a>Veri toplamayı devre dışı bırakma

İstediğiniz zaman veri toplamayı durdurabilirsiniz. Veri toplamayı devre dışı kakmak için Python kodunu veya Azure Machine Learning'i kullanın.

### <a name="option-1---disable-data-collection-in-azure-machine-learning"></a>Seçenek 1 - Azure Machine Learning'de veri toplamayı devre dışı

1. [Azure Machine Learning'de](https://ml.azure.com)oturum açın.

1. Çalışma alanınızı açın.

1. **Dağıtımları** > Seçin hizmet > **editi****seçin.**

   [![Edit seçeneğini seçin](././media/how-to-enable-data-collection/EditService.PNG)](./././media/how-to-enable-data-collection/EditService.PNG#lightbox)

1. **Gelişmiş**Ayarlar'da, **Uygulama Öngörülerini Etkinleştir tanılama ve veri toplamayı**netleyin.

1. Değişikliği uygulamak için **Güncelleştir'i** seçin.

Bu ayarlara [Azure Machine Learning'de](https://ml.azure.com)de iş alanınızda erişebilirsiniz.

### <a name="option-2---use-python-to-disable-data-collection"></a>Seçenek 2 - Veri toplamayı devre dışı kalmak için Python'u kullanma

  ```python 
  ## replace <service_name> with the name of the web service
  <service_name>.update(collect_model_data=False)
  ```

## <a name="validate-and-analyze-your-data"></a>Verilerinizi doğrulama ve analiz

Blob depolamanızda toplanan verileri çözümlemek için tercih ettiğiniz bir araç seçebilirsiniz.

### <a name="quickly-access-your-blob-data"></a>Blob verilerinize hızla erişin

1. [Azure Machine Learning'de](https://ml.azure.com)oturum açın.

1. Çalışma alanınızı açın.

1. **Depolama**’yı seçin.

    [![Depolama seçeneğini seçin](./media/how-to-enable-data-collection/StorageLocation.png)](././media/how-to-enable-data-collection/StorageLocation.png#lightbox)

1. Bu sözdizimi ile blob çıktı verilerine giden yolu izleyin:

   ```
   /modeldata/<subscriptionid>/<resourcegroup>/<workspace>/<webservice>/<model>/<version>/<designation>/<year>/<month>/<day>/data.csv
   # example: /modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/12/31/data.csv
   ```

### <a name="analyze-model-data-using-power-bi"></a>Power BI kullanarak model verilerini analiz edin

1. İndirin ve [Power BI Masaüstü](https://www.powerbi.com)açın.

1. **Veri Al'ı** ve [**Azure Blob Depolama'yı**](https://docs.microsoft.com/power-bi/desktop-data-sources)seçin.

    [![Güç BI blob kurulumu](./media/how-to-enable-data-collection/PBIBlob.png)](././media/how-to-enable-data-collection/PBIBlob.png#lightbox)

1. Depolama hesabı adınızı ekleyin ve depolama anahtarınızı girin. Bu bilgileri blob'unuzda **Ayarlar** > **Erişim tuşlarını** seçerek bulabilirsiniz.

1. Model **veri** kapsayıcısını seçin ve **Edit'i**seçin.

    [![Güç BI Navigator](./media/how-to-enable-data-collection/pbiNavigator.png)](././media/how-to-enable-data-collection/pbiNavigator.png#lightbox)

1. Sorgu düzenleyicisinde, **Ad** sütununa tıklayın ve depolama hesabınızı ekleyin.

1. Model yolunuzu filtreye girin. Yalnızca belirli bir yıl veya aydaki dosyalara bakmak istiyorsanız, filtre yolunu genişletmeniz gereken bir uygulamadır. Örneğin, yalnızca Mart verilerine bakmak için şu filtre yolunu kullanın:

   /modeldata/\<subscriptionid\<>/\<resourcegroupname>/\<workspacename>/ webservicename>/\<modelname>/\<modelversion>/\<atama>/\<yıl>/3

1. **Ad** değerlerine göre sizinle alakalı verileri filtreleyin. Öngörüleri ve girişleri depolasanız, her biri için bir sorgu oluşturmanız gerekir.

1. Dosyaları birleştirmek için **İçerik** sütunbaşlığının yanındaki aşağı doğru çift okları seçin.

    [![Güç BI İçerik](./media/how-to-enable-data-collection/pbiContent.png)](././media/how-to-enable-data-collection/pbiContent.png#lightbox)

1. **Tamam'ı**seçin. Veri önceden yüklenir.

    [![Güç BI Birleştirme Dosyaları](./media/how-to-enable-data-collection/pbiCombine.png)](././media/how-to-enable-data-collection/pbiCombine.png#lightbox)

1. **Kapat ve Uygula'yı**seçin.

1. Girişler ve öngörüler eklediyseniz, tablolarınız **RequestId** değerleri tarafından otomatik olarak sıralanır.

1. Model verileriniz üzerinde özel raporlarınızı oluşturmaya başlayın.

### <a name="analyze-model-data-using-azure-databricks"></a>Azure Databricks'i kullanarak model verilerini analiz edin

1. Azure [Databricks çalışma alanı](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal)oluşturun.

1. Databricks çalışma alanınıza gidin.

1. Databricks çalışma alanınızda **Verileri Yükle'yi**seçin.

    [![Databricks Yükleme Veri seçeneğini seçme](./media/how-to-enable-data-collection/dbupload.png)](././media/how-to-enable-data-collection/dbupload.png#lightbox)

1. **Yeni Tablo Oluştur'u** seçin ve Not Defteri'nde Diğer **Veri Kaynakları** > **Azure Blob Depolama** > **Oluşturma Tablosu'nu**seçin.

    [![Databricks tablo oluşturma](./media/how-to-enable-data-collection/dbtable.PNG)](././media/how-to-enable-data-collection/dbtable.PNG#lightbox)

1. Verilerinizin konumunu güncelleştirin. Örnek aşağıda verilmiştir:

    ```
    file_location = "wasbs://mycontainer@storageaccountname.blob.core.windows.net/modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/*/*/data.csv" 
    file_type = "csv"
    ```

    [![Databricks kurulumu](./media/how-to-enable-data-collection/dbsetup.png)](././media/how-to-enable-data-collection/dbsetup.png#lightbox)

1. Verilerinizi görüntülemek ve analiz etmek için şablondaki adımları izleyin.
