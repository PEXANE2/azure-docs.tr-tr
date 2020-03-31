---
title: Azure Veri Fabrikası ile veri alma
titleSuffix: Azure Machine Learning
description: Azure Veri Fabrikası ile veri alma ardışık bir denetim hattı oluşturmayı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78274794"
---
# <a name="data-ingestion-with-azure-data-factory"></a>Azure Veri Fabrikası ile veri alma

Bu makalede, Azure Veri Fabrikası (ADF) ile veri alma ardışık bir yapı oluşturmayı öğrenirsiniz. Bu ardışık işlem, Azure Machine Learning ile kullanılmak üzere veri yutmak için kullanılır. Azure Veri Fabrikası, (ETL) verilerini kolayca ayıklamanızı, dönüştürmenizi ve yüklemenizi sağlar. Veriler dönüştürüldükten ve depolama alanına yüklendikten sonra, makine öğrenimi modellerinizi eğitmek için kullanılabilir.

Basit veri dönüşümü yerel ADF etkinlikleri ve [veri akışı](https://docs.microsoft.com/azure/data-factory/control-flow-execute-data-flow-activity)gibi araçlarla işlenebilir. Daha karmaşık senaryolar söz konusu olduğunda, veriler bazı özel kodlarla işlenebilir. Örneğin, Python veya R kodu.

Yutma sırasında verileri dönüştürmek için Azure Veri Fabrikası'nı kullanmanın birkaç yaygın tekniği vardır. Her tekniğin, belirli bir kullanım örneği için uygun olup olmadığını belirleyen artıları ve eksileri vardır:

| Teknik | Artıları | Simgeler |
| ----- | ----- | ----- |
| ADF + Azure Fonksiyonları | Düşük gecikme, sunucusuz işlem</br>Devletli işlevler</br>Yeniden kullanılabilir fonksiyonlar | Sadece kısa çalışan işleme için iyi |
| ADF + özel bileşen | Büyük ölçekli paralel bilgi işlem</br>Ağır algoritmalar için uygundur | Kodu çalıştırılabilir bir şekilde sarma</br>Bağımlılıkları ve IO'ları işleme karmaşıklığı |
| ADF + Azure Databricks dizüstü bilgisayar | Apache Spark</br>Yerli Python ortamı | Pahalı olabilir</br>Kümeoluşturma başlangıçta zaman alır ve gecikme

## <a name="adf-with-azure-functions"></a>Azure işlevleri ne kadar adf

![adf fonksiyonu](media/how-to-data-ingest-adf/adf-function.png)

Azure İşlevleri, uygulama altyapısı konusunda endişelenmeden küçük kod parçalarını (işlevleri) çalıştırmanızı sağlar. Bu seçenekte, veriler Azure İşlevi'ne sarılmış özel Python koduyla işlenir. 

İşlev [ADF Azure İşlevi etkinliği](https://docs.microsoft.com/azure/data-factory/control-flow-azure-function-activity)ile çağrılır. Bu yaklaşım, hafif veri dönüşümleri için iyi bir seçenektir. 

* Profesyonel:
    * Veriler, nispeten düşük gecikme süreyle sunucusuz bir bilgi işlemde işlenir
    * ADF ardışık hattı, gelişmiş bir veri dönüştürme akışı uygulayabilecek dayanıklı bir [Azure İşlevi](/azure/azure-functions/durable/durable-functions-overview) çağırabilir 
    * Veri dönüştürmenin ayrıntıları, başka yerlerden yeniden kullanılabilen ve çağrılabilen Azure İşlevi tarafından soyutlanır
* Eksi -lerini:
    * Azure Fonksiyonları ADF ile kullanılmadan önce oluşturulmalıdır
    * Azure İşlevleri yalnızca kısa çalışan veri işleme için iyidir

## <a name="adf-with-custom-component-activity"></a>Özel Bileşen Etkinliği ile ADF

![adf-özel bileşen](media/how-to-data-ingest-adf/adf-customcomponent.png)

Bu seçenekte, veriler yürütülebilir olarak sarılmış özel Python koduyla işlenir. [Bir ADF Özel Bileşen etkinliği](https://docs.microsoft.com/azure/data-factory/transform-data-using-dotnet-custom-activity)ile çağrılır. Bu yaklaşım, büyük veriler için önceki tekniğe göre daha uygun.

* Profesyonel:
    * Veriler, büyük ölçekli paralel ve yüksek performanslı bilgi işlem sağlayan [Azure Toplu İşlem](https://docs.microsoft.com/azure/batch/batch-technical-overview) havuzunda işlenir
    * Ağır algoritmaları çalıştırmak ve önemli miktarda veriyi işlemek için kullanılabilir
* Eksi -lerini:
    * Azure Toplu İş havuzu ADF ile kullanılmadan önce oluşturulmalıdır
    * Python kodunun çalıştırılabilir olarak paketlemeyle ilgili mühendislik üzerinde. Kullanım bağımlılıklarının karmaşıklığı ve giriş/çıkış parametreleri

## <a name="adf-with-azure-databricks-python-notebook"></a>Azure Databricks Python dizüstü bilgisayar ile ADF

![adf-databricks](media/how-to-data-ingest-adf/adf-databricks.png)

[Azure Databricks,](https://azure.microsoft.com/services/databricks/) Microsoft bulutundaki Apache Spark tabanlı bir analiz platformudur.

Bu teknikte, veri dönüştürme, Azure Veri Tuğlaları kümesinde çalışan bir [Python dizüstü bilgisayarı](https://docs.microsoft.com/azure/data-factory/transform-data-using-databricks-notebook)tarafından gerçekleştirilir. Bu büyük olasılıkla, bir Azure Databricks hizmetinin tüm gücünden yararlanan en yaygın yaklaşımdır. Ölçekte dağıtılmış veri işleme için tasarlanmıştır.

* Profesyonel:
    * Veriler, Apache Spark ortamı tarafından yedeklenen en güçlü veri işleme Azure hizmetinde dönüştürülür
    * TensorFlow, PyTorch ve scikit-learn dahil olmak üzere veri bilimi çerçeveleri ve kütüphaneleri ile birlikte Python yerli desteği
    * Python kodunu işlevlere veya çalıştırılabilir modüllere sarmaya gerek yoktur. Kod olduğu gibi çalışır.
* Eksi -lerini:
    * Azure Databricks altyapısı ADF ile kullanılmadan önce oluşturulmalıdır
    * Azure Databricks yapılandırmasına bağlı olarak pahalı olabilir
    * "Soğuk" modundan işlem kümelerini döndürmek çözüme yüksek gecikme süresi getiren biraz zaman alır 
    

## <a name="consuming-data-in-azure-machine-learning-pipelines"></a>Azure Machine Learning ardışık hatlarında veri tüketme

![aml-veri seti](media/how-to-data-ingest-adf/aml-dataset.png)

ADF ardışık kaynaktan dönüştürülen veriler veri depolamasına (Azure Blob gibi) kaydedilir. Azure Machine Learning bu verilere [veri depolarını](https://docs.microsoft.com/azure/machine-learning/how-to-access-data#create-and-register-datastores) ve [veri kümelerini](https://docs.microsoft.com/azure/machine-learning/how-to-create-register-datasets)kullanarak erişebilir.

ADF ardışık aygıtı her çalıştığında, veriler depolama alanında farklı bir konuma kaydedilir. Konumu Azure Machine Learning'e geçirmek için ADF boru hattı, Azure Machine Learning ardışık hattını çağırır. ML ardışık hattını ararken, veri konumu ve çalıştırkimliği parametre olarak gönderilir. ML ardışık iş aktarım, veri konumunu kullanarak bir veri deposu/veri kümesi oluşturabilir. 

> [!TIP]
> Veri kümeleri [sürüm destekleme,](https://docs.microsoft.com/azure/machine-learning/how-to-version-track-datasets)böylece ML ardışık adf ardışık gelen en son verilere işaret veri kümesinin yeni bir sürümünü kaydedebilirsiniz.

Verilere bir veri deposu veya veri seti üzerinden erişilenden, bir ML modelini eğitmek için kullanabilirsiniz. Eğitim süreci, ADF'den çağrılan aynı ML ardışık boru hattının bir parçası olabilir. Ya da jupyter not defterinde deneme gibi ayrı bir işlem olabilir.

Veri kümeleri sürüm oluşturmayı desteklediğinden ve ardışık ardışık ardışık alanher çalıştırma yeni bir sürüm oluşturduğundan, bir modeli eğitmek için verilerin hangi sürümünün kullanıldığını anlamak kolaydır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Fabrikası'nda bir Databricks dizüstü bilgisayarı çalıştırma](https://docs.microsoft.com/azure/data-factory/transform-data-using-databricks-notebook)
* [Azure depolama hizmetlerinde verilere erişin](https://docs.microsoft.com/azure/machine-learning/how-to-access-data#create-and-register-datastores)
* [Azure Machine Learning'de veri kümeleriyle modelleri eğitin](https://docs.microsoft.com/azure/machine-learning/how-to-train-with-datasets)
* [Veri alma boru hattı için DevOps](https://docs.microsoft.com/azure/machine-learning/how-to-cicd-data-ingestion)

