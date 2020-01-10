---
title: Üretim modellerinizde veri toplama
titleSuffix: Azure Machine Learning
description: Azure Blob depolamada Azure Machine Learning giriş modeli verilerini nasıl toplayacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: laobri
ms.author: copeters
author: lostmygithubaccount
ms.date: 11/12/2019
ms.custom: seodec18
ms.openlocfilehash: 3c481a2e12d83e865025cd90e59e0eba572ad9a5
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75771402"
---
# <a name="collect-data-for-models-in-production"></a>Üretimde modelleri için veri toplama

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

>[!IMPORTANT]
> Azure Machine Learning Izleme SDK 'Sı yakında kullanımdan kaldırılacak. SDK halen, SDK 'leri modellerdeki veri kayması izlemek için kullanan geliştiriciler için uygun olmaya devam etmektedir. Ancak yeni müşteriler için [Application Insights ile basitleştirilmiş veri izlemeyi](https://docs.microsoft.com/azure/machine-learning/how-to-enable-app-insights)kullanmanızı öneririz.

Bu makalede, Azure Machine Learning giriş modeli verilerinin nasıl toplanacağı gösterilmektedir. Ayrıca, giriş verilerinin bir Azure Kubernetes hizmeti (AKS) kümesine nasıl dağıtılacağı ve çıkış verilerinin Azure Blob depolama alanında nasıl depolandığı gösterilmektedir.

Koleksiyon etkinleştirildikten sonra, topladığınız veriler size yardımcı olur:

* Üretim verileri modelinize girdiğinde [veri Drifts izleyin](how-to-monitor-data-drift.md) .

* Modelinizin ne zaman yeniden eğitileceği veya iyileştirilmesine yönelik daha iyi kararlar alın.

* Toplanan verilerle modelinize yeniden eğitme.

## <a name="what-is-collected-and-where-it-goes"></a>Ne toplanır ve nerede gidiyor

Aşağıdaki veriler toplanabilir:

* AKS kümesinde dağıtılan Web hizmetlerinden model girişi verileri. Ses ses, görüntü ve *video toplanmaz.*
  
* Üretim giriş verileri kullanarak model tahmin.

>[!NOTE]
> Bu verilerdeki ön kimlik doğrulama ve önceden hesaplamaları şu anda koleksiyon hizmeti 'nin bir parçası değil.

Çıktı, BLOB depolama alanına kaydedilir. Veriler blob depolamaya eklendiğinden, Analizi çalıştırmak için en sevdiğiniz aracı seçebilirsiniz.

Çıktı verilerini BLOB yolunu bu söz dizimi aşağıdaki gibidir:

```
/modeldata/<subscriptionid>/<resourcegroup>/<workspace>/<webservice>/<model>/<version>/<designation>/<year>/<month>/<day>/data.csv
# example: /modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/12/31/data.csv
```

>[!NOTE]
> Python için Azure Machine Learning SDK 'nın 0.1.0 A16 sürümünden önceki sürümlerinde, `designation` bağımsız değişkeni `identifier`olarak adlandırılır. Kodunuzu daha önceki bir sürümle geliştirdiyseniz, uygun şekilde güncelleştirmeniz gerekir.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://aka.ms/AMLFree) oluşturun.

- Bir AzureMachine Learning çalışma alanı, betiklerinizi içeren bir yerel dizin ve Python için Azure Machine Learning SDK yüklü olmalıdır. Bunları nasıl yükleyeceğiniz hakkında bilgi edinmek için bkz. [geliştirme ortamı yapılandırma](how-to-configure-environment.md).

- AKS 'e dağıtılacak eğitilen makine öğrenimi modelinin olması gerekir. Modeliniz yoksa, [görüntü sınıflandırma modeline eğit](tutorial-train-models-with-aml.md) öğreticiye bakın.

- Bir AKS kümeniz olması gerekir. Oluşturma ve dağıtma hakkında daha fazla bilgi için bkz. [nasıl dağıtılır ve nerede](how-to-deploy-and-where.md).

- [Ortamınızı ayarlayın](how-to-configure-environment.md) ve [Azure Machine Learning izleme SDK 'sını](https://aka.ms/aml-monitoring-sdk)kurun.

## <a name="enable-data-collection"></a>Veri toplamayı etkinleştirme

Azure Machine Learning veya diğer araçlarla dağıttığınız modelden bağımsız olarak veri toplamayı etkinleştirebilirsiniz.

Veri toplamayı etkinleştirmek için şunları yapmanız gerekir:

1. Puanlama dosyası açın.

1. Ekleme [koddan](https://aka.ms/aml-monitoring-sdk) dosyanın üst:

   ```python 
   from azureml.monitoring import ModelDataCollector
   ```

1. Veri koleksiyonu değişkenlerinizi bildirin, `init` işlevi:

    ```python
    global inputs_dc, prediction_dc
    inputs_dc = ModelDataCollector("best_model", designation="inputs", feature_names=["feat1", "feat2", "feat3". "feat4", "feat5", "feat6"])
    prediction_dc = ModelDataCollector("best_model", designation="predictions", feature_names=["prediction1", "prediction2"])
    ```

    *CorrelationId* isteğe bağlı bir parametredir. Modeliniz gerekmiyorsa kullanmanız gerekmez. *CorrelationId* kullanımı, *Krete numarası* veya *CustomerID*gibi diğer verilerle daha kolay bir şekilde eşleşmenizin sağlanmasına yardımcı olur.
    
    *Tanımlayıcı* parametresi daha sonra Blobun klasör yapısını oluşturmak için kullanılır. Ham verileri işlenen verilerden ayırt etmek için kullanabilirsiniz.

1. Aşağıdaki kod satırlarını ekleme `run(input_df)` işlevi:

    ```python
    data = np.array(data)
    result = model.predict(data)
    inputs_dc.collect(data) #this call is saving our input data into Azure Blob
    prediction_dc.collect(result) #this call is saving our input data into Azure Blob
    ```

1. AKS 'de bir hizmet dağıttığınızda veri *toplama otomatik olarak* **doğru** olarak ayarlanamaz. Yapılandırma dosyanızı aşağıdaki örnekte olduğu gibi güncelleştirin:

    ```python
    aks_config = AksWebservice.deploy_configuration(collect_model_data=True)
    ```

    Ayrıca, bu yapılandırmayı değiştirerek hizmet izleme için Application Insights etkinleştirebilirsiniz:

    ```python
    aks_config = AksWebservice.deploy_configuration(collect_model_data=True, enable_app_insights=True)
    ```

1. Yeni bir görüntü oluşturmak ve Machine Learning modelini dağıtmak için bkz. [dağıtım ve nerede](how-to-deploy-and-where.md).

Ortam dosyanıza ve Puanlama dosyanıza yüklenmiş bağımlılıklara sahip bir hizmetiniz zaten varsa, aşağıdaki adımları izleyerek veri toplamayı etkinleştirin:

1. [Azure Machine Learning](https://ml.azure.com)gidin.

1. Çalışma alanını açın.

1. **Dağıtım** > **hizmet** > **Düzenle**' yi seçin.

   ![Hizmeti düzenleme](././media/how-to-enable-data-collection/EditService.PNG)

1. **Gelişmiş ayarlar**' da **model veri toplamayı etkinleştir**' i seçin.

    [veri toplamayı ![seçin](./media/how-to-enable-data-collection/CheckDataCollection.png)](././media/how-to-enable-data-collection/CheckDataCollection.png#lightbox)

   Hizmetinizin sistem durumunu izlemek için **Appınsights tanılamayı etkinleştir** ' i de seçebilirsiniz.

1. Değişiklikleri uygulamak için **Güncelleştir** ' i seçin.

## <a name="disable-data-collection"></a>Veri toplamayı devre dışı

İstediğiniz zaman veri toplamayı durdurabilirsiniz. Veri toplamayı devre dışı bırakmak için Python kodu veya Azure Machine Learning kullanın.

### <a name="option-1---disable-data-collection-in-azure-machine-learning"></a>Seçenek 1-Azure Machine Learning veri toplamayı devre dışı bırak

1. [Azure Machine Learning](https://ml.azure.com)oturum açın.

1. Çalışma alanını açın.

1. **Dağıtım** > **hizmet** > **Düzenle**' yi seçin.

   [![Düzenle seçeneğini belirleyin](././media/how-to-enable-data-collection/EditService.PNG)](./././media/how-to-enable-data-collection/EditService.PNG#lightbox)

1. **Gelişmiş ayarlar**' da **model veri toplamayı etkinleştir**' i temizleyin.

    [veri toplama onay kutusunu ![temizleyin](./media/how-to-enable-data-collection/UncheckDataCollection.png)](././media/how-to-enable-data-collection/UncheckDataCollection.png#lightbox)

1. Seçin **güncelleştirme** değişikliği uygulamak için.

Ayrıca, bu ayarlara [Azure Machine Learning](https://ml.azure.com)çalışma alanınızda erişebilirsiniz.

### <a name="option-2---use-python-to-disable-data-collection"></a>Seçenek 2-veri toplamayı devre dışı bırakmak için Python kullanma

  ```python 
  ## replace <service_name> with the name of the web service
  <service_name>.update(collect_model_data=False)
  ```

## <a name="validate-and-analyze-your-data"></a>Verilerinizi doğrulayın ve çözümleyin

Blob depolamadaki toplanan verileri analiz etmek için tercih ettiğiniz bir aracı seçebilirsiniz.

### <a name="quickly-access-your-blob-data"></a>Blob verilerinize hızlıca erişin

1. [Azure Machine Learning](https://ml.azure.com)oturum açın.

1. Çalışma alanını açın.

1. **Depolama**’yı seçin.

    [![depolama seçeneğini belirleyin](./media/how-to-enable-data-collection/StorageLocation.png)](././media/how-to-enable-data-collection/StorageLocation.png#lightbox)

1. Bu söz dizimi ile Blobun çıkış verilerinin yolunu izleyin:

   ```
   /modeldata/<subscriptionid>/<resourcegroup>/<workspace>/<webservice>/<model>/<version>/<designation>/<year>/<month>/<day>/data.csv
   # example: /modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/12/31/data.csv
   ```

### <a name="analyze-model-data-using-power-bi"></a>Power BI kullanarak model verilerini analiz etme

1. [Power BI Desktop](https://www.powerbi.com)indirip açın.

1. **Veri al** ' ı seçin ve [**Azure Blob depolama**](https://docs.microsoft.com/power-bi/desktop-data-sources)' yı seçin.

    [![Power BI blobu kurulumu](./media/how-to-enable-data-collection/PBIBlob.png)](././media/how-to-enable-data-collection/PBIBlob.png#lightbox)

1. Depolama hesabınızın adını ekleyin ve depolama anahtarınızı girin. Bu bilgileri, bloba > **erişim anahtarlarına** **Ayarlar** ' i seçerek bulabilirsiniz.

1. **Model veri** kapsayıcısını seçin ve **Düzenle**' yi seçin.

    [Power BI Gezgini ![](./media/how-to-enable-data-collection/pbiNavigator.png)](././media/how-to-enable-data-collection/pbiNavigator.png#lightbox)

1. Sorgu Düzenleyicisi 'nde **ad** sütununun altına tıklayın ve depolama hesabınızı ekleyin.

1. Model yolunuzu filtreye girin. Yalnızca belirli bir yıla veya aya ait dosyalara bakmak isterseniz, filtre yolunu genişletmeniz yeterlidir. Örneğin, yalnızca Mart verilerine bakmak için şu filtre yolunu kullanın:

   /modeldata/\<SubscriptionID >/\<resourcegroupname >/\<çalışmaadı >/\<WebServiceName >/\<ModelName >/\<modelversion >/\<gösterimi >/\<Year >/3

1. **Ad** değerlerine göre sizin için uygun olan verileri filtreleyin. Tahminleri ve girişleri depoladıysanız, her biri için bir sorgu oluşturmanız gerekir.

1. Dosyaları birleştirmek için **içerik** sütun başlığının yanındaki aşağı çift okları seçin.

    [Power BI Içerik ![](./media/how-to-enable-data-collection/pbiContent.png)](././media/how-to-enable-data-collection/pbiContent.png#lightbox)

1. **Tamam**’ı seçin. Veriler önceden yüklenir.

    [Dosyaları Power BI ![birleştirme](./media/how-to-enable-data-collection/pbiCombine.png)](././media/how-to-enable-data-collection/pbiCombine.png#lightbox)

1. **Kapat ve Uygula '** yı seçin.

1. Giriş ve tahmin eklediyseniz, tablolarınız otomatik olarak **RequestId** değerlerine göre sıralanır.

1. Model verileriniz üzerinde özel raporlar oluşturmaya başlayın.

### <a name="analyze-model-data-using-azure-databricks"></a>Azure Databricks kullanarak model verilerini analiz etme

1. [Azure Databricks çalışma alanı](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal)oluşturun.

1. Databricks çalışma alanınıza gidin.

1. Databricks çalışma alanınızda **verileri karşıya yükle**' yi seçin.

    [Databricks veri yükleme seçeneğini belirleme ![](./media/how-to-enable-data-collection/dbupload.png)](././media/how-to-enable-data-collection/dbupload.png#lightbox)

1. **Yeni tablo oluştur** **' u**seçin ve > **Azure Blob depolama** > **diğer veri kaynaklarını** seçin.

    [Databricks tablosu oluşturma ![](./media/how-to-enable-data-collection/dbtable.PNG)](././media/how-to-enable-data-collection/dbtable.PNG#lightbox)

1. Verilerinizin konumunu güncelleştirin. Örnek aşağıda verilmiştir:

    ```
    file_location = "wasbs://mycontainer@storageaccountname.blob.core.windows.net/modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/*/*/data.csv" 
    file_type = "csv"
    ```

    [Databricks kurulumu ![](./media/how-to-enable-data-collection/dbsetup.png)](././media/how-to-enable-data-collection/dbsetup.png#lightbox)

1. Verilerinizi görüntülemek ve analiz etmek için şablondaki adımları izleyin.
