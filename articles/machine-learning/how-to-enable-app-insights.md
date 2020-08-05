---
title: Machine Learning Web hizmeti uç noktalarından verileri izleme ve toplama
titleSuffix: Azure Machine Learning
description: Azure Application Insights kullanarak Azure Machine Learning ile dağıtılan Web hizmetlerini izleme
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: jmartens
ms.author: larryfr
author: blackmist
ms.date: 07/23/2020
ms.topic: conceptual
ms.custom: how-to, tracking-python
ms.openlocfilehash: e12c22d56399ce1690bee678623c58288cf0163b
ms.sourcegitcommit: 1b2d1755b2bf85f97b27e8fbec2ffc2fcd345120
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87552212"
---
# <a name="monitor-and-collect-data-from-ml-web-service-endpoints"></a>ML Web hizmeti uç noktalarından verileri izleme ve toplama
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Kubernetes Service (AKS) veya Azure Container Instances (acı) içindeki Web hizmeti uç noktalarına dağıtılan modellerden nasıl veri toplayacağınızı öğrenirsiniz ve bu, günlükleri sorgulayarak ve Azure Application Insights ile 
* [Python SDK Azure Machine Learning](#python)
* [Azure Machine Learning Studio](#studio)https://ml.azure.com

Uç noktanın çıkış verilerini ve yanıtını toplamaya ek olarak şunları izleyebilirsiniz:

* İstek ücretleri, yanıt süreleri ve hata oranları
* Bağımlılık oranları, yanıt süreleri ve hata oranları
* Özel durumlar

[Azure Application Insights hakkında daha fazla bilgi edinin](../azure-monitor/app/app-insights-overview.md). 


## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin

* Azure Machine Learning çalışma alanı, betiklerinizi içeren yerel bir dizin ve Python için Azure Machine Learning SDK yüklü. Bu önkoşulları nasıl alabileceğinizi öğrenmek için bkz. [geliştirme ortamını yapılandırma](how-to-configure-environment.md)

* Azure Kubernetes hizmeti (AKS) veya Azure Container Instance 'a (acı) dağıtılacak eğitilen bir makine öğrenme modeli. Bir tane yoksa, bkz. [eğitim resmi sınıflandırma modeli](tutorial-train-models-with-aml.md) öğreticisi

## <a name="query-logs-for-deployed-models"></a>Dağıtılan modeller için sorgu günlükleri

Daha önce dağıtılan bir Web hizmetinden günlükleri almak için hizmeti yükleyin ve `get_logs()` işlevini kullanın. Günlükler, dağıtım sırasında oluşan hatalar hakkında ayrıntılı bilgiler içerebilir.

```python
from azureml.core.webservice import Webservice

# load existing web service
service = Webservice(name="service-name", workspace=ws)
logs = service.get_logs()
```

## <a name="web-service-metadata-and-response-data"></a>Web hizmeti meta verileri ve yanıt verileri

> [!IMPORTANT]
> Azure Application Insights, yalnızca 64 KB 'a kadar olan yükleri günlüğe kaydeder. Bu sınıra ulaşıldığında, yetersiz bellek gibi hatalarla karşılaşabilirsiniz veya hiçbir bilgi günlüğe kaydedilmez.

Web hizmetine bir isteğin bilgilerini günlüğe kaydetmek için, `print` Score.py dosyanıza deyimler ekleyin. Her `print` bir ifade, ileti altındaki Application Insights izleme tablosunda bir girdiye neden olur `STDOUT` . Deyimin içerikleri, `print` `customDimensions` ve ardından `Contents` izleme tablosunda yer alır. Bir JSON dizesi yazdırırsanız, altındaki izleme çıkışında hiyerarşik bir veri yapısı üretir `Contents` .

Bu verilere erişmek için doğrudan Azure Application Insights sorgulayabilir veya daha uzun bekletme veya daha fazla işleme için depolama hesabına [sürekli bir dışarı aktarma](https://docs.microsoft.com/azure/azure-monitor/app/export-telemetry) ayarlayabilirsiniz. Model verileri daha sonra etiketleme, yeniden eğitim, explainability, veri analizi veya diğer kullanımı ayarlamak için Azure Machine Learning kullanılabilir. 


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

1. Çıkarım sırasında Application Insights veri göndermek için, Print deyimlerini ekleyerek Puanlama dosyasını güncelleştirin. İstek verileri ve yanıt gibi daha karmaşık bilgileri günlüğe kaydetmek için bir JSON yapısı. Aşağıdaki örnek score.py dosyası, modelin başlatıldığı süreyi, çıkarım sırasında giriş ve çıktıyı ve hata meydana gelir:

    > [!IMPORTANT]
    > Azure Application Insights, yalnızca 64 KB 'a kadar olan yükleri günlüğe kaydeder. Bu sınıra ulaşılırsa, yetersiz bellek gibi hatalarla karşılaşabilirsiniz veya hiçbir bilgi günlüğe kaydedilmez. Günlüğe kaydetmek istediğiniz veriler 64 KB veya daha büyükse, [üretim içindeki modeller Için veri toplama](how-to-enable-data-collection.md)bölümündeki bilgileri kullanarak BLOB depolama alanına depolamanız gerekir.
    
    ```python
    import pickle
    import json
    import numpy 
    from sklearn.externals import joblib
    from sklearn.linear_model import Ridge
    from azureml.core.model import Model
    import time

    def init():
        global model
        #Print statement for appinsights custom traces:
        print ("model initialized" + time.strftime("%H:%M:%S"))
        
        # note here "sklearn_regression_model.pkl" is the name of the model registered under the workspace
        # this call should return the path to the model.pkl file on the local disk.
        model_path = Model.get_model_path(model_name = 'sklearn_regression_model.pkl')
        
        # deserialize the model file back into a sklearn model
        model = joblib.load(model_path)
    

    # note you can pass in multiple rows for scoring
    def run(raw_data):
        try:
            data = json.loads(raw_data)['data']
            data = numpy.array(data)
            result = model.predict(data)
            # Log the input and output data to appinsights:
            info = {
                "input": raw_data,
                "output": result.tolist()
                }
            print(json.dumps(info))
            # you can return any datatype as long as it is JSON-serializable
            return result.tolist()
        except Exception as e:
            error = str(e)
            print (error + time.strftime("%H:%M:%S"))
            return error
    ```

2. Hizmet yapılandırmasını güncelleştirme
    
    ```python
    config = Webservice.deploy_configuration(enable_app_insights=True)
    ```

3. Bir görüntü oluşturun ve [aks veya ACI](how-to-deploy-and-where.md)üzerinde dağıtın.

Günlüğe kaydetme ve veri toplama hakkında daha fazla bilgi için bkz. [Azure Machine Learning oturum açmayı etkinleştirme](how-to-enable-logging.md) ve [üretimdeki modellerden veri toplama](how-to-enable-data-collection.md).

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

## <a name="view-metrics-and-logs"></a>Ölçümleri ve günlükleri görüntüleme

Hizmetinizin verileri, Azure Application Insights hesabınızda, Azure Machine Learning ile aynı kaynak grubu içinde depolanır.
Görüntülemek için:

1. [Studio](https://ml.azure.com/)'daki Azure Machine Learning çalışma alanınıza gidin.
1. **Uç noktaları**seçin.
1. Dağıtılan hizmetinizi seçin.
1. **Application Insights URL 'sini** bulmak için aşağı kaydırın ve bağlantıyı seçin.

    [![Application Insights URL 'sini bul](./media/how-to-enable-app-insights/appinsightsloc.png)](././media/how-to-enable-app-insights/appinsightsloc.png#lightbox)

1. Application Insights ' de, sol taraftaki listenin **genel bakış** sekmesinde veya __izleme__ bölümünde __Günlükler__' i seçin.

    [![İzlemenin Genel Bakış sekmesi](./media/how-to-enable-app-insights/overview.png)](./media/how-to-enable-app-insights/overview.png#lightbox)

1. Score.py dosyasından günlüğe kaydedilen bilgileri görüntülemek için __izlemeler__ tablosuna bakın. Aşağıdaki sorgu, __giriş__ değerinin günlüğe kaydedildiği günlükleri arar:

    ```kusto
    traces
    | where customDimensions contains "input"
    | limit 10
    ```

   [![izleme verileri](./media/how-to-enable-app-insights/model-data-trace.png)](././media/how-to-enable-app-insights/model-data-trace.png#lightbox)

Azure Application Insights kullanma hakkında daha fazla bilgi edinmek için bkz. [Application Insights nedir?](../azure-monitor/app/app-insights-overview.md).

## <a name="export-data-for-further-processing-and-longer-retention"></a>Daha fazla işleme ve daha uzun bekletme için verileri dışarı aktarma

>[!Important]
> Azure Application Insights yalnızca blob depolamaya dışarı aktarmaları destekler. Bu dışa aktarma yeteneğinin ek sınırları, [uygulama öngörülerine ait Telemetriyi dışarı aktarma](https://docs.microsoft.com/azure/azure-monitor/app/export-telemetry#continuous-export-advanced-storage-configuration)bölümünde listelenmiştir.

Azure Application Insights ' [sürekli dışarı aktarma](https://docs.microsoft.com/azure/azure-monitor/app/export-telemetry) ' yı, daha uzun bir saklama 'nin ayarlandığı desteklenen bir depolama hesabına ileti göndermek için kullanabilirsiniz. Veriler JSON biçiminde depolanır ve model verilerini ayıklamak için kolayca ayrıştırılabilir. 

Azure Data Factory, Azure ML işlem hatları veya diğer veri işleme araçları, verileri gerektiği şekilde dönüştürmek için kullanılabilir. Verileri dönüştürdüğünü daha sonra Azure Machine Learning çalışma alanıyla bir veri kümesi olarak kaydedebilirsiniz. Bunu yapmak için bkz. [veri kümesi oluşturma ve kaydetme](how-to-create-register-datasets.md).

:::image type="content" source="media/how-to-enable-app-insights/continuous-export-setup.png" alt-text="Sürekli dışarı aktarma":::


## <a name="example-notebook"></a>Örnek not defteri

[Enable-App-Insights-in-Production-Service. ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/enable-app-insights-in-production-service/enable-app-insights-in-production-service.ipynb) Not defteri, bu makaledeki kavramları gösterir. 
 
[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Kubernetes hizmet kümesine model dağıtma](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-azure-kubernetes-service) veya modellerinizi Web hizmeti uç noktalarına dağıtmak için [Azure Container Instances bir modeli dağıtma](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-azure-container-instance) ve Azure Application Insights veri toplamayı ve uç nokta izlemeyi kullanmasını sağlama
* Üretimde modellerden toplanan verileri kullanma hakkında daha fazla bilgi edinmek için bkz. [Mlops: Azure Machine Learning modelleri yönetme, dağıtma ve izleme](https://docs.microsoft.com/azure/machine-learning/concept-model-management-and-deployment) . Bu tür veriler, Machine Learning işleminizi sürekli olarak iyileştirmenize yardımcı olabilir
