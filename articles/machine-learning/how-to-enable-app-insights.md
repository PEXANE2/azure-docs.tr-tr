---
title: Machine Learning web hizmeti uç noktalarından veri izleme ve toplama
titleSuffix: Azure Machine Learning
description: Azure Uygulama Öngörüleri'ni kullanarak Azure Machine Learning ile dağıtılan web hizmetlerini izleyin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: larryfr
author: blackmist
ms.date: 03/12/2020
ms.openlocfilehash: 464ec1fcf0986dc04bd92bbe9e31b5675e5822d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79136202"
---
# <a name="monitor-and-collect-data-from-ml-web-service-endpoints"></a>ML web hizmeti uç noktalarından veri izleme ve toplama
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Uygulama Öngörülerini etkinleştirerek Azure Kubernetes Hizmeti (AKS) veya Azure Kapsayıcı Örnekleri'ndeki (ACI) web hizmeti bitiş noktalarına dağıtılan modellerden veri toplamayı ve izlemeyi öğreniyorsunuz. 
* [Azure Machine Öğrenme Python SDK](#python)
* [Azure Machine Learning stüdyosu](#studio)https://ml.azure.com

Bir uç noktanın çıktı verilerini ve yanıtını toplamaya ek olarak şunları izleyebilirsiniz:

* İstek oranları, yanıt süreleri ve başarısızlık oranları
* Bağımlılık oranları, yanıt süreleri ve başarısızlık oranları
* Özel durumlar

[Azure Uygulama Öngörüleri hakkında daha fazla bilgi edinin.](../azure-monitor/app/app-insights-overview.md) 


## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin

* Azure Machine Learning çalışma alanı, komut dosyalarınızı içeren yerel bir dizini ve Python için Azure Machine Learning SDK yüklü. Bu ön koşulları nasıl elde edeceğiz öğrenmek için geliştirme [ortamını nasıl yapılandırılatırsınız](how-to-configure-environment.md)

* Azure Kubernetes Hizmeti (AKS) veya Azure Kapsayıcı Örneği 'ne (ACI) dağıtılacak eğitimli bir makine öğrenme modeli. Eğer yoksa, [Tren görüntü sınıflandırma modeli](tutorial-train-models-with-aml.md) öğretici bakın

## <a name="web-service-metadata-and-response-data"></a>Web hizmeti meta verileri ve yanıt verileri

>[!Important]
> Azure Application Insights yalnızca 64 kb'ye kadar yükleri kaydeder. Bu sınıra ulaşılırsa, yalnızca modelin en son çıktıları günlüğe kaydedilir. 

Web hizmeti meta verilerine ve modelin tahminlerine karşılık gelen meta veriler ve hizmete yanıt, iletinin `"model_data_collection"`altındaki Azure Uygulama Öngörüleri izlemelerine kaydedilir. Bu verilere erişmek için doğrudan Azure Uygulama Öngörülerini sorgulayabilir veya daha uzun bekletme veya daha fazla işlem için bir depolama hesabına [sürekli bir dışak verme](https://docs.microsoft.com/azure/azure-monitor/app/export-telemetry) ayarlayabilirsiniz. Model verileri daha sonra Azure Machine Learning'de etiketleme, yeniden eğitim, açıklanabilirlik, veri analizi veya başka bir kullanım ayarlamak için kullanılabilir. 

<a name="python"></a>

## <a name="use-python-sdk-to-configure"></a>Yapılandırmak için Python SDK'yı kullanın 

### <a name="update-a-deployed-service"></a>Dağıtılan bir hizmeti güncelleştirme

1. Çalışma alanınızdaki hizmeti tanımlayın. `ws` Değer, çalışma alanınızın adıdır

    ```python
    from azureml.core.webservice import Webservice
    aks_service= Webservice(ws, "my-service-name")
    ```
2. Hizmetinizi güncelleyin ve Azure Uygulama Öngörüleri'ni etkinleştirin

    ```python
    aks_service.update(enable_app_insights=True)
    ```

### <a name="log-custom-traces-in-your-service"></a>Hizmetinizde özel izlemeleri günlüğe kaydetme

Özel izlemeleri günlüğe kaydetmek istiyorsanız, aks veya ACI için standart dağıtım işlemini izleme nin nasıl ve nerede belge [dağıtılanınca.](how-to-deploy-and-where.md) Ardından aşağıdaki adımları kullanın:

1. Yazdırma ekstreleri ekleyerek puanlama dosyasını güncelleştirme
    
    ```python
    print ("model initialized" + time.strftime("%H:%M:%S"))
    ```

2. Hizmet yapılandırmasını güncelleştirme
    
    ```python
    config = Webservice.deploy_configuration(enable_app_insights=True)
    ```

3. Bir görüntü oluşturun ve [AKS veya ACI](how-to-deploy-and-where.md)üzerinde dağıtın.

### <a name="disable-tracking-in-python"></a>Python'da izlemeyi devre dışı

Azure Uygulama Öngörülerini devre dışı kullanabilirsiniz:

```python 
## replace <service_name> with the name of the web service
<service_name>.update(enable_app_insights=False)
```

<a name="studio"></a>

## <a name="use-azure-machine-learning-studio-to-configure"></a>Yapılandırmak için Azure Machine Learning stüdyosu'ni kullanma

Modelinizi bu adımlarla dağıtmaya hazır olduğunuzda Azure Machine Learning stüdyosundan Azure Uygulama Öngörüleri'ni de etkinleştirebilirsiniz.

1. Çalışma alanınızda oturum açhttps://ml.azure.com/
1. **Modellere** gidin ve dağıtmak istediğiniz modeli seçin
1. **+Dağıt'ı** seçin
1. **Dağıt model** formunu doldurma
1. **Gelişmiş** menüyü genişletin

    ![Formu dağıt](./media/how-to-enable-app-insights/deploy-form.png)
1. **Uygulama Öngörüleri tanılama ve veri toplamayı etkinleştir'i** seçin

    ![Uygulama Öngörülerini Etkinleştir](./media/how-to-enable-app-insights/enable-app-insights.png)
## <a name="evaluate-data"></a>Verileri değerlendirme
Hizmetinizin verileri Azure Uygulama Öngörüleri hesabınızda, Azure Machine Learning ile aynı kaynak grubunda depolanır.
Görüntülemek için:

1. [Azure portalındaki](https://ms.portal.azure.com/) Azure Makine Öğrenimi çalışma alanınıza gidin ve Uygulama Öngörüleri bağlantısını tıklayın

    [![AppInsightsLoc](./media/how-to-enable-app-insights/AppInsightsLoc.png)](././media/how-to-enable-app-insights/AppInsightsLoc.png#lightbox)

1. Hizmetinizin temel metrik kümesini görmek için **Genel Bakış** sekmesini seçin

   [![Genel Bakış](./media/how-to-enable-app-insights/overview.png)](././media/how-to-enable-app-insights/overview.png#lightbox)

1. Web hizmetinizin meta veri ve yanıt istemesine bakmak için **Günlükler (Analitik)** bölümündeki **istekler** tablosunu seçin ve istekleri görüntülemek için **Çalıştır'ı** seçin

   [![Model verileri](./media/how-to-enable-app-insights/model-data-trace.png)](././media/how-to-enable-app-insights/model-data-trace.png#lightbox)


3. Özel izlemelerinizi görmek için **Analytics'i** seçin
4. Şema **bölümünde, İzler'i**seçin. Ardından sorgunuzu çalıştırmak için **Çalıştır'ı** seçin. Veriler tablo biçiminde görünmeli ve puanlama dosyanızdaki özel çağrılarınızla eşlenmeli

   [![Özel izlemeler](./media/how-to-enable-app-insights/logs.png)](././media/how-to-enable-app-insights/logs.png#lightbox)

Azure Uygulama Öngörüleri'ni nasıl kullanacağınız hakkında daha fazla bilgi edinmek için [Bkz. Uygulama Öngörüleri nedir?](../azure-monitor/app/app-insights-overview.md)

## <a name="export-data-for-further-processing-and-longer-retention"></a>Daha fazla işleme ve daha uzun saklama için veri dışa aktarma

>[!Important]
> Azure Application Insights yalnızca blob depolamasına yapılan dışa aktarmayı destekler. Bu ihracat kapasitesinin ek sınırları [App Insights'tan Dışa Aktarma telemetrisinde](https://docs.microsoft.com/azure/azure-monitor/app/export-telemetry#continuous-export-advanced-storage-configuration)listelenmiştir.

Daha uzun bir bekletme ayarlanabilecek desteklenen bir depolama hesabına ileti göndermek için Azure Application Insights'ın [sürekli dış aaktarılır'ini](https://docs.microsoft.com/azure/azure-monitor/app/export-telemetry) kullanabilirsiniz. İletiler `"model_data_collection"` JSON biçiminde depolanır ve model verilerini ayıklamak için kolayca ayrışabilir. 

Azure Veri Fabrikası, Azure ML Boru Hatları veya diğer veri işleme araçları, verileri gerektiği gibi dönüştürmek için kullanılabilir. Verileri dönüştürdüğünüzde, verileri veri kümesi olarak Azure Machine Learning çalışma alanına kaydedebilirsiniz. Bunu yapmak için [veri kümelerini nasıl oluşturup kaydedin.](how-to-create-register-datasets.md)

   [![Sürekli İhracat](./media/how-to-enable-app-insights/continuous-export-setup.png)](././media/how-to-enable-app-insights/continuous-export-setup.png)


## <a name="example-notebook"></a>Örnek not defteri

[Enable-app-insights-in-production-service.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/enable-app-insights-in-production-service/enable-app-insights-in-production-service.ipynb) dizüstü bilgisayar bu makalede kavramları gösterir. 
 
[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Bir modeli Azure Kubernetes Hizmet kümesine nasıl dağıtılayabildiğini](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-azure-kubernetes-service) veya modellerinizi web hizmeti uç noktalarına dağıtmak için Bir Modeli Azure Kapsayıcı [Örneklerine nasıl dağıtılabekleyeceğinizi](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-azure-container-instance) ve Azure Uygulama Öngörülerinin veri toplama ve uç nokta izlemeden yararlanmasını sağlamanızı sağlama
* MlOps: Üretimdeki modellerden toplanan verilerden yararlanma hakkında daha fazla bilgi edinmek için [Azure Machine Learning ile modelleri yönetin, dağıtın ve izleyin.](https://docs.microsoft.com/azure/machine-learning/concept-model-management-and-deployment) Bu tür veriler, makine öğrenimi sürecinizi sürekli olarak iyileştirmenize yardımcı olabilir
