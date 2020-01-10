---
title: Makine öğrenimi ardışık düzenleri hata ayıklama ve sorunlarını giderme
titleSuffix: Azure Machine Learning
description: Python için Azure Machine Learning SDK 'da makine öğrenimi ardışık düzenleri hata ayıklayın ve sorun giderin. İşlem hatları geliştirmek için ortak olan ve uzaktan yürütme sırasında ve betiklerinizde hata ayıklamanıza yardımcı olan ipuçları hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.date: 12/12/2019
ms.openlocfilehash: e0e4ea4689baeb8e85a765a33c3f8124d6bc4362
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75764295"
---
# <a name="debug-and-troubleshoot-machine-learning-pipelines"></a>Makine öğrenimi ardışık düzenleri hata ayıklama ve sorunlarını giderme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) ve [Azure Machine Learning Tasarımcısı 'nda (Önizleme)](https://docs.microsoft.com/azure/machine-learning/concept-designer) [makine öğrenimi](concept-ml-pipelines.md) işlem hatlarında hata ayıklamayı ve sorun gidermeyi öğreneceksiniz.


## <a name="debug-and-troubleshoot-in-the-azure-machine-learning-sdk"></a>Azure Machine Learning SDK 'da hata ayıklama ve sorun giderme
Aşağıdaki bölümler, işlem hatları oluştururken ortak olan genel bakışa genel bakış ve bir işlem hattında çalışan kodunuzda hata ayıklama için farklı stratejiler sağlar. İşlem hattını beklenen şekilde çalıştırmak için bir işlem hattı alırken sorun yaşadığınızda aşağıdaki ipuçlarını kullanın. 
### <a name="testing-scripts-locally"></a>Betikleri yerel olarak test etme

Bir işlem hattındaki en yaygın hatalardan biri, ekli bir betiğin (veri temizleme betiği, Puanlama betiği, vb.) amaçlanan gibi çalışmadığı veya uzak işlem bağlamında hata ayıklamanın Azure makinesindeki çalışma alanınızda hata ayıklaması zor olan çalışma zamanı hataları içerdiğinde Eğitim Studio. 

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

### <a name="debugging-scripts-from-remote-context"></a>Uzak bağlamdaki betiklerin hatalarını ayıklama

Komut dosyalarını yerel olarak test etmek, bir işlem hattı oluşturmaya başlamadan önce büyük kod parçalarının ve karmaşık mantığın hatalarını ayıklamanın harika bir yoludur. ancak bazı durumlarda gerçek işlem hattı sırasında betiklerde hata ayıklamanız gerekir, özellikle de oluşan davranışı tanılarken işlem hattı adımları arasındaki etkileşim sırasında. Adım betiklerinizde `print()` deyimlerinin serbest bir şekilde kullanılmasını öneririz; Böylece, JavaScript kodunun hatalarını ayıklamanıza benzer şekilde, uzaktan yürütme sırasında nesne durumunu ve beklenen değerleri görebilirsiniz.

`70_driver_log.txt` günlük dosyası şunları içerir: 

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

### <a name="troubleshooting-tips"></a>Sorun giderme ipuçları

Aşağıdaki tabloda, potansiyel çözümlerle birlikte işlem hattı geliştirme sırasında yaygın sorunlar yer almaktadır.

| Sorun | Olası çözüm |
|--|--|
| `PipelineData` dizinine veri geçirilemez | Komut dosyasında, işlem hattının adım çıkış verilerini beklediği yere karşılık gelen bir dizin oluşturduğunuzdan emin olun. Çoğu durumda, bir giriş bağımsız değişkeni çıkış dizinini tanımlar ve ardından dizini açıkça oluşturursunuz. Çıkış dizinini oluşturmak için `os.makedirs(args.output_dir, exist_ok=True)` kullanın. Bu tasarım modelini gösteren bir Puanlama betiği örneği için [öğreticiye](tutorial-pipeline-batch-scoring-classification.md#write-a-scoring-script) bakın. |
| Bağımlılık hataları | Komut dosyalarını yerel olarak geliştirdiyseniz, ancak işlem hattındaki uzak bir işlem üzerinde çalışırken bağımlılık sorunları bulursanız, işlem ortamı bağımlılıklarınızın ve sürümlerinin test ortamınızla eşleştiğinden emin olun. |
| İşlem hedefleri ile belirsiz hatalar | İşlem hedeflerini silme ve yeniden oluşturma işlemi, işlem hedefleriyle ilgili bazı sorunları çözebilir. |
| İşlem hattı adımları yeniden kullanma | Adım yeniden kullanım varsayılan olarak etkindir, ancak işlem hattı adımında devre dışı bırakılmadığınızdan emin olun. Yeniden kullanım devre dışıysa, adımdaki `allow_reuse` parametresi `False`olarak ayarlanır. |
| İşlem hattı gereksiz yere yeniden çalıştırılıyor | Adımların yalnızca temel alınan verileri veya betikleri değiştiğinde yeniden çalıştırılması için dizinlerinizi her bir adımla ayırın. Birden çok adım için aynı kaynak dizinini kullanırsanız, gereksiz yeniden çalıştırma işlemleri yaşayabilirsiniz. İşlem hattı adım nesnesindeki `source_directory` parametresini kullanarak bu adım için yalıtılmış dizininizi işaret edin ve birden çok adım için aynı `source_directory` yolunu kullanmadığınız emin olun. |

## <a name="debug-and-troubleshoot-in-azure-machine-learning-designer-preview"></a>Azure Machine Learning tasarımcısında hata ayıklama ve sorun giderme (Önizleme)

Bu bölümde, tasarımcıda işlem hatlarında sorun giderme hakkında genel bakış sunulmaktadır.
Tasarımcıda oluşturulan işlem hatları için, yazma sayfasında ya da işlem hattı çalıştırma ayrıntısı sayfasında **günlük dosyalarını** bulabilirsiniz.

### <a name="access-logs-from-the-authoring-page"></a>Yazma sayfasındaki günlüklere erişin

Bir işlem hattı çalıştırması gönderdiğinizde ve yazma sayfasında kaladığınızda, her modül için oluşturulan günlük dosyalarını bulabilirsiniz.

1. Yazma tuvalindeki herhangi bir modülü seçin.
1. Özellikler bölmesinde **Günlükler** sekmesine gidin.
1. Günlük dosyasını seçin `70_driver_log.txt`

    ![Yazma sayfası modül günlükleri](./media/how-to-debug-pipelines/pipelinerun-05.png)

### <a name="access-logs-from-pipeline-runs"></a>İşlem hattı çalıştırmalarından günlüklere erişim

Ayrıca, işlem **hatları** veya **denemeleri** bölümlerindeki işlem hattı çalışma ayrıntısı sayfasında belirli çalıştırmaların günlük dosyalarını bulabilirsiniz.

1. Tasarımcıda oluşturulan bir işlem hattı çalıştırması seçin.
    ![işlem hattı çalıştırma sayfası](./media/how-to-debug-pipelines/pipelinerun-04.png)
1. Önizleme bölmesinde herhangi bir modül seçin.
1. Özellikler bölmesinde **Günlükler** sekmesine gidin.
1. Günlük dosyasını seçin `70_driver_log.txt`

## <a name="next-steps"></a>Sonraki adımlar

* [Azureml işlem hatları-Core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) paketi ve [azureml-işlem hatları-adımlar](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) PAKETIYLE ilgili yardım için SDK başvurusuna bakın.

* [Tasarımcı özel durumları ve hata kodları](algorithm-module-reference/designer-error-codes.md)listesine bakın.
