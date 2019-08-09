---
title: 'İşlem hatları: makine öğrenimi iş akışlarını iyileştirme'
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
ms.openlocfilehash: a9965dbbca939f566048312af921061a188ee50d
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68884234"
---
# <a name="build-reusable-ml-pipelines-in-azure-machine-learning-service"></a>Azure Machine Learning hizmetinde yeniden kullanılabilir ML işlem hatları oluşturun

Bu makalede, Python için Azure Machine Learning SDK ile oluşturabileceğiniz makine öğrenimi ardışık düzenleri ve işlem hatları kullanmanın avantajları hakkında bilgi edinin.

## <a name="what-are-machine-learning-pipelines"></a>Machine learning işlem hatlarını nelerdir?

Machine learning (ML) işlem hatları, veri bilimcileri, veri mühendisleri ve BT uzmanları kullanarak içinde yer alan adımların üzerinde işbirliği yapabilirsiniz:
+ Normalizations ve dönüştürmeler gibi veri hazırlama
+ Modeli eğitimi
+ Modeli değerlendirme
+ Dağıtım

Aşağıdaki diyagramda örnek bir işlem hattı işlemi gösterilmektedir:

![Azure Machine Learning hizmetinde makine öğrenimi ardışık düzenleri](./media/concept-ml-pipelines/pipeline-flow.png)

<a name="compare"></a>
### <a name="which-azure-pipeline-technology-should-i-use"></a>Hangi Azure ardışık düzen teknolojisini kullanmalıyım?

Azure bulutu, her biri farklı bir amaca sahip birkaç farklı işlem hattı sağlar. Aşağıdaki tabloda, farklı işlem hatları ve bunların kullanıldıkları özellikler listelenmiştir:

