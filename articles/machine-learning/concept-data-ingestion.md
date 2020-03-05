---
title: Veri alma seçenekleri
titleSuffix: Azure Machine Learning
description: Makine öğrenimi modellerinizi eğitmek için veri alma seçenekleri hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: nibaccam
author: nibaccam
ms.author: nibaccam
ms.date: 02/26/2020
ms.openlocfilehash: 35c133a4d9d95dc6e942d5b8c3e8d21debb9493b
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78268557"
---
# <a name="data-ingestion-in-azure-machine-learning"></a>Azure Machine Learning veri alımı

Bu makalede, Azure Machine Learning ile kullanılabilen aşağıdaki veri alma seçeneklerinin avantajlarını ve dezavantajlarını öğrenirsiniz. 

1. [Azure Data Factory](#use-azure-data-factory) işlem hatları
2. [Azure Machine Learning Python SDK'sı](#use-the-python-sdk)

Veri alımı, yapılandırılmamış verilerin bir veya daha fazla kaynaktan ayıklanması ve ardından eğitim makine öğrenimi modelleri için hazırlandığı işlemdir. Ayrıca, özellikle el ile yapıldıysa ve birden çok kaynaktan büyük miktarda veriniz varsa, bu da yoğun zaman alabilir. Bu çabayı otomatik hale getirmek kaynakları boşaltır ve modellerinizin en son ve uygun verileri kullanmasını sağlar.

Verileri ayıklamak, yüklemek ve dönüştürmek için özel olarak oluşturulduğu için başlangıçta Azure Data Factory (ADF) kullanarak değerlendirmeniz önerilir. ADF 'yi kullanarak gereksinimlerinizi karşılamadıysanız, özel bir kod çözümü geliştirmek için Python SDK 'sını kullanabilir veya gereksinimlerinize uyan genel veri alma iş akışını oluşturmak için ADF 'yi ve Python SDK 'sını kullanabilirsiniz.

## <a name="use-azure-data-factory"></a>Azure Data Factory'yi kullanma

[Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) veri alma işlem hatları için veri kaynağı izleme ve Tetikleyicileri için yerel destek sunar.  

Aşağıdaki tabloda, veri alma iş akışlarınız için Azure Data Factory kullanmanın uzmanları ve dezavantajları özetlenmektedir.

|Uzmanları|Simgeler
---|---
Verileri ayıklamak, yüklemek ve dönüştürmek için özel olarak oluşturulmuştur.|Şu anda sınırlı sayıda Azure Data Factory işlem hattı görevi sunmaktadır 
Veri hareketini ve dönüştürmeleri ölçekleyerek düzenlemek için veri odaklı iş akışları oluşturmanıza olanak tanır.|Yapı ve bakım açısından pahalıdır. Daha fazla bilgi için Azure Data Factory [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/) bakın.
[Azure Databricks](https://docs.microsoft.com/azure/data-factory/transform-data-using-databricks-notebook) ve [Azure Işlevleri](https://docs.microsoft.com/azure/data-factory/control-flow-azure-function-activity) gibi çeşitli Azure araçlarıyla tümleşiktir | Betikleri yerel olarak çalıştırmaz, bunun yerine betik çalıştırmaları için ayrı bir işlem kullanır 
Veri kaynağı tarafından tetiklenen veri alımını yerel olarak destekler| 
Veri hazırlama ve model eğitimi işlemi birbirinden farklıdır.|
Azure Data Factory veri akışları için gömülü Data kökenini özelliği|
Betik olmayan yaklaşımlar için düşük kod deneyimi [Kullanıcı arabirimi](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal) sağlar |

Bu adımlar ve aşağıdaki diyagramda Azure Data Factory veri alımı iş akışı gösterilmektedir.

1. Verileri kaynaklarından çekin
1. Verileri Azure Machine Learning için veri depolama işlevi gören bir çıktı blobu kapsayıcısına dönüştürün ve kaydedin
1. Hazırlanan veriler depolandığında Azure Data Factory işlem hattı, model eğitimi için hazırlanan verileri alan bir eğitim Machine Learning işlem hattı çağırır


    ![ADF veri alımı](media/concept-data-ingestion/data-ingest-option-one.svg)

## <a name="use-the-python-sdk"></a>Python SDK 'sını kullanma 

[Python SDK](https://docs.microsoft.com/python/api/overview/azure/ml)ile veri alma görevlerini Azure Machine Learning bir işlem [hattı](how-to-create-your-first-pipeline.md) adımına ekleyebilirsiniz.

Aşağıdaki tabloda, veri alma görevleri için SDK 'Yı ve bir ML işlem hattı adımını kullanmaya yönelik profesyonelleri ve Con özetlenmektedir.

Uzmanları| Simgeler
---|---
Kendi Python betiklerinizi yapılandırma | Veri kaynağı değişikliği tetiklemesini yerel olarak desteklemez. Mantıksal uygulama veya Azure Işlev uygulamaları gerektirir
Her model eğitimi yürütmesinin parçası olarak veri hazırlama|Veri alma betiği oluşturmak için geliştirme becerileri gerektirir
[Azure Machine Learning işlem](concept-compute-target.md#azure-machine-learning-compute-managed) dahil olmak üzere çeşitli işlem hedeflerinde veri hazırlama betiklerini destekler |Alma mekanizmasını oluşturmak için bir kullanıcı arabirimi sağlamaz

Aşağıdaki diyagramda Azure Machine Learning işlem hattı iki adımdan oluşur: veri alımı ve model eğitimi. Veri alma adımı, Python kitaplıkları ve Python SDK kullanılarak gerçekleştirilebilen, yerel/Web kaynaklarından veri ayıklama ve eksik değer imputation gibi temel veri dönüştürmeleri gibi görevleri kapsar. Daha sonra eğitim adımı, makine öğrenimi modelinizi eğmek için hazırlanan verileri eğitim betiğe giriş olarak kullanır. 

![Azure işlem hattı + SDK veri alımı](media/concept-data-ingestion/data-ingest-option-two.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Pipelines](how-to-cicd-data-ingestion.md)ile veri alma işlem hatlarınızın geliştirme yaşam döngüsünü otomatik hale getirmeyi ve yönetmeyi öğrenin.