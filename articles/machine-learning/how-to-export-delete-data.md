---
title: Çalışma alanı verilerini dışa aktarma veya silme
titleSuffix: Azure Machine Learning
description: Azure Machine Learning stüdyosu, CLI, SDK ve kimlik doğrulaması REST API'leriyle çalışma alanınızı nasıl dışa aktarıp silebilirsiniz öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218280"
---
# <a name="export-or-delete-your-machine-learning-service-workspace-data"></a>Machine Learning hizmeti çalışma alanı verilerinizi dışa aktarma veya silme

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning'de, kimlik doğrulaması yapılan REST API ile çalışma alanı verilerinizi dışa aktarabilir veya silebilirsiniz. Bu makalede nasıl söyler.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="control-your-workspace-data"></a>Çalışma alanı verilerinizi denetleme

Azure Machine Learning tarafından depolanan ürün içi veriler, Azure Machine Learning stüdyosu, CLI, SDK ve kimlik doğrulaması REST API'leri aracılığıyla dışa aktarma ve silme için kullanılabilir. Telemetri verilerine Azure Gizlilik portalından erişilebilir. 

Azure Machine Learning'de kişisel veriler, çalışma geçmişi belgelerindeki kullanıcı bilgileri ve hizmetle olan bazı kullanıcı etkileşimlerinin telemetri kayıtlarından oluşur.

## <a name="delete-workspace-data-with-the-rest-api"></a>REST API ile çalışma alanı verilerini silme

Verileri silmek için HTTP DELETE fiili ile aşağıdaki API aramaları yapılabilir. Bunlar, `https://management.core.windows.net/` bitiş noktası `Authorization: Bearer <arm-token>` için AAD erişim `<arm-token>` belirteci nerede istek bir üstbilgi sahip tarafından yetkilendirilir.  

Bu belirteci nasıl alacağınızı öğrenmek ve Azure uç noktalarını aramak için ML kaynaklarını ve [Azure REST API belgelerini](https://docs.microsoft.com/rest/api/azure/) [yönetmek için REST'i kullan'a](how-to-manage-rest.md) bakın.  

Aşağıdaki örneklerde, metni ilişkili {} kaynağı belirleyen örnek adlarla değiştirin.

### <a name="delete-an-entire-workspace"></a>Çalışma alanının tamamını silme

Tüm çalışma alanını silmek için bu aramayı kullanın.  
> [!WARNING]
> Tüm bilgiler silinir ve çalışma alanı artık kullanılabilir olmayacaktır.

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}?api-version=2019-11-01

### <a name="delete-models"></a>Modelleri silme

Modellerin ve kimliklerinin listesini almak için bu aramayı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models?api-version=2019-11-01

Tek tek modeller ile silinebilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models/{id}?api-version=2019-11-01

### <a name="delete-assets"></a>Varlıkları silme

Varlıkların ve kimliklerinin listesini almak için bu aramayı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets?api-version=2019-11-01

Tek tek varlıklar şu şekilde silinebilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets/{id}?api-version=2019-11-01

### <a name="delete-images"></a>Görüntüleri silme

Görüntülerin ve kimliklerinin listesini almak için bu aramayı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images?api-version=2019-11-01

Tek tek görüntüler şu şekilde silinebilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images/{id}?api-version=2019-11-01

### <a name="delete-services"></a>Hizmetleri silme

Hizmetlerin ve kimliklerinin listesini almak için bu aramayı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services?api-version=2019-11-01

Bireysel hizmetler şu şekilde silinebilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services/{id}?api-version=2019-11-01

## <a name="export-service-data-with-the-rest-api"></a>REST API ile hizmet verilerini dışa aktarma

Veri aktarmak için HTTP GET fiili ile aşağıdaki API aramaları yapılabilir. Bunlar, istekte bir `Authorization: Bearer <arm-token>` üstbilgi nin olması `<arm-token>` yla yetkilendirilir, bitiş noktası için AAD erişim belirteci nerede`https://management.core.windows.net/`  

Bu belirteci nasıl alacağınızı öğrenmek ve Azure uç noktalarını aramak için ML kaynaklarını ve [Azure REST API belgelerini](https://docs.microsoft.com/rest/api/azure/) [yönetmek için REST'i kullan](how-to-manage-rest.md) 'a bakın.   

Aşağıdaki örneklerde, metni ilişkili {} kaynağı belirleyen örnek adlarla değiştirin.

### <a name="export-workspace-information"></a>Çalışma Alanı bilgilerini dışa aktarma

Tüm çalışma alanlarının listesini almak için bu aramayı kullanın:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces?api-version=2019-11-01

Bireysel çalışma alanı hakkında bilgi şu şekilde elde edilebilir:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}?api-version=2019-11-01

### <a name="export-compute-information"></a>İhracat Bilgi

