---
title: ML işlem hatları nelerdir
titleSuffix: Azure Machine Learning service
description: Bu makalede, makine öğrenimi işlem hatları için Python ve işlem hatlarını kullanmanın avantajları Azure Machine Learning SDK ile birlikte derleme hakkında bilgi edinin. Machine learning (ML) işlem hatları oluşturmak, en iyi duruma getirmek ve makine öğrenimi iş akışları yönetmek için veri uzmanları tarafından kullanılır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: sanpil
author: sanpil
ms.date: 08/08/2019
ms.custom: seodec18
ms.openlocfilehash: 2547ca3af3039af53cb5fd5b244669b9152c21b3
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70993296"
---
# <a name="what-are-ml-pipelines-in-azure-machine-learning-service"></a>Azure Machine Learning hizmetinde ML işlem hatları nelerdir?

Azure Machine Learning hizmeti ile oluşturabileceğiniz ve yönetebileceğiniz makine öğrenimi ardışık düzenleri hakkında bilgi edinin. 

Machine learning (ML) işlem hatları, veri bilimcileri, veri mühendisleri ve BT uzmanları kullanarak içinde yer alan adımların üzerinde işbirliği yapabilirsiniz:
+ Normalizations ve dönüştürmeler gibi veri hazırlama
+ Modeli eğitimi
+ Modeli değerlendirme
+ Dağıtım

Bilgi edinmek için nasıl [ilk işlem hattınızı oluşturma](how-to-create-your-first-pipeline.md).

![Azure Machine Learning hizmetinde makine öğrenimi ardışık düzenleri](./media/concept-ml-pipelines/pipeline-flow.png)

<a name="compare"></a>
### <a name="which-azure-pipeline-technology-should-i-use"></a>Hangi Azure ardışık düzen teknolojisini kullanmalıyım?

Azure bulutu, her biri farklı bir amaca sahip birkaç farklı işlem hattı sağlar. Aşağıdaki tabloda, farklı işlem hatları ve bunların kullanıldıkları özellikler listelenmiştir:

| İşlem hattı | Ne yapar? | Kurallı Kanal |
| ---- | ---- | ---- |
| Azure Machine Learning işlem hatları | Makine öğrenimi senaryolarınız için şablon olarak kullanılabilen yeniden kullanılabilir makine öğrenimi iş akışlarını tanımlar. | Veri > modeli |
| [Azure Data Factory işlem hatları](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) | Bir görevi gerçekleştirmek için gereken veri taşıma, dönüştürme ve denetim etkinliklerini gruplandırır.  | Veri > verileri |
| [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/) | Uygulamanızın herhangi bir platforma/buluta yönelik sürekli tümleştirme ve teslim  | Kod-> uygulaması/hizmeti |

## <a name="why-build-pipelines-with-azure-machine-learning"></a>Neden Azure Machine Learning ile işlem hatları oluşturabilir?

Makine öğrenimi ardışık düzenleri, iş akışınızı hız, taşınabilirlik ve yeniden kullanım açısından iyileştirerek altyapı ve otomasyon yerine uzmanınıza, makine öğrenimine odaklanmanıza olanak sağlayabilir.

İşlem hatları, işlem hattının farklı hesaplama birimleri olan birden çok **adımdan**oluşturulur. Her adım bağımsız olarak çalıştırılabilir ve yalıtılmış işlem kaynakları kullanabilir.
Bağımsız adımlar, birden çok veri bilimcilerinin aynı işlem hattında aynı anda aynı işlem hattı üzerinde çalışmasına izin verir ve ayrıca her adım için farklı işlem türleri/boyutları kullanmayı kolaylaştırır.

İşlem hattı tasarlandıktan sonra, işlem hattının eğitim döngüsü etrafında genellikle daha fazla ince ayar yapılır. Bir işlem hattını yeniden çalıştırdığınızda, çalıştırma, güncelleştirilmiş bir eğitim betiği gibi yeniden çalıştırılması gereken ayrı adımlara atlar ve nelerin değiştirilmediğini atlar. Aynı paradigma adımının yürütülmesi için kullanılan aynı komut için geçerlidir. Bu yeniden kullanım işlevselliği, temel alınan veriler değişmemişse veri alımı ve dönüştürme gibi maliyetli ve zaman uyumlu adımların çalıştırılmasının önlenmesine yardımcı olur.

Azure Machine Learning, işlem hattınızdaki her adım için PyTorch veya TensorFlow gibi çeşitli araç setleri ve çerçeveler kullanabilirsiniz. Ara verilerinizin aşağı akış işlem hedefleri ile kolayca paylaşılabilmesi için, kullandığınız çeşitli [işlem hedefleri](concept-azure-machine-learning-architecture.md) arasında Azure koordinatları.

