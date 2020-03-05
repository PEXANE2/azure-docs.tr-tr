---
title: Azure Data Factory ile veri alımı
titleSuffix: Azure Machine Learning
description: Azure Data Factory bir veri alma işlem hattı oluşturmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: iefedore
author: eedorenko
manager: davete
ms.reviewer: larryfr
ms.date: 03/01/2020
ms.openlocfilehash: 5b4ed40048aab815397c9726098880b2125b732c
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78274794"
---
# <a name="data-ingestion-with-azure-data-factory"></a>Azure Data Factory ile veri alımı

Bu makalede, Azure Data Factory (ADF) ile veri alma işlem hattı oluşturmayı öğreneceksiniz. Bu işlem hattı, Azure Machine Learning ile kullanım için verileri almak üzere kullanılır. Azure Data Factory, verileri kolayca ayıklamanızı, dönüştürmenizi ve yüklemeyi (ETL) sağlar. Veriler dönüştürüldükten ve depolama alanına yüklendikten sonra, makine öğrenimi modellerinizi eğitebilmeniz için kullanılabilir.

Basit veri dönüştürme, yerel ADF etkinlikleri ve [veri akışı](https://docs.microsoft.com/azure/data-factory/control-flow-execute-data-flow-activity)gibi gereçlerle işlenebilir. Daha karmaşık senaryolara geldiğinde veriler bazı özel kodla işlenebilir. Örneğin, Python veya R kodu.

Alma sırasında verileri dönüştürmek için Azure Data Factory kullanmanın birçok yaygın tekniği vardır. Her bir tekniğe, belirli bir kullanım örneğine uygun olup olmadığını belirten olumlu ve olumsuz yönleri vardır:

| Teknik | Uzmanları | Simgeler |
| ----- | ----- | ----- |
| ADF + Azure Işlevleri | Düşük gecikme süresi, sunucusuz işlem</br>Durum bilgisi olan işlevler</br>Yeniden kullanılabilir işlevler | Yalnızca kısa süre çalışan işleme için iyi |
| ADF + özel bileşen | Büyük ölçekli paralel bilgi işlem</br>Ağır algoritmalara uygun | Kodu çalıştırılabilire sarmalama</br>Bağımlılıkları ve GÇ işleme karmaşıklığı |
| ADF + Azure Databricks Not defteri | Apache Spark</br>Yerel Python ortamı | Pahalı olabilir</br>Başlangıçta küme oluşturulması zaman alır ve gecikme süresi ekler

## <a name="adf-with-azure-functions"></a>Azure işlevleri ile ADF

![ADF-işlevi](media/how-to-data-ingest-adf/adf-function.png)

Azure Işlevleri, uygulama altyapısı hakkında endişelenmeden küçük kod parçalarını (işlevler) çalıştırmanıza olanak sağlar. Bu seçenekte, veriler bir Azure Işlevine Sarmalanan özel Python koduyla işlenir. 

İşlev, [ADF Azure işlevi etkinliğiyle](https://docs.microsoft.com/azure/data-factory/control-flow-azure-function-activity)çağrılır. Bu yaklaşım, hafif veri dönüştürmeleri için iyi bir seçenektir. 

* Ları
    * Veriler, görece düşük gecikme süresine sahip sunucusuz bir işlem üzerinde işlenir
    * ADF işlem hattı, gelişmiş bir veri dönüştürme akışı uygulayabilen [dayanıklı bir Azure işlevi](/azure/azure-functions/durable/durable-functions-overview) çağırabilir 
    * Veri dönüşümünün ayrıntıları, diğer konumlardan yeniden kullanılabilen ve çağrılabilen Azure Işlevi tarafından soyutlanmıştır
* Larını
    * ADF ile kullanılmadan önce Azure Işlevlerinin oluşturulması gerekir
    * Azure Işlevleri yalnızca kısa süre çalışan veri işleme için uygundur

## <a name="adf-with-custom-component-activity"></a>Özel bileşen etkinliği ile ADF

![ADF-customcomponent](media/how-to-data-ingest-adf/adf-customcomponent.png)

Bu seçenekte, veriler yürütülebilir dosyaya sarmalanmış özel Python kodu ile işlenir. [ADF özel bileşeni etkinliğiyle](https://docs.microsoft.com/azure/data-factory/transform-data-using-dotnet-custom-activity)çağrılır. Bu yaklaşım, önceki tekniğinden daha büyük veriler için daha iyi bir uyum.

* Ları
    * Veriler, büyük ölçekli paralel ve yüksek performanslı bilgi işlem sağlayan [Azure Batch](https://docs.microsoft.com/azure/batch/batch-technical-overview) havuzunda işlenir
    * Ağır algoritmaları çalıştırmak ve önemli miktarda veriyi işlemek için kullanılabilir
* Larını
    * ADF ile kullanılmadan önce Azure Batch havuzunun oluşturulması gerekir
    * Python kodunu yürütülebilir dosyaya sarmalama ile ilgili mühendisler. Bağımlılıkları ve giriş/çıkış parametrelerini işleme karmaşıklığı

## <a name="adf-with-azure-databricks-python-notebook"></a>Azure Databricks Python Not defteri ile ADF

![ADF-databricks](media/how-to-data-ingest-adf/adf-databricks.png)

[Azure Databricks](https://azure.microsoft.com/services/databricks/) , Microsoft bulutundaki Apache Spark tabanlı bir analiz platformudur.

Bu teknikte, veri dönüştürme bir Azure Databricks kümesinde çalışan bir [Python Not defteri](https://docs.microsoft.com/azure/data-factory/transform-data-using-databricks-notebook)tarafından gerçekleştirilir. Bu, Azure Databricks bir hizmetin tam gücünden yararlanan en yaygın yaklaşım olabilir. Ölçeklenebilir veri işleme için ölçekli olarak tasarlanmıştır.

* Ları
    * Veriler, Apache Spark ortamı tarafından yedeklenen en güçlü veri işleme Azure hizmetinde dönüştürülür
    * TensorFlow, PyTorch ve scikit-öğrenme dahil olmak üzere veri bilimi çerçeveleri ve kitaplıkları ile Python 'un yerel desteği
    * Python kodunu işlevlere veya yürütülebilir modüllere sarmasına gerek yoktur. Kod olduğu gibi çalışıyor.
* Larını
    * ADF ile kullanılmadan önce Azure Databricks altyapısı oluşturulmalıdır
    * Azure Databricks yapılandırmasına bağlı olarak pahalı olabilir
    * "Soğuk" modundan işlem kümelerinin dönme, çözüme yüksek gecikme süresi getiren bir süre sürer 
    

## <a name="consuming-data-in-azure-machine-learning-pipelines"></a>Azure Machine Learning işlem hatlarında veri kullanma

![AML-veri kümesi](media/how-to-data-ingest-adf/aml-dataset.png)

ADF işlem hattındaki dönüştürülmüş veriler veri deposuna (Azure Blob gibi) kaydedilir. Azure Machine Learning, veri [depoları](https://docs.microsoft.com/azure/machine-learning/how-to-access-data#create-and-register-datastores) ve veri [kümelerini](https://docs.microsoft.com/azure/machine-learning/how-to-create-register-datasets)kullanarak bu verilere erişebilir.

ADF işlem hattı her çalıştığında, veriler depolamada farklı bir konuma kaydedilir. Konumu Azure Machine Learning geçirmek için, ADF işlem hattı bir Azure Machine Learning işlem hattını çağırır. ML ardışık düzeni çağrılırken, veri konumu ve çalıştırma KIMLIĞI parametre olarak gönderilir. ML işlem hattı daha sonra veri konumunu kullanarak bir veri deposu/veri kümesi oluşturabilir. 

> [!TIP]
> Veri kümeleri [sürüm oluşturmayı destekler](https://docs.microsoft.com/azure/machine-learning/how-to-version-track-datasets), bu nedenle ml ARDıŞıK düzeni ADF işlem hattındaki en son verileri gösteren yeni bir veri kümesinin yeni bir sürümünü kaydedebilir.

Veriler bir veri deposu veya veri kümesi aracılığıyla erişilebilir olduktan sonra, ML modelini eğitebilmeniz için bunu kullanabilirsiniz. Eğitim süreci, ADF 'den çağrılan aynı ML ardışık düzeninin bir parçası olabilir. Ya da bir Jupyter not defterinde deneme gibi ayrı bir işlem olabilir.

Veri kümeleri sürümü oluşturmayı desteklediği ve işlem hattından çalıştırılan her çalıştırma yeni bir sürüm oluşturduğundan, bir modeli eğitmek için hangi verilerin sürümünün kullanıldığını anlamak kolaydır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Factory bir Databricks Not defteri çalıştırma](https://docs.microsoft.com/azure/data-factory/transform-data-using-databricks-notebook)
* [Azure depolama hizmetlerindeki verilere erişme](https://docs.microsoft.com/azure/machine-learning/how-to-access-data#create-and-register-datastores)
* [Azure Machine Learning veri kümeleri ile modelleri eğitme](https://docs.microsoft.com/azure/machine-learning/how-to-train-with-datasets)
* [Veri alma işlem hattı için DevOps](https://docs.microsoft.com/azure/machine-learning/how-to-cicd-data-ingestion)

