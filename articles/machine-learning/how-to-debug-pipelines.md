---
title: Hata ayıklama & ML işlem hatları sorunlarını giderme
titleSuffix: Azure Machine Learning
description: Python 'da Azure Machine Learning işlem hatlarınızı hata ayıklayın. İşlem hatları geliştirmek için ortak olan ve uzaktan yürütme sırasında ve betiklerinizde hata ayıklamanıza yardımcı olan ipuçları hakkında bilgi edinin. Visual Studio Code kullanarak Machine Learning işlem hatlarınızı etkileşimli olarak hata ayıklamanızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
author: lobrien
ms.author: laobri
ms.date: 08/28/2020
ms.topic: conceptual
ms.custom: troubleshooting, devx-track-python
ms.openlocfilehash: 0f051e5b5711cec9fd8e72ec2b84c18f80430a0a
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89018068"
---
# <a name="debug-and-troubleshoot-machine-learning-pipelines"></a>Makine öğrenmesi işlem hatlarında hata ayıklama ve sorun giderme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) ve [Azure Machine Learning Tasarımcısı 'nda (Önizleme)](https://docs.microsoft.com/azure/machine-learning/concept-designer) [makine öğrenimi ardışık düzenleri](concept-ml-pipelines.md) sorunlarını gidermeye ve hata ayıklamanıza öğrenirsiniz. 

## <a name="troubleshooting-tips"></a>Sorun giderme ipuçları

Aşağıdaki tabloda, potansiyel çözümlerle birlikte işlem hattı geliştirme sırasında yaygın sorunlar yer almaktadır.