Bir çalışma alanına bağlı tüm bilgi işlem hedefleri şu şekilde elde edilebilir:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroup/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes?api-version=2019-11-01

Tek bir bilgi işlem hedefi hakkında bilgi şu şekilde elde edilebilir:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroup/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes/{computeName}?api-version=2019-11-01

### <a name="export-run-history-data"></a>Çalışma geçmişi verilerini dışa aktarma

Tüm denemelerin ve bilgilerin listesini almak için bu aramayı kullanın:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments 

Belirli bir denemeiçin tüm çalıştırmalar aşağıdakiler tarafından elde edilebilir:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/runs 

Run geçmiş öğeleri tarafından elde edilebilir:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/runs/{runId} 

Bir denemeiçin tüm çalıştırma ölçümleri şu şekilde elde edilebilir:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/metrics 

Tek bir çalıştırma ölçümü:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/metrics/{metricId}

### <a name="export-artifacts"></a>Yapıtları dışa aktarma

Yapıtların ve bunların yollarının listesini almak için bu aramayı kullanın:

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/artifacts/origins/ExperimentRun/containers/{runId}

### <a name="export-notifications"></a>Dışa aktarma bildirimleri

Depolanan görevlerin listesini almak için bu aramayı kullanın:

    https://{location}.experiments.azureml.net/notification/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/tasks

Tek bir görev için bildirimler:

    https://{location}.experiments.azureml.net/notification/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}tasks/{taskId}

### <a name="export-data-stores"></a>Veri depolarını dışa aktarma

Veri depolarının listesini almak için bu aramayı kullanın:

    https://{location}.experiments.azureml.net/datastore/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/datastores

Bireysel veri depoları şu şekilde elde edilebilir:

    https://{location}.experiments.azureml.net/datastore/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/datastores/{name}

### <a name="export-models"></a>Modelleri dışarı aktarma

Modellerin ve kimliklerinin listesini almak için bu aramayı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models?api-version=2019-11-01

Bireysel modeller elde edilebilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models/{id}?api-version=2019-11-01

### <a name="export-assets"></a>İhracat varlıkları

Varlıkların ve kimliklerinin listesini almak için bu aramayı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets?api-version=2019-11-01

Münferit varlıklar:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets/{id}?api-version=2019-11-01

### <a name="export-images"></a>Görüntüleri dışa aktarma

Görüntülerin ve kimliklerinin listesini almak için bu aramayı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images?api-version=2019-11-01

Bireysel görüntüler elde edilebilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images/{id}?api-version=2019-11-01

### <a name="export-services"></a>İhracat hizmetleri

Hizmetlerin ve kimliklerinin listesini almak için bu aramayı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services?api-version=2019-11-01

Bireysel hizmetler:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services/{id}?api-version=2019-11-01

### <a name="export-pipeline-experiments"></a>İhracat Boru Hattı Deneyleri

Bireysel deneyler:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Experiments/{experimentId}

### <a name="export-pipeline-graphs"></a>İhracat Boru Hattı Grafikleri

Bireysel grafikler elde edilebilir:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Graphs/{graphId}

### <a name="export-pipeline-modules"></a>İhracat Boru Hattı Modülleri

Modüller:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Modules/{id}

### <a name="export-pipeline-templates"></a>Dışa Aktarma Boru Hattı Şablonları

Şablonlar tarafından elde edilebilir:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Templates/{templateId}

### <a name="export-pipeline-data-sources"></a>İhracat Boru Hattı Veri Kaynakları

Veri Kaynakları şu kaynaklarla elde edilebilir:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/DataSources/{id}

## <a name="delete-assets-in-the-designer"></a>Tasarımcıdaki varlıkları silme

Denemenizi oluşturduğunuz tasarımcıda, tek tek varlıkları silin:

1. Tasarımcıya git

    ![Varlıkları silme](./media/how-to-export-delete-data/delete-experiment.png)

1. Listede, silmek için tek tek ardışık düzen ektürününü seçin.

1. **Sil**’i seçin.

### <a name="delete-datasets-in-the-designer"></a>Tasarımcıdaki veri kümelerini silme

Tasarımcıdaki veri kümelerini silmek için, bağlı depolama hesaplarına gitmek ve oradaki veri kümelerini silmek için Azure portalını veya Depolama Gezgini'ni kullanın. Tasarımcıdaki veri kümelerinin kaydını kaldırmak yalnızca depolama daki başvuru noktasını kaldırır.

## <a name="export-data-in-the-designer"></a>Tasarımcıda veri verme

Denemenizi oluşturduğunuz tasarımcıda, eklediğiniz verileri dışa aktarın:

1. Solda Veri **Kümeleri'ni**seçin.

1. Listede, dışa aktarmak için veri kümesini seçin

    ![Verileri indirme](./media/how-to-export-delete-data/unregister-dataset.png)
