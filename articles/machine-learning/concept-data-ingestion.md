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
ms.openlocfilehash: 6b1c671d2079c7d8ab59e9afe981ccef3f58ef27
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79086891"
---
# <a name="data-ingestion-in-azure-machine-learning"></a>Azure Machine Learning'de veri alımı

Bu makalede, Azure Machine Learning ile kullanılabilen aşağıdaki veri alma seçeneklerinin artılarını ve eksilerini öğreneceksiniz. 

1. [Azure Veri Fabrikası](#use-azure-data-factory) ardışık hatları
2. [Azure Machine Öğrenme Python SDK](#use-the-python-sdk)

Veri alımı, yapılandırılmamış verilerin bir veya birden fazla kaynaktan ayıklandığı ve daha sonra makine öğrenimi modellerini eğitmek için hazırlandığı süreçtir. Ayrıca, özellikle el ile yapılıyorsa ve birden çok kaynaktan büyük miktarda veriniz varsa, zaman yoğundur. Bu çabayı otomatikleştirmek kaynakları serbest eder ve modellerinizin en yeni ve uygulanabilir verileri kullanmasını sağlar.

Azure Veri Fabrikası (ADF) verileri ayıklamak, yüklemek ve dönüştürmek için özel olarak oluşturulmuştür, ancak Python SDK temel veri alma görevleri için özel bir kod çözümü geliştirebilmenizi sağlar. Hiçbiri ihtiyacınız değilse, ihtiyaçlarınızı karşılayan genel bir veri akışı iş akışı oluşturmak için ADF ve Python SDK'yı birlikte kullanabilirsiniz. 

## <a name="use-azure-data-factory"></a>Azure Data Factory'yi kullanma

[Azure Veri Fabrikası,](https://docs.microsoft.com/azure/data-factory/introduction) veri kaynağı izleme ve veri alma ardışık hatları için tetikleyiciler için yerel destek sunar.  

Aşağıdaki tablo, veri alma iş akışlarınız için Azure Veri Fabrikası'nı kullanmanın artılarını ve eksilerini özetleyin.

|Artıları|Simgeler
---|---
Verileri ayıklamak, yüklemek ve dönüştürmek için özel olarak oluşturulmuştür.|Şu anda sınırlı bir Azure Veri Fabrikası ardışık iş hattı görevleri kümesi sunar 
Veri hareketini ve dönüşümleri ölçekte düzenlemek için veri tabanlı iş akışları oluşturmanıza olanak tanır.|İnşa etmek ve bakımı pahalı. Daha fazla bilgi için Azure Veri Fabrikası'nın [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/) bakın.
[Azure Veri Tuğlaları](https://docs.microsoft.com/azure/data-factory/transform-data-using-databricks-notebook) ve Azure [Fonksiyonları](https://docs.microsoft.com/azure/data-factory/control-flow-azure-function-activity) gibi çeşitli Azure araçlarıyla tümleşik | Yerel olarak komut dosyaları çalıştırmıyor, bunun yerine komut dosyası çalıştırmaları için ayrı bir işlem 
Yerel olarak veri kaynağı tetiklenen veri alımını destekler| 
Veri hazırlama ve model eğitim süreçleri ayrıdır.|
Azure Veri Fabrikası veri akışları için gömülü veri soyu özelliği|
Komut dosyası olmayan yaklaşımlar için düşük kod deneyimi [kullanıcı arabirimi](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal) sağlar |

Bu adımlar ve aşağıdaki diyagram, Azure Veri Fabrikası'nın veri alma iş akışını göstermektedir.

1. Kaynaklarının verilerini çekme
1. Verileri Azure Machine Learning için veri depolama görevi gören bir çıktı blob kapsayıcısına dönüştürün ve kaydedin
1. Hazırlanan veriler depolanırken, Azure Veri Fabrikası boru hattı, model eğitimi için hazırlanan verileri alan bir eğitim Machine Learning ardışık hattını çağırır


    ![ADF Veri alımı](media/concept-data-ingestion/data-ingest-option-one.svg)
    
[Azure Veri Fabrikası](how-to-data-ingest-adf.md)ile Makine Öğrenimi için bir veri alma ardışık hattı oluşturmayı öğrenin.

## <a name="use-the-python-sdk"></a>Python SDK'yı kullan 

Python [SDK](https://docs.microsoft.com/python/api/overview/azure/ml)ile, veri alma görevlerini bir [Azure Machine Learning ardışık aşamaya](how-to-create-your-first-pipeline.md) dahil edebilirsiniz.

Aşağıdaki tabloda, veri alma görevleri için SDK ve ML ardışık lıklar adımını kullanmanın artılarını ve artılarını özetler.

Artıları| Simgeler
---|---
Kendi Python komut dosyalarınızı yapılandırın | Yerel olarak veri kaynağı değişikliğini desteklemez. Mantık Uygulaması veya Azure İşlevi uygulamaları gerektirir
Her model eğitim yürütmenin bir parçası olarak veri hazırlama|Veri alma komut dosyası oluşturmak için geliştirme becerileri gerektirir
Azure Machine Learning bilgi işlem dahil olmak üzere çeşitli bilgi işlem hedeflerinde veri hazırlama komut [dosyalarını](concept-compute-target.md#azure-machine-learning-compute-managed) destekler |Yutma mekanizması oluşturmak için bir kullanıcı arabirimi sağlamaz

Aşağıdaki diyagramda, Azure Machine Learning ardışık alanı iki adımdan oluşur: veri alma ve model eğitimi. Veri alma adımı, Python kitaplıkları ve Python SDK kullanılarak gerçeklenebilen yerel/web kaynaklarından veri ayıklama ve eksik değer imputasyonu gibi temel veri dönüşümleri gibi görevleri kapsar. Eğitim adımı daha sonra makine öğrenimi modelinizi eğitmek için hazırlanan verileri eğitim komut dosyanıza girdi olarak kullanır. 

![Azure ardışık alma + SDK veri alımı](media/concept-data-ingestion/data-ingest-option-two.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Fabrikası](how-to-data-ingest-adf.md) ile Makine Öğrenimi için veri alma ardışık bir boru hattı oluşturmayı öğrenin
* [Azure Pipelines](how-to-cicd-data-ingestion.md)ile veri alma ardışık hatlarınızın geliştirme yaşam döngülerini nasıl otomatikleştirip yönetebilirsiniz öğrenin.
