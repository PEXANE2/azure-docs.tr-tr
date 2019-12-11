---
title: Machine Learning Web hizmeti uç noktalarından verileri izleme ve toplama
titleSuffix: Azure Machine Learning
description: Azure Application Insights kullanarak Azure Machine Learning ile dağıtılan Web hizmetlerini izleme
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: copeters
author: lostmygithubaccount
ms.date: 11/12/2019
ms.custom: seoapril2019
ms.openlocfilehash: f4bb399e24b2c480023eba6bf5b71990f6d39c8c
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74978382"
---
# <a name="monitor-and-collect-data-from-ml-web-service-endpoints"></a>ML Web hizmeti uç noktalarından verileri izleme ve toplama
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Application Insights etkinleştirerek Azure Kubernetes Service (AKS) veya Azure Container Instances (acı) içindeki Web hizmeti uç noktalarına dağıtılan modellerden veri toplamaya ve bunları izlemeye öğreneceksiniz. Bir uç noktanın giriş verilerini ve yanıtını toplamaya ek olarak şunları izleyebilirsiniz:

* İstek ücretleri, yanıt süreleri ve hata oranları
* Bağımlılık oranları, yanıt süreleri ve hata oranları
* Özel Durumlar

[Azure Application Insights hakkında daha fazla bilgi edinin](../../azure-monitor/app/app-insights-overview.md). 


## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin

* Bir Azure Machine Learning çalışma alanı, yüklü Python için betikleri ve Azure Machine Learning SDK'sını içeren yerel bir dizin. Bu önkoşulları nasıl alabileceğinizi öğrenmek için bkz. [geliştirme ortamını yapılandırma](how-to-configure-environment.md)
* Azure Kubernetes Service (AKS) veya Azure Container örneği (ACI) dağıtılması için eğitilen makine öğrenme modeli. Bir tane yoksa, bkz. [eğitim resmi sınıflandırma modeli](tutorial-train-models-with-aml.md) öğreticisi

## <a name="web-service-input-and-response-data"></a>Web hizmeti giriş ve yanıt verileri

