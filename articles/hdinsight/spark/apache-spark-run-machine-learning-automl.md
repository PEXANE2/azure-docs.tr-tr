---
title: HDInsight'ta Azure Machine Learning iş yüklerini Apache Spark'ta çalıştırın
description: Azure HDInsight'ta Apache Spark'ta otomatik makine öğrenimi (AutoML) ile Azure Machine Learning iş yüklerini nasıl çalıştıracağınız öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 12/13/2019
ms.openlocfilehash: 6fc0d4cfe29e0fb189c44b307576bd08d2da8a31
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75638893"
---
# <a name="run-azure-machine-learning-workloads-with-automated-machine-learning-on-apache-spark-in-hdinsight"></a>HDInsight'ta Apache Spark'ta otomatik makine öğrenimiyle Azure Machine Learning iş yüklerini çalıştırın

Azure Machine Learning, makine öğrenimi modellerinin oluşturulmasını, eğitimini ve dağıtımını basitleştirir ve hızlandırır. Otomatik makine öğreniminde (AutoML) tanımlı bir hedef özelliğe sahip eğitim verileriyle başlarsınız ve ardından eğitim puanlarına göre verileriniz için en iyi modeli otomatik olarak seçmek için algoritma ve özellik seçimleri kombinasyonları aracılığıyla yinelersiniz. HDInsight, müşterilerin yüzlerce düğümle kümeler sağlamasına olanak tanır. Bir HDInsight kümesinde Spark'ta çalışan AutoML, kullanıcıların eğitim işlerini ölçeklendirici bir şekilde çalıştırmak ve paralel olarak birden çok eğitim işini çalıştırmak için bu düğümler arasında bilgi işlem kapasitesini kullanmalarına olanak tanır. Bu, kullanıcıların bilgileri diğer büyük veri iş yükleri ile paylaşırken AutoML denemelerini çalıştırmalarına olanak tanır.

## <a name="install-azure-machine-learning-on-an-hdinsight-cluster"></a>Azure Machine Learning'i HDInsight kümesine yükleme

Otomatik makine öğreniminin genel öğreticileri için [Bkz. Öğretici: Regresyon modelinizi oluşturmak için otomatik makine öğrenimini kullanın.](../../machine-learning/tutorial-auto-train-models.md)
Tüm yeni HDInsight-Spark kümeleri AzureML-AutoML SDK ile önceden yüklenmiş olarak gelir.

> [!Note]
> Azure Machine Learning paketleri Python3 conda ortamına yüklenir. Yüklü Jupyter dizüstü pyspark3 çekirdeği kullanılarak çalıştırılmalıdır.

AutoML'i kullanmak için Zeppelin dizüstü bilgisayarlarını da kullanabilirsiniz.

> [!Note]
> Zeppelin'in PySpark3'ün Python'un doğru sürümünü seçmediği bilinen bir [sorunu](https://community.hortonworks.com/content/supportkb/207822/the-livypyspark3-interpreter-uses-python-2-instead.html) vardır. Lütfen belgelenmiş geçici çözüm kullanımını kullanın.

## <a name="authentication-for-workspace"></a>Çalışma alanı için kimlik doğrulaması

Çalışma alanı oluşturma ve deneme gönderme kimlik doğrulama belirteci gerektirir. Bu belirteç, Bir [Azure AD uygulaması](../../active-directory/develop/app-objects-and-service-principals.md)kullanılarak oluşturulabilir. Bir [Azure REKLAM kullanıcısı,](/azure/python/python-sdk-azure-authenticate) hesapta çok faktörlü kimlik doğrulama etkinleştirilmezse, gerekli kimlik doğrulama belirteci oluşturmak için de kullanılabilir.  

Aşağıdaki kod snippet bir **Azure AD uygulaması**kullanarak bir kimlik doğrulama belirteci oluşturur.

```python
from azureml.core.authentication import ServicePrincipalAuthentication
auth_sp = ServicePrincipalAuthentication(
    tenant_id='<Azure Tenant ID>',
    service_principal_id='<Azure AD Application ID>',
    service_principal_password='<Azure AD Application Key>'
)
```

Aşağıdaki kod snippet bir **Azure AD kullanıcıkullanarak**bir kimlik doğrulama belirteci oluşturur.

```python
from azure.common.credentials import UserPassCredentials
credentials = UserPassCredentials('user@domain.com', 'my_smart_password')
```

## <a name="loading-dataset"></a>Veri kümesini yükleme

Spark'ta otomatik makine öğrenimi, tembelce değerlendirilen, veriler üzerinde değişmez işlemler yapan **Veri akışlarını**kullanır.  Veri akışı, genel okuma erişimi olan bir blob'dan veya SAS belirteci içeren bir blob URL'sinden bir veri kümesi yükleyebilir.

```python
import azureml.dataprep as dprep

dataflow_public = dprep.read_csv(
    path='https://commonartifacts.blob.core.windows.net/automl/UCI_Adult_train.csv')

dataflow_with_token = dprep.read_csv(
    path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv?st=2018-06-15T23%3A01%3A42Z&se=2019-06-16T23%3A01%3A00Z&sp=r&sv=2017-04-17&sr=b&sig=ugQQCmeC2eBamm6ynM7wnI%2BI3TTDTM6z9RPKj4a%2FU6g%3D')
```

Ayrıca, bir kerelik kayıt kullanarak veri mağazasını çalışma alanına kaydedebilirsiniz.

## <a name="experiment-submission"></a>Deneme gönderimi

Otomatik [makine öğrenimi yapılandırmasında,](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig)paketin dağıtılmış modda çalışması için özellik `spark_context` ayarlanmalıdır. Paralel `concurrent_iterations`olarak yürütülen en fazla yineleme sayısı olan özellik, Spark uygulamasının yürütücü çekirdeklerinden daha az bir sayıolarak ayarlanmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* Otomatik makine öğreniminin ardındaki motivasyon hakkında daha fazla bilgi için [Microsoft'un otomatik makine öğrenimini kullanarak Sürüm modellerine bakın!](https://azure.microsoft.com/blog/release-models-at-pace-using-microsoft-s-automl/)
* Azure ML Otomatik ML özelliklerini kullanma hakkında daha fazla bilgi için [Azure Machine Learning'deki yeni otomatik makine öğrenimi özelliklerine](https://azure.microsoft.com/blog/new-automated-machine-learning-capabilities-in-azure-machine-learning-service/) bakın
* [Microsoft Research'ten AutoML projesi](https://www.microsoft.com/research/project/automl/)
