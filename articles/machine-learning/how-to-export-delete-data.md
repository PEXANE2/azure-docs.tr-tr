---
title: Dışarı aktarma veya çalışma alanı verilerini sil
titleSuffix: Azure Machine Learning
description: Azure Machine Learning Studio, CLı, SDK ve kimliği doğrulanmış REST API 'Leri ile çalışma alanınızı dışarı aktarmayı veya silmeyi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: lobrien
ms.author: laobri
ms.date: 03/06/2020
ms.custom: seodec18
ms.openlocfilehash: 4abef0146b4bf0cfaa254d196b0ca68f0d8ac883
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79218280"
---
# <a name="export-or-delete-your-machine-learning-service-workspace-data"></a>Dışarı aktarma veya, Machine Learning hizmeti çalışma alanı verilerini sil

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning'de dışarı aktarma veya kimliği doğrulanmış REST API ile çalışma alanı verilerini sil. Bu makalede nasıl yapılacağı açıklanmaktadır.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="control-your-workspace-data"></a>Çalışma alanı sizin denetiminizdedir

Azure Machine Learning tarafından depolanan ürün içi veriler Azure Machine Learning Studio, CLı, SDK ve kimliği doğrulanmış REST API 'Leri aracılığıyla dışarı ve silmeye hazırdır. Telemetri verilerini Azure gizlilik portal üzerinden erişilebilir. 

Azure Machine Learning, kişisel veriler, çalışma geçmişi belgelerinin ve hizmet ile bazı kullanıcı etkileşimlerinin telemetri kayıtlarındaki Kullanıcı bilgilerinden oluşur.

## <a name="delete-workspace-data-with-the-rest-api"></a>REST API'si ile çalışma alanı verilerini sil

Şu API çağrıları, verileri silmek için DELETE HTTP fiili ile yapılabilir. Bunlar, istekte bir `Authorization: Bearer <arm-token>` üst bilgisi bulundurarak yetkilendirilir; burada `<arm-token>`, `https://management.core.windows.net/` uç noktası için AAD erişim belirtecidir.  

Bu belirteci alma ve Azure uç noktalarını çağırma hakkında bilgi edinmek için bkz. ML kaynaklarını ve [azure REST API belgelerini](https://docs.microsoft.com/rest/api/azure/) [yönetmek için REST kullanma](how-to-manage-rest.md) .  

Aşağıdaki örneklerde {} içindeki metni, ilişkili kaynağı belirten örnek adlarıyla değiştirin.

### <a name="delete-an-entire-workspace"></a>Tüm bir çalışma alanını silme

Bu çağrı, çalışma alanının tamamına silmek için kullanın.  
> [!WARNING]
> Tüm bilgiler silinir ve bu çalışma alanında kullanılabilir.

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}?api-version=2019-11-01

### <a name="delete-models"></a>Modeli Sil

Bu çağrı, modelleri ve kimliklerinin bir listesini almak için kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models?api-version=2019-11-01

Tek tek modelleri ile silinebilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models/{id}?api-version=2019-11-01

### <a name="delete-assets"></a>Varlıkları silme

Varlıklar ve kimliklerinin bir listesini almak için bu çağrıyı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets?api-version=2019-11-01

Tek tek varlıklar ile silinebilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets/{id}?api-version=2019-11-01

### <a name="delete-images"></a>Görüntüleri silme

Görüntüleri ve kimliklerinin bir listesini almak için bu çağrıyı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images?api-version=2019-11-01

Birlikte ayrı görüntüleri silinebilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images/{id}?api-version=2019-11-01

### <a name="delete-services"></a>Hizmetleri Sil

Bu çağrı, hizmetleri ve kimliklerinin bir listesini almak için kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services?api-version=2019-11-01

Tek tek Hizmetleri ile silinebilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services/{id}?api-version=2019-11-01

## <a name="export-service-data-with-the-rest-api"></a>REST API sahip hizmet verileri dışarı aktarma

Verileri dışarı aktarma için şu API çağrıları HTTP GET fiili ile yapılabilir. Bunlar, istekte bir `Authorization: Bearer <arm-token>` üst bilgisi bulundurarak yetkilendirilir; burada `<arm-token>`, uç nokta için AAD erişim belirtecidir `https://management.core.windows.net/`  

Bu belirteci nasıl alabileceğinizi ve Azure uç noktalarına nasıl çağrılacağını öğrenmek için bkz. ML kaynaklarını ve [azure REST API belgelerini](https://docs.microsoft.com/rest/api/azure/) [yönetmek için REST kullanma](how-to-manage-rest.md) ..   

Aşağıdaki örneklerde {} içindeki metni, ilişkili kaynağı belirten örnek adlarıyla değiştirin.

### <a name="export-workspace-information"></a>Çalışma alanı bilgileri verme

Tüm çalışma alanlarının bir listesini almak için bu çağrıyı kullanın:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces?api-version=2019-11-01

Tek bir çalışma alanı hakkında bilgi tarafından alınabilir:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}?api-version=2019-11-01