ML modeline ve tahmine yönelik girişlere karşılık gelen giriş ve yanıt, Azure Application Insights izlerinde ileti `"model_data_collection"`altında günlüğe kaydedilir. Bu verilere erişmek için doğrudan Azure Application Insights sorgulayabilir veya daha uzun bekletme veya daha fazla işleme için depolama hesabına [sürekli bir dışarı aktarma](https://docs.microsoft.com/azure/azure-monitor/app/export-telemetry) ayarlayabilirsiniz. Model verileri daha sonra etiketleme, yeniden eğitim, explainability, veri analizi veya diğer kullanımı ayarlamak için Azure Machine Learning kullanılabilir. 

## <a name="use-the-azure-portal-to-configure"></a>Yapılandırmak için Azure portal kullanın

Azure portal Azure Application Insights etkinleştirebilir ve devre dışı bırakabilirsiniz. 

1. [Azure Portal](https://portal.azure.com), çalışma alanınızı açın

1. **Dağıtımlar** sekmesinde, Azure Application Insights etkinleştirmek istediğiniz hizmeti seçin

   [![Dağıtımları sekmesinde hizmetlerin listesi](media/how-to-enable-app-insights/Deployments.PNG)](./media/how-to-enable-app-insights/Deployments.PNG#lightbox)

3. **Düzenle** 'yi seçin

   [![Düzenle düğmesi](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

4. **Gelişmiş ayarlar**' da **Appınsights tanılamayı etkinleştir** onay kutusunu seçin.

   [![Seçili onay kutusu için tanılamayı etkinleştirme](media/how-to-enable-app-insights/AdvancedSettings.png)](./media/how-to-enable-app-insights/AdvancedSettings.png#lightbox)

1. Değişiklikleri uygulamak için ekranın alt kısmındaki **Güncelleştir** ' i seçin

### <a name="disable"></a>Devre dışı bırakma

1. [Azure Portal](https://portal.azure.com), çalışma alanınızı açın
1. **Dağıtımlar**' ı seçin, hizmeti seçin ve ardından **Düzenle** ' yi seçin.

   [![Düzenle düğmesini kullanın](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

1. **Gelişmiş ayarlar**' da **Appınsights tanılamayı etkinleştir** onay kutusunu temizleyin.

   [![Temizlenmiş onay kutusu için tanılamayı etkinleştirme](media/how-to-enable-app-insights/uncheck.png)](./media/how-to-enable-app-insights/uncheck.png#lightbox)

1. Değişiklikleri uygulamak için ekranın alt kısmındaki **Güncelleştir** ' i seçin
 
## <a name="use-python-sdk-to-configure"></a>Yapılandırmak için Python SDK 'sını kullanma 

### <a name="update-a-deployed-service"></a>Dağıtılan bir hizmette güncelleştir

1. Hizmet çalışma alanınızdaki belirleyin. `ws` değeri, çalışma alanınızın adıdır

    ```python
    from azureml.core.webservice import Webservice
    aks_service= Webservice(ws, "my-service-name")
    ```
2. Hizmetinizi güncelleştirme ve Azure Application Insights etkinleştirme

    ```python
    aks_service.update(enable_app_insights=True)
    ```

### <a name="log-custom-traces-in-your-service"></a>Özel günlük izlemelerini hizmetinizde

Günlük izlemeleri özel istiyorsanız, AKS veya ACI için standart dağıtım işlemini izleyin [nasıl dağıtılacağı ve nerede](how-to-deploy-and-where.md) belge. Ardından aşağıdaki adımları kullanın:

1. Yazdırma deyimleri ekleyerek Puanlama dosyasını güncelleştirme
    
    ```python
    print ("model initialized" + time.strftime("%H:%M:%S"))
    ```

2. Hizmet yapılandırmasını güncelleştirme
    
    ```python
    config = Webservice.deploy_configuration(enable_app_insights=True)
    ```

3. Bir görüntü oluşturun ve [aks](how-to-deploy-to-aks.md) veya [aci](how-to-deploy-to-aci.md) 'de dağıtın

### <a name="disable-tracking-in-python"></a>Python'da izleme devre dışı bırak

Azure Application Insights 'yi devre dışı bırakmak için aşağıdaki kodu kullanın:

```python 
## replace <service_name> with the name of the web service
<service_name>.update(enable_app_insights=False)
```

## <a name="evaluate-data"></a>Veri değerlendir
Hizmetinizin verileri, Azure Application Insights hesabınızda, Azure Machine Learning ile aynı kaynak grubu içinde depolanır.
Bunu görüntülemek için:

1. [Azure Machine Learning Studio](https://ml.azure.com) 'daki Azure Machine Learning çalışma alanınıza gidin ve Application Insights bağlantısına tıklayın

    [![AppInsightsLoc](media/how-to-enable-app-insights/AppInsightsLoc.png)](./media/how-to-enable-app-insights/AppInsightsLoc.png#lightbox)

1. Hizmetinizin temel ölçüm kümesini görmek için **genel bakış** sekmesini seçin

   [![Genel bakış](media/how-to-enable-app-insights/overview.png)](./media/how-to-enable-app-insights/overview.png#lightbox)

1. Web hizmeti giriş ve yanıt yüklerinizi görmek için **analiz** ' i seçin.
1. Şema bölümünde **izlemeler** ' ı seçin ve izleme `"model_data_collection"`ileti ile filtreleyin. Özel boyutlarda girişleri, tahminleri ve diğer ilgili ayrıntıları görebilirsiniz

   [Model verileri ![](media/how-to-enable-app-insights/model-data-trace.png)](./media/how-to-enable-app-insights/model-data-trace.png#lightbox)


3. Özel izlemelerinizi görmek için **analiz** ' i seçin
4. Şema bölümünde **izlemeleri**. Ardından **çalıştırma** sorgunuzu çalıştırılacak. Veriler bir tablo biçiminde görünmelidir ve Puanlama dosyanızdaki özel çağrılarınız ile eşleşmelidir

   [![Özel izlemeler](media/how-to-enable-app-insights/logs.png)](./media/how-to-enable-app-insights/logs.png#lightbox)

Azure Application Insights kullanma hakkında daha fazla bilgi edinmek için bkz. [Application Insights nedir?](../../azure-monitor/app/app-insights-overview.md).

## <a name="export-data-for-further-processing-and-longer-retention"></a>Daha fazla işleme ve daha uzun bekletme için verileri dışarı aktarma

Azure Application Insights ' [sürekli dışarı aktarma](https://docs.microsoft.com/azure/azure-monitor/app/export-telemetry) ' yı, daha uzun bir saklama 'nin ayarlandığı desteklenen bir depolama hesabına ileti göndermek için kullanabilirsiniz. `"model_data_collection"` iletileri JSON biçiminde depolanır ve model verilerini ayıklamak için kolayca ayrıştırılabilir. Azure Data Factory, Azure ML işlem hatları veya diğer veri işleme araçları, verileri gerektiği şekilde dönüştürmek için kullanılabilir. Verileri dönüştürdüğünü daha sonra Azure Machine Learning çalışma alanıyla bir veri kümesi olarak kaydedebilirsiniz. Bunu yapmak için bkz. [veri kümesi oluşturma ve kaydetme](how-to-create-register-datasets.md).

   [Sürekli dışarı aktarma ![](media/how-to-enable-app-insights/continuous-export-setup.png)](./media/how-to-enable-app-insights/continuous-export-setup.png)


## <a name="example-notebook"></a>Örneğin not defteri

[Enable-App-Insights-in-Production-Service. ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/enable-app-insights-in-production-service/enable-app-insights-in-production-service.ipynb) Not defteri, bu makaledeki kavramları gösterir. 
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Kubernetes hizmet kümesine model dağıtma](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-azure-kubernetes-service) veya modellerinizi Web hizmeti uç noktalarına dağıtmak için [Azure Container Instances bir modeli dağıtma](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-azure-container-instance) ve Azure Application Insights veri toplamayı ve uç nokta izlemeyi kullanmasını sağlama
* Üretimde modellerden toplanan verileri kullanma hakkında daha fazla bilgi edinmek için bkz. [Mlops: Azure Machine Learning modelleri yönetme, dağıtma ve izleme](https://docs.microsoft.com/azure/machine-learning/service/concept-model-management-and-deployment) . Bu tür veriler, Machine Learning işleminizi sürekli olarak iyileştirmenize yardımcı olabilir