| İşlem hattı | Ne yapar? | Kurallı Kanal |
| ---- | ---- | ---- |
| Azure Machine Learning işlem hatları | Makine öğrenimi senaryolarınız için şablon olarak kullanılabilen yeniden kullanılabilir makine öğrenimi iş akışlarını tanımlar. | Veri > modeli |
| [Azure Data Factory işlem hatları](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) | Bir görevi gerçekleştirmek için gereken veri taşıma, dönüştürme ve denetim etkinliklerini gruplandırır.  | Veri > verileri |
| [Azure işlem hatları](https://azure.microsoft.com/services/devops/pipelines/) | Uygulamanızın herhangi bir platforma/buluta yönelik sürekli tümleştirme ve teslim  | Kod-> uygulaması/hizmeti |

## <a name="why-build-pipelines-with-azure-machine-learning"></a>Neden Azure Machine Learning ile işlem hatları oluşturabilir?

Aynı zamanda tek tek işlem hattı çalıştırmalarını göndermek ve izlemek için [Python için Azure Machine Learning SDK 'sını](#the-python-sdk-for-pipelines) kullanabilirsiniz.

İşlem hattı ile akışınızı Basitlik, hız, taşınabilirlik ve yeniden iyileştirebilirsiniz. Azure Machine Learning ile işlem hatları oluştururken, altyapı ve otomasyon yerine uzmanınıza ve makine öğrenmenize odaklanırsınız.

İşlem hatları, işlem hattının farklı hesaplama birimleri olan birden çok **adımdan**oluşturulur. Her adım bağımsız olarak çalıştırılabilir ve yalıtılmış işlem kaynakları kullanabilir. Bu, birden çok veri bilimcilerinin aynı işlem hattı üzerinde, yoğun bilgi işlem kaynakları olmadan aynı anda çalışmasına olanak tanır ve ayrıca her adım için farklı işlem türleri/boyutları kullanmayı kolaylaştırır.

İşlem hattı tasarlandıktan sonra, işlem hattının eğitim döngüsü etrafında genellikle daha fazla ince ayar yapılır. Bir işlem hattını yeniden çalıştırdığınızda, çalıştırma, güncelleştirilmiş bir eğitim betiği gibi yeniden çalıştırılması gereken ayrı adımlara atlar ve nelerin değiştirilmediğini atlar. Aynı paradigma adımının yürütülmesi için kullanılan aynı komut için geçerlidir. Bu işlevsellik, temel alınan veriler değişmemişse veri alma ve dönüştürme gibi maliyetli ve zaman uyumlu adımların çalıştırılmasına engel olmaya yardımcı olur.

Azure Machine Learning, işlem hattınızdaki her adım için PyTorch veya TensorFlow gibi çeşitli araç setleri ve çerçeveler kullanabilirsiniz. Ara verilerinizin aşağı akış işlem hedefleri ile kolayca paylaşılabilmesi için, kullandığınız çeşitli [işlem hedefleri](concept-azure-machine-learning-architecture.md) arasında Azure koordinatları.

Yapabilecekleriniz [Ölçümleriyle için işlem hattı denemelerinizi](https://docs.microsoft.com/azure/machine-learning/service/how-to-track-experiments) doğrudan Azure portalında. İşlem hattı yayımlandıktan sonra, herhangi bir platform veya yığından işlem hattını yeniden çalıştırmanıza olanak sağlayan bir REST uç noktası yapılandırabilirsiniz.

## <a name="key-advantages"></a>Başlıca avantajları

Machine Learning iş akışlarınız için işlem hatlarını kullanmanın temel avantajları şunlardır:

|Önemli bir avantajı|Açıklama|
|:-------:|-----------|
|**Katılımsız&nbsp;çalıştırır**|Adımları güvenilir ve katılımsız bir şekilde paralel veya sırayla çalışacak şekilde zamanlayın. Veri hazırlama ve modelleme son günler veya haftadır ve işlem hatları, işlem çalışırken diğer görevlere odaklanabilmenize olanak tanır. |
|**Heterojen işlem**|Heterojen ve ölçeklenebilir işlem kaynakları ve depolama konumları genelinde güvenilir bir şekilde koordine edilen birden çok işlem hattı kullanın. HDInsight, GPU veri bilimi VM 'Leri ve Databricks gibi farklı işlem hedeflerinde tek tek ardışık düzen adımlarını çalıştırın. Bu, kullanılabilir işlem seçeneklerinin verimli bir şekilde kullanılmasını sağlar.|
|**Yeniden kullanılırlığı**|Yeniden eğitme ve Batch Puanlama gibi belirli senaryolar için ardışık düzen şablonları oluşturun. Dış sistemlerden yayınlanan işlem hatlarını basit REST çağrıları aracılığıyla tetikleyin.|
|**İzleme ve sürüm oluşturma**|Yineleme sırasında verileri ve sonuç yollarını el ile izlemek yerine, veri kaynaklarınızı, girdilerlerinizi ve çıkışları açıkça adlandırmak ve sürümüne eklemek için işlem hatları SDK 'sını kullanın. Ayrıca, daha fazla üretkenlik için betikleri ve verileri ayrı olarak yönetebilirsiniz.|
|**İş**|İşlem hatları, veri bilimcilerinin makine öğrenimi tasarım sürecinin tüm alanlarında işbirliği yapmasına olanak sağlarken işlem hattı adımlarında eşzamanlı olarak çalışabiliyor.|

## <a name="the-python-sdk-for-pipelines"></a>İşlem hatları için Python SDK'sı

ML işlem hatlarınızı oluşturmak için Python kullanın. Azure Machine Learning SDK'sı, sıralama ve hiçbir veri bağımlılık mevcut olduğunda işlem hatlarınızı adımları paralelleştirmek için zorunlu yapıları sunar. Jupyıter not defterlerinde veya başka bir tercih edilen IDE 'de onunla etkileşime geçebilirsiniz.

Bildirim temelli veriler bağımlılıkları kullanarak, görevlerinizi en iyi duruma getirebilirsiniz. SDK, veri aktarımı ve model yayımlama gibi ortak görevler için önceden oluşturulmuş modüller çerçevesini içerir. İşlem hatları arasında yeniden kullanılabilir olan özel adımları uygulayarak kendi kurallarınızı modellemek için çerçeveyi genişletebilirsiniz. İşlem hedeflerini ve depolama kaynaklarını doğrudan SDK 'dan da yönetebilirsiniz.

İşlem hatlarını şablon olarak kaydedebilir ve toplu Puanlama veya yeniden eğitim işleri zamanlayabilmeniz için bunları bir REST uç noktasına dağıtabilirsiniz.

Kendinizinkini nasıl derulacağını görmek için, sonraki bölümde işlem hatları ve Not defteri [Için Python SDK başvurusu belgelerine](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) bakın.

## <a name="example-notebooks"></a>Örnek Not Defterleri

Azure Machine Learning işlem hatlarında aşağıdaki not defterlerini göstermektedir: [how-to-use-azureml/machine-learning-pipelines](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines).

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [ilk işlem hattınızı oluşturma](how-to-create-your-first-pipeline.md).