### <a name="export-compute-information"></a>İşlem bilgileri verme

Tüm işlem, bir çalışma alanına bağlı hedefler tarafından elde edilebilir:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroup/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes?api-version=2019-11-01

Tek işlem hedefi hakkında bilgi tarafından alınabilir:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroup/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes/{computeName}?api-version=2019-11-01

### <a name="export-run-history-data"></a>Çalıştırma geçmişini dışarı aktarma

Tüm denemeler ve kendi bilgi listesini almak için bu çağrıyı kullanın:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments 

Belirli bir deneme için tüm çalıştırmalar tarafından alınabilir:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/runs 

Çalıştırma geçmişi öğeleri tarafından elde edilebilir:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/runs/{runId} 

Bir deneme için tüm çalışma ölçümleri tarafından alınabilir:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/metrics 

Tek bir çalışma ölçüm tarafından alınabilir:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/metrics/{metricId}

### <a name="export-artifacts"></a>Yapıtları dışarı aktarma

Yapıtlar ve yollarının listesini almak için bu çağrıyı kullanın:

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/artifacts/origins/ExperimentRun/containers/{runId}

### <a name="export-notifications"></a>Dışarı aktarma bildirimleri

Saklı görevlerinin listesini almak için bu çağrıyı kullanın:

    https://{location}.experiments.azureml.net/notification/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/tasks

Bildirimler için tek bir görev tarafından alınabilir:

    https://{location}.experiments.azureml.net/notification/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}tasks/{taskId}

### <a name="export-data-stores"></a>Dışarı aktarma veri depoları

Veri depolarının bir listesini almak için bu çağrıyı kullanın:

    https://{location}.experiments.azureml.net/datastore/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/datastores

Ayrı veri depoları tarafından alınabilir:

    https://{location}.experiments.azureml.net/datastore/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/datastores/{name}

### <a name="export-models"></a>Modelleri dışarı aktarma

Bu çağrı, modelleri ve kimliklerinin bir listesini almak için kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models?api-version=2019-11-01

Tek tek modelleri tarafından alınabilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models/{id}?api-version=2019-11-01

### <a name="export-assets"></a>Dışarı aktarma varlıklar

Varlıklar ve kimliklerinin bir listesini almak için bu çağrıyı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets?api-version=2019-11-01

Tek tek varlıklar tarafından alınabilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets/{id}?api-version=2019-11-01

### <a name="export-images"></a>Görüntü dışarı aktarma

Görüntüleri ve kimliklerinin bir listesini almak için bu çağrıyı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images?api-version=2019-11-01

Ayrı görüntüleri tarafından alınabilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images/{id}?api-version=2019-11-01

### <a name="export-services"></a>Dışarı aktarma hizmetleri

Bu çağrı, hizmetleri ve kimliklerinin bir listesini almak için kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services?api-version=2019-11-01

Tek tek Hizmetleri tarafından alınabilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services/{id}?api-version=2019-11-01

### <a name="export-pipeline-experiments"></a>İşlem hattı denemeleri dışarı aktarma

Tek tek denemeleri tarafından alınabilir:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Experiments/{experimentId}

### <a name="export-pipeline-graphs"></a>İşlem hattı grafikleri dışarı aktarma

Tek tek grafikleri tarafından alınabilir:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Graphs/{graphId}

### <a name="export-pipeline-modules"></a>Ardışık Düzen modüllerine dışarı aktarma

Modüller tarafından alınabilir:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Modules/{id}

### <a name="export-pipeline-templates"></a>İşlem hattı şablonlarını dışarı aktarma

Şablonları tarafından alınabilir:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Templates/{templateId}

### <a name="export-pipeline-data-sources"></a>İşlem hattı veri kaynakları Dışarı Aktar

Veri kaynakları tarafından alınabilir:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/DataSources/{id}

## <a name="delete-assets-in-the-designer"></a>Tasarımcıda varlıkları silme

Denemenizi oluşturduğunuz tasarımcıda tek tek varlıkları silin:

1. Tasarımcıya git

    ![Varlıkları silme](./media/how-to-export-delete-data/delete-experiment.png)

1. Listede, silinecek tek işlem hattı taslağını seçin.

1. **Sil**’i seçin.

### <a name="delete-datasets-in-the-designer"></a>Tasarımcıda veri kümelerini silme

Tasarımcıda veri kümelerini silmek için Azure portal veya Depolama Gezgini kullanarak bağlı depolama hesaplarına gidin ve veri kümelerini burada silin. Tasarımcıdaki veri kümelerinin kaydı, yalnızca depolamadaki başvuru noktasını kaldırır.

## <a name="export-data-in-the-designer"></a>Tasarımcıda verileri dışarı aktarma

Denemenizi oluşturduğunuz tasarımcıda, eklediğiniz verileri dışarı aktarın:

1. Sol tarafta **veri kümeleri**' ni seçin.

1. Listeden dışarı aktarılacak veri kümesini seçin

    ![Verileri indir](./media/how-to-export-delete-data/unregister-dataset.png)