İşlem [hattı denemeleri için ölçümleri](https://docs.microsoft.com/azure/machine-learning/service/how-to-track-experiments) doğrudan Azure Portal veya [çalışma alanı giriş sayfanız (Önizleme)](https://ml.azure.com)içinde izleyebilirsiniz. Bir işlem hattı yayımlandıktan sonra, herhangi bir platform veya yığından işlem hattını yeniden çalıştırmanıza olanak sağlayan bir REST uç noktası yapılandırabilirsiniz.

## <a name="key-advantages"></a>Başlıca avantajları

Machine Learning iş akışlarınız için işlem hatlarını kullanmanın temel avantajları şunlardır:

|Önemli bir avantajı|Açıklama|
|:-------:|-----------|
|**Katılımsız&nbsp;çalıştırır**|Adımları güvenilir ve katılımsız bir şekilde paralel veya sırayla çalışacak şekilde zamanlayın. Veri hazırlama ve modelleme son günler veya haftadır ve işlem hatları, işlem çalışırken diğer görevlere odaklanabilmenize olanak tanır. |
|**Heterojen işlem**|Heterojen ve ölçeklenebilir işlem kaynakları ve depolama konumları genelinde güvenilir bir şekilde koordine edilen birden çok işlem hattı kullanın. HDInsight, GPU veri bilimi VM 'Leri ve Databricks gibi farklı işlem hedeflerinde bireysel ardışık düzen adımlarını çalıştırarak kullanılabilir işlem kaynaklarının verimli bir şekilde kullanılmasını sağlayın.|
|**Yeniden kullanılırlığı**|Yeniden eğitme ve Batch Puanlama gibi belirli senaryolar için ardışık düzen şablonları oluşturun. Dış sistemlerden yayınlanan işlem hatlarını basit REST çağrıları aracılığıyla tetikleyin.|
|**İzleme ve sürüm oluşturma**|Yineleme sırasında verileri ve sonuç yollarını el ile izlemek yerine, veri kaynaklarınızı, girdilerlerinizi ve çıkışları açıkça adlandırmak ve sürümüne eklemek için işlem hatları SDK 'sını kullanın. Ayrıca, daha fazla üretkenlik için betikleri ve verileri ayrı olarak yönetebilirsiniz.|
|**İş**|İşlem hatları, veri bilimcilerinin makine öğrenimi tasarım sürecinin tüm alanlarında işbirliği yapmasına olanak sağlarken işlem hattı adımlarında eşzamanlı olarak çalışabiliyor.|

## <a name="the-python-sdk-for-pipelines"></a>İşlem hatları için Python SDK'sı

Tercih ettiğiniz tümleşik geliştirme ortamında (IDE) veya Jupyıter not defterlerinde ML işlem hatlarınızı oluşturmak için [Python SDK 'Sını kullanın](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) . Azure Machine Learning SDK'sı, sıralama ve hiçbir veri bağımlılık mevcut olduğunda işlem hatlarınızı adımları paralelleştirmek için zorunlu yapıları sunar. 

Bildirim temelli veriler bağımlılıkları kullanarak, görevlerinizi en iyi duruma getirebilirsiniz. SDK, veri aktarımı ve model yayımlama gibi ortak görevler için önceden oluşturulmuş modüller çerçevesini içerir. İşlem hatları arasında yeniden kullanılabilir özel adımları uygulayarak, kendi kurallarınızı modellemek için çerçeveyi genişletebilirsiniz. İşlem hedeflerini ve depolama kaynaklarını doğrudan SDK 'dan da yönetebilirsiniz.

İşlem hatlarınızı şablon olarak kaydedin ve toplu Puanlama veya yeniden eğitim işleri için bir REST uç noktasına dağıtın.

Azure Machine Learning işlem hatları için iki Python paketi vardır: [azureml-işlem hattı-çekirdek](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) ve [azureml-işlem hattı-adımlar](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py). Hızlıca kullanmaya başlamak için, önceden oluşturulmuş modüllerden birini kullanın:

* [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep) ile bir adımda Python betiği çalıştırma
* [Datatransferstep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.datatransferstep) ile depolama seçenekleri arasında veri aktarımı
* Oto [mlstep](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlstep) Ile bir oto ml işlem hattı adımı oluşturma

## <a name="next-steps"></a>Sonraki adımlar

+ Bilgi edinmek için nasıl [ilk işlem hattınızı oluşturma](how-to-create-your-first-pipeline.md).

+ [Büyük verilerde toplu tahmine dayalı tahminleri nasıl çalıştıracağınızı](tutorial-pipeline-batch-scoring-classification.md)öğrenin.

+ İşlem [hatları Için SDK başvuru belgelerine](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py)bakın.

+ İşlem [hatları Azure Machine Learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines)gösteren örnek bir Jupyter Not defteri deneyin. [Bu hizmeti araştırmak için not defterlerini çalıştırmayı](samples-notebooks.md)öğrenin.
