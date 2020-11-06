---
title: '& Azure Databricks, oto ml ile geliştirin'
titleSuffix: Azure Machine Learning
description: Azure Machine Learning ve Azure Databricks bir geliştirme ortamı ayarlamayı öğrenin. Databricks ve Databricks için Azure ML SDK 'larını, oto ml ile birlikte kullanın.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.date: 10/21/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python
ms.openlocfilehash: ef8ee7718aabb443fda6cd7b276ee53472261913
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93424546"
---
# <a name="set-up-a-development-environment-with-azure-databricks-and-automl-in-azure-machine-learning"></a>Azure Machine Learning içinde Azure Databricks ve oto ml ile bir geliştirme ortamı ayarlama 

Azure Databricks ve otomatik ML kullanan Azure Machine Learning bir geliştirme ortamını nasıl yapılandıracağınızı öğrenin.

Azure Databricks, Azure bulutundaki ölçeklenebilir Apache Spark platformunda büyük ölçekli yoğun makine öğrenimi iş akışlarını çalıştırmak için idealdir. CPU veya GPU tabanlı işlem kümesi içeren, birlikte çalışan bir not defteri tabanlı ortam sağlar.

Diğer makine öğrenimi geliştirme ortamları hakkında bilgi için bkz. [Python geliştirme ortamı ayarlama](how-to-configure-environment.md).


## <a name="prerequisite"></a>Önkoşul

