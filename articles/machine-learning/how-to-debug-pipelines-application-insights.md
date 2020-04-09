---
title: Application Insights'ta hata ayıklama ve sorun giderme makinesi öğrenme boru hatları
titleSuffix: Azure Machine Learning
description: Eğitim ve toplu puanlama ardışık hatlarınıza günlüğe kaydetme ekleyin ve Uygulama Öngörüleri'nde günlüğe kaydedilmiş sonuçları görüntüleyin.
services: machine-learning
author: sanpil
ms.author: sanpil
ms.service: machine-learning
ms.subservice: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/16/2020
ms.custom: seodec18
ms.openlocfilehash: b3e4bf19a7ec153f85483f3c5028e468e06ed7f0
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80982370"
---
# <a name="debug-and-troubleshoot-machine-learning-pipelines-in-application-insights"></a>Application Insights'ta hata ayıklama ve sorun giderme makinesi öğrenme boru hatları
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

[OpenCensus](https://opencensus.io/quickstart/python/) python kitaplığı, günlükleri komut dosyalarınızdan Uygulama Öngörüleri'ne yönlendirmek için kullanılabilir. Ardışık ardışık işlerden günlükleri tek bir yerde toplama, sorgular oluşturmanıza ve sorunları tanılamanıza olanak tanır. Uygulama Öngörüleri'ni kullanmak, zaman içinde günlükleri izlemenize ve akışlar arasında ardışık bilgi girişlerini karşılaştırmanıza olanak sağlar.

Günlüklerinizin bir kez yer olması, özel durumların ve hata iletilerinin geçmişini sağlar. Application Insights Azure Uyarıları ile tümleştirdiğinden, Uygulama Öngörüleri sorgularını temel alan uyarılar da oluşturabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* [Azure Machine Learning](./how-to-manage-workspace.md) çalışma alanı oluşturmak ve ilk [ardışık hattınızı oluşturmak](./how-to-create-your-first-pipeline.md) için adımları izleyin
* Azure Machine Learning SDK'yı yüklemek için [geliştirme ortamınızı yapılandırın.](./how-to-configure-environment.md)
* [OpenCensus Azure MonitörÜ İhracatçı](https://pypi.org/project/opencensus-ext-azure/) paketini yerel olarak yükleyin:
  ```python
  pip install opencensus-ext-azure
  ```
* Uygulama [Öngörüleri örneği](../azure-monitor/app/opencensus-python.md) oluşturun (bu doküman, kaynak için bağlantı dizesini alma yla ilgili bilgileri de içerir)

## <a name="getting-started"></a>Başlarken

Bu bölüm, Bir Azure Machine Learning ardışık bölümünden OpenCensus'ı kullanmaya özel bir giriştir. Ayrıntılı bir eğitim için [OpenCensus Azure Monitör İhracatçıları](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-azure)

Azure ML Ardışık Alanınıza Bir PythonScriptStep ekleyin. [RunConfiguration'ınızı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py) opencensus-ext-azure'a bağımlılıkla yapılandırın. Ortam değişkenini `APPLICATIONINSIGHTS_CONNECTION_STRING` yapılandırın.

```python
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core.runconfig import RunConfiguration
from azureml.pipeline.core import Pipeline
from azureml.pipeline.steps import PythonScriptStep

# Connecting to the workspace and compute target not shown

# Add pip dependency on OpenCensus
dependencies = CondaDependencies()
dependencies.add_pip_package("opencensus-ext-azure>=1.0.1")
run_config = RunConfiguration(conda_dependencies=dependencies)

# Add environment variable with Application Insights Connection String
# Replace the value with your own connection string
run_config.environment.environment_variables = {
    "APPLICATIONINSIGHTS_CONNECTION_STRING": 'InstrumentationKey=00000000-0000-0000-0000-000000000000'
}

# Configure step with runconfig
sample_step = PythonScriptStep(
        script_name="sample_step.py",
        compute_target=compute_target,
        runconfig=run_config
)

# Submit new pipeline run
pipeline = Pipeline(workspace=ws, steps=[sample_step])
pipeline.submit(experiment_name="Logging_Experiment")
```

`sample_step.py` adlı bir dosya oluşturun. Günlükleri Uygulama Öngörüleri'ne yönlendirmek için AzureLogHandler sınıfını aktarın. Python Günlük kitaplığını da almanız gerekir.

```python
from opencensus.ext.azure.log_exporter import AzureLogHandler
import logging
```

Ardından, Python logger'ına AzureLogHandler'ı ekleyin.

```python
logger = logging.getLogger(__name__)
logger.setLevel(logging.DEBUG)
logger.addHandler(logging.StreamHandler())

# Assumes the environment variable APPLICATIONINSIGHTS_CONNECTION_STRING is already set
logger.addHandler(AzureLogHandler())
logger.warning("I will be sent to Application Insights")
```

## <a name="logging-with-custom-dimensions"></a>Özel Boyutlarla Günlüğe Kaydetme
 
Varsayılan olarak, Uygulama Öngörüleri'ne iletilen günlükler, çalıştırılan veya denemeyi izlemek için yeterli içeriğe sahip olmaz. Günlükleri tanılama sorunları için eyleme geçirilebilir hale getirmek için ek alanlar gerekir. 

Bu alanları eklemek için, günlük iletisine bağlam sağlamak için Özel Boyutlar eklenebilir. Bir örnek, bir kişinin aynı ardışık ardışık çalışmada birden çok adımda günlükleri görüntülemek istemesidir.

Özel Boyutlar anahtar değeri (dize, dize olarak depolanan) çiftleri bir sözlük oluşturan. Sözlük daha sonra Application Insights'a gönderilir ve sorgu sonuçlarında sütun olarak görüntülenir. Bireysel boyutları [sorgu parametreleri](#additional-helpful-queries)olarak kullanılabilir.

### <a name="helpful-context-to-include"></a>İçerecek Yararlı Bağlam

| Alan                          | Muhakeme/Örnek                                                                                                                                                                       |
|--------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| parent_run_id                  | Her adıma dalmak yerine tüm adımlar için zaman içinde günlükleri görmek için aynı parent_run_id sahip olanların günlüklerini sorgulayabilir                                        |
| step_id                        | Bir sorunun dar bir kapsamla nerede oluştuğunu tek tek adıma göre görmek için aynı step_id sahip olanların günlüklerini sorgulayabilir mi?                                                        |
| step_name                      | Zaman içinde adım performansını görmek için günlükleri sorgulayabilir. Ayrıca portal UI içine dalış olmadan son çalışır için bir step_id bulmak için yardımcı olur                                          |
| experiment_name                | Zaman içinde deneme performansını görmek için günlükler arasında sorgu yapabilir. Ayrıca portal UI içine dalış olmadan son çalışır için bir parent_run_id veya step_id bulmanıza yardımcı olur                   |
| run_url                 | Doğrudan geri soruşturma için çalıştırmak için bir bağlantı sağlayabilir. |

**Diğer yararlı alanlar**

Bu alanlar ek kod enstrümantasyonu gerektirebilir ve çalıştırılaç bağlamı tarafından sağlanmaz.

| Alan                   | Muhakeme/Örnek                                                                                                                                                                                                           |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| build_url/build_version | Dağıtmak için CI/CD kullanıyorsanız, bu alan günlükleri adım ve ardışık işlem mantığını sağlayan kod sürümüyle ilişkilendirebilir. Bu bağlantı, sorunları tanılamaya veya belirli özelliklere (günlük/metrik değerler) sahip modelleri tanımlamaya daha fazla yardımcı olabilir |
| run_type                       | Farklı model türleri veya eğitim ve puanlama çalıştırmaları arasında ayrım yapabilir |

### <a name="creating-a-custom-dimensions-dictionary"></a>Özel Boyutlar sözlüğü oluşturma

```python
from azureml.core import Run

run = Run.get_context(allow_offline=False)

custom_dimensions = {
    "parent_run_id": run.parent.id,
    "step_id": run.id,
    "step_name": run.name,
    "experiment_name": run.experiment.name,
    "run_url": run.parent.get_portal_url(),
    "run_type": "training"
}

# Assumes AzureLogHandler was already registered above
logger.info("I will be sent to Application Insights with Custom Dimensions", custom_dimensions)
```

## <a name="opencensus-python-logging-considerations"></a>OpenCensus Python günlük hususlar

OpenCensus AzureLogHandler, Python günlüklerini Uygulama Öngörüleri'ne yönlendirmek için kullanılır. Sonuç olarak, Python günlük nüansları dikkate alınmalıdır. Bir logger oluşturulduğunda, varsayılan günlük düzeyi ne kadardır ve günlükleri bu düzeyden daha büyük veya eşit gösterir. Python günlük özelliklerini kullanmak için iyi bir referans [Günlük Yemek Kitabı](https://docs.python.org/3/howto/logging-cookbook.html)olduğunu.

OpenCensus kitaplığı için `APPLICATIONINSIGHTS_CONNECTION_STRING` ortam değişkeni gereklidir. Düz metin bağlantı dizelerinin etrafından geçmesini önlemek için bu ortam değişkenini bir boru hattı parametresi olarak geçirmek yerine ayarlamanızı öneririz.

## <a name="querying-logs-in-application-insights"></a>Uygulama Öngörüleri'nde günlükleri sorgulama

Uygulama Öngörüleri'ne yönlendirilen günlükler 'izlemeler' veya 'özel durumlar' altında gösterilecek. Zaman pencerenizi boru hattı çalıştırınızı içerecek şekilde ayarladıknızdan emin olun.

![Uygulama Öngörüleri Sorgu sonucu](./media/how-to-debug-pipelines-application-insights/traces-application-insights-query.png)

Application Insights'taki sonuç, günlük iletisini ve düzeyini, dosya yolunu ve kod satır numarasını gösterir. Ayrıca dahil herhangi bir özel boyutları gösterecektir. Bu resimde, customDimensions sözlüğü önceki [kod örneğindeki](#creating-a-custom-dimensions-dictionary)anahtar/değer çiftlerini gösterir.

### <a name="additional-helpful-queries"></a>Ek yararlı sorgular

Aşağıdaki sorgulardan bazıları 'customDimensions.Level' kullanır. Bu önem düzeyleri Python günlüğünün başlangıçta gönderildiği düzeye karşılık gelir. Ek sorgu bilgileri için [Azure Monitör günlük sorgularına](https://docs.microsoft.com/azure/azure-monitor/log-query/query-language)bakın.

| Kullanım örneği                                                               | Sorgu                                                                                              |
|------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Belirli özel boyut için günlük sonuçları, örneğin 'parent_run_id' | <pre>traces \| <br>where customDimensions.parent_run_id == '931024c2-3720-11ea-b247-c49deda841c1</pre> |
| Son 7 gün içinde yapılan tüm eğitimlerin sonuçlarını günlüğe kaydetme                     | <pre>traces \| <br>where timestamp > ago(7d) <br>and customDimensions.run_type == 'training'</pre>           |
| Son 7 güne ait önem düzeyi Hatası ile sonuçları günlüğe kaydetme              | <pre>traces \| <br>where timestamp > ago(7d) <br>and customDimensions.Level == 'ERROR'                     |
| Son 7 gün içinde önem düzeyi Hatası ile günlük sonuçlarının sayısı     | <pre>traces \| <br>where timestamp > ago(7d) <br>and customDimensions.Level == 'ERROR' \| <br>summarize count()</pre> |

## <a name="next-steps"></a>Sonraki Adımlar

Uygulama Öngörüleri örneğinde günlükleri olduktan sonra, sorgu sonuçlarına göre [Azure Monitor uyarılarını](../azure-monitor/platform/alerts-overview.md#what-you-can-alert-on) ayarlamak için kullanılabilir.

Ayrıca, ek öngörüler için sorgulardan sonuçları Azure [Panosuna](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-app-dashboards#add-logs-analytics-query) ekleyebilirsiniz.
