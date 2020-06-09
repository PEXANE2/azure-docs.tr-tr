---
title: Machine Learning Web hizmeti uç noktalarından verileri izleme ve toplama
titleSuffix: Azure Machine Learning
description: Azure Application Insights kullanarak Azure Machine Learning ile dağıtılan Web hizmetlerini izleme
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.reviewer: jmartens
ms.author: larryfr
author: blackmist
ms.date: 03/12/2020
ms.custom: tracking-python
ms.openlocfilehash: 2473d864e0ad0fca4a886a6135a9caac0742e3d7
ms.sourcegitcommit: 964af22b530263bb17fff94fd859321d37745d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84557083"
---
# <a name="monitor-and-collect-data-from-ml-web-service-endpoints"></a>ML Web hizmeti uç noktalarından verileri izleme ve toplama
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, kullanarak Azure Kubernetes Service (AKS) veya Azure Container Instances (acı Application Insights) içindeki Web hizmeti uç noktalarına dağıtılan modellerden nasıl veri toplayacağınızı öğrenirsiniz. 
* [Python SDK Azure Machine Learning](#python)
* [Azure Machine Learning Studio](#studio)https://ml.azure.com

Uç noktanın çıkış verilerini ve yanıtını toplamaya ek olarak şunları izleyebilirsiniz:

* İstek ücretleri, yanıt süreleri ve hata oranları
* Bağımlılık oranları, yanıt süreleri ve hata oranları
* Özel durumlar

[Azure Application Insights hakkında daha fazla bilgi edinin](../azure-monitor/app/app-insights-overview.md). 


## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin

* Azure Machine Learning çalışma alanı, betiklerinizi içeren yerel bir dizin ve Python için Azure Machine Learning SDK yüklü. Bu önkoşulları nasıl alabileceğinizi öğrenmek için bkz. [geliştirme ortamını yapılandırma](how-to-configure-environment.md)

* Azure Kubernetes hizmeti (AKS) veya Azure Container Instance 'a (acı) dağıtılacak eğitilen bir makine öğrenme modeli. Bir tane yoksa, bkz. [eğitim resmi sınıflandırma modeli](tutorial-train-models-with-aml.md) öğreticisi

## <a name="web-service-metadata-and-response-data"></a>Web hizmeti meta verileri ve yanıt verileri

>[!Important]
> Azure Application Insights, yalnızca 64 KB 'a kadar olan yükleri günlüğe kaydeder. Bu sınıra ulaşıldığında, yalnızca modelin en son çıkışları günlüğe kaydedilir. 

Web hizmeti meta verilerine ve modelin tahminlere karşılık gelen meta veriler ve hizmetin yanıtı, ileti altındaki Azure Application Insights izlemelerinde günlüğe kaydedilir `"model_data_collection"` . Bu verilere erişmek için doğrudan Azure Application Insights sorgulayabilir veya daha uzun bekletme veya daha fazla işleme için depolama hesabına [sürekli bir dışarı aktarma](https://docs.microsoft.com/azure/azure-monitor/app/export-telemetry) ayarlayabilirsiniz. Model verileri daha sonra etiketleme, yeniden eğitim, explainability, veri analizi veya diğer kullanımı ayarlamak için Azure Machine Learning kullanılabilir. 

<a name="python"></a>

## <a name="use-python-sdk-to-configure"></a>Yapılandırmak için Python SDK 'sını kullanma 

### <a name="update-a-deployed-service"></a>Dağıtılan bir hizmeti güncelleştirme

1. Çalışma alanınızdaki hizmeti belirler. Değeri, `ws` çalışma alanınızın adıdır

    ```python
    from azureml.core.webservice import Webservice
    aks_service= Webservice(ws, "my-service-name")
    ```
2. Hizmetinizi güncelleştirme ve Azure Application Insights etkinleştirme

    ```python
    aks_service.update(enable_app_insights=True)
    ```

### <a name="log-custom-traces-in-your-service"></a>Hizmetinizdeki özel izlemeleri günlüğe kaydet

Özel izlemeleri günlüğe kaydetmek istiyorsanız, AKS için standart dağıtım sürecini [ve nasıl dağıtılacağı ve nerede](how-to-deploy-and-where.md) belgede bir aci 'yi izleyin. Ardından aşağıdaki adımları kullanın:

1. Yazdırma deyimleri ekleyerek Puanlama dosyasını güncelleştirme
    
    ```python
    print ("model initialized" + time.strftime("%H:%M:%S"))
    ```

2. Hizmet yapılandırmasını güncelleştirme
    
    ```python
    config = Webservice.deploy_configuration(enable_app_insights=True)
    ```

3. Bir görüntü oluşturun ve [aks veya ACI](how-to-deploy-and-where.md)üzerinde dağıtın.

### <a name="disable-tracking-in-python"></a>Python 'da izlemeyi devre dışı bırak

Azure Application Insights 'yi devre dışı bırakmak için aşağıdaki kodu kullanın:

```python 
## replace <service_name> with the name of the web service
<service_name>.update(enable_app_insights=False)
```

<a name="studio"></a>

## <a name="use-azure-machine-learning-studio-to-configure"></a>Yapılandırmak için Azure Machine Learning Studio 'yu kullanma

Ayrıca, modelinizi bu adımlarla dağıtmaya hazırsanız Azure Machine Learning Studio 'dan Azure Application Insights 'yi etkinleştirebilirsiniz.

1. Şu adreste çalışma alanınızda oturum açın:https://ml.azure.com/
1. **Modeller** ' e gidin ve dağıtmak istediğiniz modeli seçin
1. **+ Dağıt** seçeneğini belirleyin
1. **Dağıtım modeli** formunu doldur
1. **Gelişmiş** menüyü Genişlet

    ![Formu dağıt](./media/how-to-enable-app-insights/deploy-form.png)
1. **Tanılamayı ve veri toplamayı etkinleştir Application Insights** seçin

    ![App Insights 'ı etkinleştir](./media/how-to-enable-app-insights/enable-app-insights.png)
## <a name="evaluate-data"></a>Verileri değerlendir
Hizmetinizin verileri, Azure Application Insights hesabınızda, Azure Machine Learning ile aynı kaynak grubu içinde depolanır.
Görüntülemek için:

1. [Azure portal](https://ms.portal.azure.com/) Azure Machine Learning çalışma alanınıza gidin ve Application Insights bağlantısına tıklayın

    [![Appınsi, Sloc](./media/how-to-enable-app-insights/AppInsightsLoc.png)](././media/how-to-enable-app-insights/AppInsightsLoc.png#lightbox)

1. Hizmetinizin temel ölçüm kümesini görmek için **genel bakış** sekmesini seçin

   [![Genel Bakış](./media/how-to-enable-app-insights/overview.png)](././media/how-to-enable-app-insights/overview.png#lightbox)

1. Web hizmeti istek meta verileri ve yanıtınıza bakmak için **Günlükler (Analiz)** bölümündeki **istekler** tablosunu seçin ve istekleri görüntülemek için **Çalıştır** ' ı seçin

   [![Verileri modelleme](./media/how-to-enable-app-insights/model-data-trace.png)](././media/how-to-enable-app-insights/model-data-trace.png#lightbox)


3. Özel izlemelerinizi görmek için **analiz** ' i seçin
4. Şema bölümünde **izlemeler**' ı seçin. Sonra sorgunuzu çalıştırmak için **Çalıştır** ' ı seçin. Veriler bir tablo biçiminde görünmelidir ve Puanlama dosyanızdaki özel çağrılarınız ile eşleşmelidir

   [![Özel izlemeler](./media/how-to-enable-app-insights/logs.png)](././media/how-to-enable-app-insights/logs.png#lightbox)

Azure Application Insights kullanma hakkında daha fazla bilgi edinmek için bkz. [Application Insights nedir?](../azure-monitor/app/app-insights-overview.md).

## <a name="export-data-for-further-processing-and-longer-retention"></a>Daha fazla işleme ve daha uzun bekletme için verileri dışarı aktarma

>[!Important]
> Azure Application Insights yalnızca blob depolamaya dışarı aktarmaları destekler. Bu dışa aktarma yeteneğinin ek sınırları, [uygulama öngörülerine ait Telemetriyi dışarı aktarma](https://docs.microsoft.com/azure/azure-monitor/app/export-telemetry#continuous-export-advanced-storage-configuration)bölümünde listelenmiştir.

Azure Application Insights ' [sürekli dışarı aktarma](https://docs.microsoft.com/azure/azure-monitor/app/export-telemetry) ' yı, daha uzun bir saklama 'nin ayarlandığı desteklenen bir depolama hesabına ileti göndermek için kullanabilirsiniz. `"model_data_collection"`ILETILER JSON biçiminde depolanır ve model verilerini ayıklamak için kolayca ayrıştırılabilir. 

Azure Data Factory, Azure ML işlem hatları veya diğer veri işleme araçları, verileri gerektiği şekilde dönüştürmek için kullanılabilir. Verileri dönüştürdüğünü daha sonra Azure Machine Learning çalışma alanıyla bir veri kümesi olarak kaydedebilirsiniz. Bunu yapmak için bkz. [veri kümesi oluşturma ve kaydetme](how-to-create-register-datasets.md).

   [![Sürekli dışarı aktarma](./media/how-to-enable-app-insights/continuous-export-setup.png)](././media/how-to-enable-app-insights/continuous-export-setup.png)


## <a name="example-notebook"></a>Örnek Not defteri

[Enable-App-Insights-in-Production-Service. ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/enable-app-insights-in-production-service/enable-app-insights-in-production-service.ipynb) Not defteri, bu makaledeki kavramları gösterir. 
 
[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Kubernetes hizmet kümesine model dağıtma](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-azure-kubernetes-service) veya modellerinizi Web hizmeti uç noktalarına dağıtmak için [Azure Container Instances bir modeli dağıtma](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-azure-container-instance) ve Azure Application Insights veri toplamayı ve uç nokta izlemeyi kullanmasını sağlama
* Üretimde modellerden toplanan verileri kullanma hakkında daha fazla bilgi edinmek için bkz. [Mlops: Azure Machine Learning modelleri yönetme, dağıtma ve izleme](https://docs.microsoft.com/azure/machine-learning/concept-model-management-and-deployment) . Bu tür veriler, Machine Learning işleminizi sürekli olarak iyileştirmenize yardımcı olabilir