Azure Machine Learning çalışma alanı. Bir tane yoksa, [Azure Portal](how-to-manage-workspace.md), [Azure CLI](how-to-manage-workspace-cli.md#create-a-workspace)ve [Azure Resource Manager şablonları](how-to-create-workspace-template.md)aracılığıyla bir Azure Machine Learning çalışma alanı oluşturabilirsiniz.


## <a name="azure-databricks-with-azure-machine-learning-and-automl"></a>Azure Machine Learning ve oto ml ile Azure Databricks

Azure Databricks, Azure Machine Learning ve onun oto ml özellikleri ile tümleşir. 

Azure Databricks kullanabilirsiniz:

+ Spark MLlib kullanarak bir modeli eğitme ve modeli ACG/AKS 'e dağıtma.
+ Azure ML SDK kullanarak [otomatik makine öğrenimi](concept-automated-ml.md) özellikleri ile.
+ Bir [Azure Machine Learning](concept-ml-pipelines.md)işlem hattından işlem hedefi olarak.

## <a name="set-up-a-databricks-cluster"></a>Databricks kümesi ayarlama

[Databricks kümesi](/azure/databricks/scenarios/quickstart-create-databricks-workspace-portal)oluşturun. Bazı ayarlar yalnızca Databricks 'te otomatik makine öğrenimi için SDK 'Yı yüklerseniz geçerlidir.

**Kümenin oluşturulması birkaç dakika sürer.**

Şu ayarları kullanın:

| Ayar |Şunlara uygulanır| Değer |
|----|---|---|
| Küme Adı |Her| yourclustername |
| Databricks Runtime sürümü |Her| Çalışma zamanı 7,1 (Scala 2,21, Spark 3.0.0)-ML değil|
| Python sürümü |Her| 3 |
| Çalışan türü <br>(en fazla eşzamanlı yineleme sayısını belirler) |Otomatikleştirilmiş ML<br>yalnızca| Bellek için iyileştirilmiş VM tercih edilen |
| Çalışanlarınız |Her| 2 veya üzeri |
| Otomatik ölçeklendirmeyi etkinleştir |Otomatikleştirilmiş ML<br>yalnızca| Kutunun |

Devam etmeden önce küme çalışmaya kadar bekleyin.

## <a name="add-the-azure-ml-sdk-to-databricks"></a>Azure ML SDK 'sını Databricks 'e ekleme

Küme çalışmaya başladıktan sonra uygun Azure Machine Learning SDK paketini kümenize eklemek için [bir kitaplık oluşturun](https://docs.databricks.com/user-guide/libraries.html#create-a-library) . 

Otomatik ML 'yi kullanmak için, [Otomatik ml Ile Azure ML SDK 'Sı ekleme](#add-the-azure-ml-sdk-with-automl-to-databricks)' yi atlayın.


1. Kitaplığı depolamak istediğiniz geçerli çalışma alanı klasörüne sağ tıklayın. Kitaplık **Oluştur** ' u seçin  >  **Library**.
    
    > [!TIP]
    > Eski bir SDK sürümünüz varsa, kümenin yüklü kitaplıklarından seçimini kaldırın ve çöp kutusuna geçiş yapın. Yeni SDK sürümünü yükleyip kümeyi yeniden başlatın. Yeniden başlatmadan sonra bir sorun varsa, kümenizi ayırın ve yeniden bağlayın.

1. Aşağıdaki seçeneği seçin (diğer SDK yüklemeleri desteklenmez)

   |SDK &nbsp; paketi &nbsp; ek özellikleri|Kaynak|Pypı &nbsp; adı&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
   |----|---|---|
   |Databricks için| Python Yumurg veya Pypı yükleme | azureml-SDK [databricks]|

   > [!WARNING]
   > Başka SDK ek özellikleri yüklenemez. Yalnızca [ `databricks` ] seçeneğini belirleyin.

   * **Tüm kümelere otomatik olarak ekle** ' yi seçmeyin.
   * Küme adınızın yanındaki  **Ekle** ' yi seçin.

1. Durum **iliştirilene** kadar, bu işlem birkaç dakika sürebilir.  Bu adım başarısız olursa:

   Kümenizi şu şekilde yeniden başlatmayı deneyin:
   1. Sol bölmede **kümeler** ' ı seçin.
   1. Tabloda, küme adınızı seçin.
   1. **Kitaplıklar** sekmesinde **Yeniden Başlat** ' ı seçin.

   Başarılı bir yüklemesi aşağıdaki gibi görünür: 

  ![Databricks için SDK Azure Machine Learning](./media/how-to-configure-environment/amlsdk-withoutautoml.jpg) 

## <a name="add-the-azure-ml-sdk-with-automl-to-databricks"></a>Databricks 'e, oto ml ile Azure ML SDK 'sını ekleyin
Küme Databricks Runtime 7,1 veya üzeri (ml *değil* ) ile oluşturulduysa, AML SDK 'sını yüklemek için not defterinizin ilk hücresinde aşağıdaki komutu çalıştırın.

```
%pip install --upgrade --force-reinstall -r https://aka.ms/automl_linux_requirements.txt
```
Databricks Runtime 7,0 ve daha düşük bir değer için, [init betiği](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks/automl/README.md)kullanarak Azure Machine Learning SDK 'sını yüklemelisiniz.

### <a name="automl-config-settings"></a>Oto ml yapılandırma ayarları

Azure Databricks kullanırken, oto ml yapılandırmasında aşağıdaki parametreleri ekleyin:

- ```max_concurrent_iterations``` , kümenizdeki çalışan düğümlerinin sayısına bağlıdır.
- ```spark_context=sc``` , varsayılan Spark bağlamını temel alır.

## <a name="ml-notebooks-that-work-with-azure-databricks"></a>Azure Databricks ile birlikte çalışan ML Not defterleri

Deneyin:
+ Birçok örnek Not defteri kullanılabilir, **ancak [Bu örnek Not defterleri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks) yalnızca Azure Databricks çalışır.**

+ Bu örnekleri doğrudan çalışma alanınızdan içeri aktarın. Aşağıya bakın: içeri aktarma ![ ](./media/how-to-configure-environment/azure-db-screenshot.png)
 ![ bölmesini seçin](./media/how-to-configure-environment/azure-db-import.png)

+ [Eğitim işlemi olarak Databricks ile bir işlem hattı oluşturmayı](how-to-create-your-first-pipeline.md)öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Machine Learning bir modeli, veri kümesi ile birlikte [eğitme](tutorial-train-models-with-aml.md) .
- [Python başvurusu için Azure Machine Learning SDK 'sına](/python/api/overview/azure/ml/intro?preserve-view=true&view=azure-ml-py)bakın.