| Sorun | Olası çözüm |
|--|--|
| Verileri dizine geçiremedi `PipelineData` | Komut dosyasında, işlem hattının adım çıkış verilerini beklediği yere karşılık gelen bir dizin oluşturduğunuzdan emin olun. Çoğu durumda, bir giriş bağımsız değişkeni çıkış dizinini tanımlar ve ardından dizini açıkça oluşturursunuz. `os.makedirs(args.output_dir, exist_ok=True)`Çıkış dizinini oluşturmak için kullanın. Bu tasarım modelini gösteren bir Puanlama betiği örneği için [öğreticiye](tutorial-pipeline-batch-scoring-classification.md#write-a-scoring-script) bakın. |
| Bağımlılık hataları | Yerel olarak test edilirken gerçekleşmeyen uzak işlem hattınızda bağımlılık hataları görürseniz, uzak ortam bağımlılıklarınızın ve sürümlerinin test ortamınızdaki olanlarla eşleştiğinden emin olun. (Bkz. [ortam oluşturma, önbelleğe alma ve yeniden kullanma](https://docs.microsoft.com/azure/machine-learning/concept-environments#environment-building-caching-and-reuse)|
| İşlem hedefleri ile belirsiz hatalar | İşlem hedeflerini silmeyi ve yeniden oluşturmayı deneyin. İşlem hedeflerini yeniden oluşturmak hızlı bir şekilde yapılır ve bazı geçici sorunları çözebilir. |
| İşlem hattı adımları yeniden kullanma | Adım yeniden kullanım varsayılan olarak etkindir, ancak işlem hattı adımında devre dışı bırakılmadığınızdan emin olun. Yeniden kullanım devre dışıysa, `allow_reuse` adımdaki parametre olarak ayarlanır `False` . |
| İşlem hattı gereksiz yere yeniden çalıştırılıyor | Adımların yalnızca temel alınan verileri veya betikleri değiştiğinde yeniden çalıştığından emin olmak için, her adımın kaynak kodu dizinlerinizi ayırın. Birden çok adım için aynı kaynak dizinini kullanırsanız, gereksiz yeniden çalıştırma işlemleri yaşayabilirsiniz. İşlem `source_directory` hattı adımı nesnesi üzerinde parametresini kullanarak bu adım için yalıtılmış dizininizi işaret edin ve `source_directory` birden çok adım için aynı yolu kullanmadığınız emin olun. |


## <a name="debugging-techniques"></a>Hata ayıklama teknikleri

Hata ayıklama ardışık düzenleri için üç önemli teknik vardır: 

* Yerel bilgisayarınızda ayrı ardışık düzen adımlarında hata ayıklayın
* Sorunun kaynağını yalıtmak ve tanılamak için günlük kaydı ve Application Insights kullanın
* Azure 'da çalışan bir işlem hattına uzaktan hata ayıklayıcı iliştirme

### <a name="debug-scripts-locally"></a>Betiklerin yerel olarak hata ayıklama

Bir işlem hattındaki en yaygın hatalardan biri, etki alanı betiğinin istendiği gibi çalıştırılmasıdır veya uzaktan işlem bağlamında hata ayıklaması zor olan çalışma zamanı hataları içerir.

Ardışık düzenleri yerel olarak çalıştırılamaz, ancak komut dosyalarını yerel makinenizde yalıtımlarda çalıştırmak, işlem ve ortam oluşturma işlemini beklemek zorunda olmadığınızdan daha hızlı hata ayıklamanıza olanak tanır. Bunu yapmak için bazı geliştirme işleri gereklidir:

* Verileriniz bir bulut veri deposunda yer alıyorsa, verileri indirmeniz ve betiğiniz için kullanılabilir hale getirmeniz gerekir. Verilerinizin küçük bir örneğini kullanmak çalışma zamanını kesmek ve betik davranışında hızlıca geri bildirim almak için iyi bir yoldur
* Ara işlem hattı adımının benzetimini gerçekleştirmeye çalışıyorsanız, belirli bir betiğin önceki adımdan beklediği nesne türlerini el ile oluşturmanız gerekebilir
* Ayrıca kendi ortamınızı tanımlamanız ve Uzaktan işlem ortamınızda tanımlanan bağımlılıkları çoğaltmanız gerekecektir

Yerel ortamınızda çalıştırmak üzere bir betik kurulumuna sahip olduktan sonra, şu gibi hata ayıklama görevlerinin olması çok daha kolaydır:

* Özel hata ayıklama yapılandırması iliştirme
* Yürütmeyi duraklatma ve nesne durumunu inceleme
* Çalışma zamanına kadar sunulmayacak tür veya mantıksal hataları yakalama

> [!TIP] 
> Betiğinizin beklendiği gibi çalıştığını doğrulayabilmeniz için, bir sonraki adım komut dosyasını birden çok adımla bir işlem hattında çalıştırmayı denemeden önce tek adımlı bir işlem hattında çalıştırıyor.

## <a name="configure-write-to-and-review-pipeline-logs"></a>İşlem hattı günlüklerini yapılandırma, yazma ve gözden geçirme

Komut dosyalarını yerel olarak test etmek, bir işlem hattı oluşturmaya başlamadan önce büyük kod parçalarının ve karmaşık mantığın hatalarını ayıklamanın harika bir yoludur. ancak bazı bir noktada, özellikle de işlem hattı adımları arasındaki etkileşim sırasında oluşan davranışı tanılarken, büyük olasılıkla gerçek ardışık düzen sırasında betiklerde hata ayıklaması yapmanız gerekir. `print()`Uzaktan yürütme sırasında nesne durumunu ve beklenen değerleri görebilmeniz için, JavaScript kodunun hatalarını ayıklamanıza benzer şekilde, adım betiklerinizde deyimlerin serbest bir şekilde kullanılmasını öneririz.

### <a name="logging-options-and-behavior"></a>Günlüğe kaydetme seçenekleri ve davranışı

Aşağıdaki tabloda, işlem hatları için farklı hata ayıklama seçenekleri sağlanmıştır. Burada yalnızca Azure Machine Learning, Python ve OpenCensus seçeneklerinin yanı sıra diğer seçenekler mevcut olduğundan, kapsamlı bir liste değildir.

| Kitaplık                    | Tür   | Örnek                                                          | Hedef                                  | Kaynaklar                                                                                                                                                                                                                                                                                                                    |
|----------------------------|--------|------------------------------------------------------------------|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Azure Machine Learning SDK’sı | Ölçüm | `run.log(name, val)`                                             | Azure Machine Learning Portal Kullanıcı arabirimi             | [Denemeleri izleme](how-to-track-experiments.md)<br>[azureml. Core. Run sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=experimental)                                                                                                                                                 |
| Python yazdırma/günlüğe kaydetme    | Günlük    | `print(val)`<br>`logging.info(message)`                          | Sürücü günlükleri, Azure Machine Learning Tasarımcısı | [Denemeleri izleme](how-to-track-experiments.md)<br><br>[Python günlüğü](https://docs.python.org/2/library/logging.html)                                                                                                                                                                       |
| OpenCensus Python          | Günlük    | `logger.addHandler(AzureLogHandler())`<br>`logging.log(message)` | Application Insights-izlemeler                | [Application Insights’ta işlem hatlarında hata ayıklama](how-to-debug-pipelines-application-insights.md)<br><br>[OpenCensus Azure İzleyici Dışarı Aktarıcıları](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-azure)<br>[Python günlüğü tanıtım rehberi](https://docs.python.org/3/howto/logging-cookbook.html) |

#### <a name="logging-options-example"></a>Günlüğe kaydetme seçenekleri örneği

```python
import logging

from azureml.core.run import Run
from opencensus.ext.azure.log_exporter import AzureLogHandler

run = Run.get_context()

# Azure ML Scalar value logging
run.log("scalar_value", 0.95)

# Python print statement
print("I am a python print statement, I will be sent to the driver logs.")

# Initialize python logger
logger = logging.getLogger(__name__)
logger.setLevel(args.log_level)

# Plain python logging statements
logger.debug("I am a plain debug statement, I will be sent to the driver logs.")
logger.info("I am a plain info statement, I will be sent to the driver logs.")

handler = AzureLogHandler(connection_string='<connection string>')
logger.addHandler(handler)

# Python logging with OpenCensus AzureLogHandler
logger.warning("I am an OpenCensus warning statement, find me in Application Insights!")
logger.error("I am an OpenCensus error statement with custom dimensions", {'step_id': run.id})
``` 

### <a name="finding-and-reading-pipeline-log-files"></a>İşlem hattı günlük dosyalarını bulma ve okuma

Günlük dosyası `70_driver_log.txt` şunları içerir: 

* Betiğinizin yürütülmesi sırasında tüm yazdırılmış deyimler
* Betik için yığın izlemesi 

Portalda bu ve diğer günlük dosyalarını bulmak için, önce çalışma alanınızdaki işlem hattına tıklayın.

![İşlem hattı çalıştırma listesi sayfası](./media/how-to-debug-pipelines/pipelinerun-01.png)

İşlem hattı çalıştırma ayrıntısı sayfasına gidin.

![İşlem hattı çalıştırma ayrıntısı sayfası](./media/how-to-debug-pipelines/pipelinerun-02.png)

Belirli bir adım için modüle tıklayın. **Günlükler** sekmesine gidin. Diğer Günlükler, ortam görüntüsü oluşturma işlemi ve adım hazırlama betikleriniz hakkında bilgiler içerir.

![İşlem hattı çalıştırma ayrıntısı sayfa günlüğü sekmesi](./media/how-to-debug-pipelines/pipelinerun-03.png)

> [!TIP]
> Yayınlanan işlem *hatları* için çalıştırmalar çalışma alanınızdaki **uç noktalar** sekmesinde bulunabilir. *Yayımlanamayan işlem hatları* için çalıştırmalar, **denemeleri** veya işlem **hatları**içinde bulunabilir.

Günlüğe kaydetme ve izleme hakkında daha fazla bilgi için `ParallelRunStep` bkz. [hata ayıklama ve ParallelRunStep sorunlarını giderme](how-to-debug-parallel-run-step.md).

## <a name="logging-in-azure-machine-learning-designer-preview"></a>Azure Machine Learning tasarımcısında oturum açma (Önizleme)

Tasarımcıda oluşturulan işlem hatları için, **70_driver_log** dosyasını yazma sayfasında veya işlem hattı çalıştırma ayrıntısı sayfasında bulabilirsiniz.

### <a name="enable-logging-for-real-time-endpoints"></a>Gerçek zamanlı uç noktalar için günlüğe kaydetmeyi etkinleştirme

Tasarımcıda gerçek zamanlı uç noktalarda sorun gidermek ve hata ayıklamak için SDK 'yı kullanarak Application Insight Logging 'i etkinleştirmeniz gerekir. Günlüğe kaydetme, model dağıtım ve kullanım sorunlarını gidermenize ve hata ayıklamanıza olanak sağlar. Daha fazla bilgi için bkz. [dağıtılan modeller Için günlüğe kaydetme](how-to-enable-logging.md#logging-for-deployed-models). 

### <a name="get-logs-from-the-authoring-page"></a>Yazma sayfasından günlükleri al

Bir işlem hattı çalıştırması gönderdiğinizde ve yazma sayfasında kaladığınızda, her modül çalışmayı bitirdiğinde her modül için oluşturulan günlük dosyalarını bulabilirsiniz.

1. Yazma tuvalinde çalışmayı tamamlamış bir modül seçin.
1. Modülün sağ bölmesinde,  **çıktılar + Günlükler** sekmesine gidin.
1. Sağ bölmeyi genişletin ve dosyayı tarayıcıda görüntülemek için **70_driver_log.txt** seçin. Günlükleri yerel olarak da indirebilirsiniz.

    ![Tasarımcıda genişletilmiş çıkış bölmesi](./media/how-to-debug-pipelines/designer-logs.png)

### <a name="get-logs-from-pipeline-runs"></a>İşlem hattı çalıştırmalarından günlükleri al

Ayrıca, Studio 'nun işlem **hatları** veya **denemeleri** bölümünde bulunan işlem hattı çalışma ayrıntısı sayfasında belirli çalıştırmalar için günlük dosyalarını bulabilirsiniz.

1. Tasarımcıda oluşturulan bir işlem hattı çalıştırması seçin.

    ![İşlem hattı çalıştırma sayfası](./media/how-to-debug-pipelines/designer-pipelines.png)

1. Önizleme bölmesinde bir modül seçin.
1. Modülün sağ bölmesinde,  **çıktılar + Günlükler** sekmesine gidin.
1. **70_driver_log.txt** dosyasını tarayıcıda görüntülemek için sağ bölmeyi genişletin veya günlükleri yerel olarak indirmek için dosyayı seçin.

> [!IMPORTANT]
> İşlem hattı çalıştırma ayrıntıları sayfasından bir işlem hattını güncelleştirmek için, işlem hattı çalıştırmasını yeni bir işlem hattı taslağında **kopyalamanız** gerekir. İşlem hattı çalıştırması, işlem hattının anlık görüntüsüdür. Bir günlük dosyasına benzer ve değiştirilemez. 

## <a name="application-insights"></a>Application Insights
OpenCensus Python kitaplığını bu şekilde kullanma hakkında daha fazla bilgi için şu kılavuza bakın: [Application Insights Machine Learning işlem hatlarında hata ayıklama ve sorun giderme](how-to-debug-pipelines-application-insights.md)

## <a name="interactive-debugging-with-visual-studio-code"></a>Visual Studio Code ile etkileşimli hata ayıklama

Bazı durumlarda, ML ardışık düzeninde kullanılan Python kodunda etkileşimli olarak hata ayıklaması yapmanız gerekebilir. Visual Studio Code (VS Code) ve hata ayıklama GPY kullanarak, eğitim ortamında çalışırken koda ekleyebilirsiniz. Daha fazla bilgi için [vs Code kılavuzunda etkileşimli hata ayıklamayı](how-to-debug-visual-studio-code.md#debug-and-troubleshoot-machine-learning-pipelines)ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azureml işlem hatları-Core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) paketi ve [azureml-işlem hatları-adımlar](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) PAKETIYLE ilgili yardım için SDK başvurusuna bakın.

* [Tasarımcı özel durumları ve hata kodları](algorithm-module-reference/designer-error-codes.md)listesine bakın.
