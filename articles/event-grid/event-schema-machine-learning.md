---
title: Azure Olay Izgara Makine Öğrenme olay şeması
description: Azure Olay Ağıtı ile Makine Öğrenimi Çalışma Alanı etkinlikleri için sağlanan özellikleri açıklar
services: event-grid
author: jenns
ms.service: event-grid
ms.topic: reference
ms.date: 10/18/2019
ms.author: jenns
ms.openlocfilehash: 4051598a9abd787f6707e67a8c4dab12fc6d626a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79202153"
---
# <a name="azure-event-grid-event-schema-for-azure-machine-learning"></a>Azure Makine Öğrenimi için Azure Olay Izgara etkinliği şeması

Bu makalede, makine öğrenimi çalışma alanı olayları için özellikleri ve şema sağlar. Etkinlik şemalarına giriş için [Azure Olay Izgara olay şemasına](event-schema.md)bakın.

Örnek komut dosyaları ve öğreticilerin listesi için [AzureML etkinlik kaynağına](event-sources.md#azure-machine-learning)bakın.

## <a name="available-event-types"></a>Kullanılabilir etkinlik türleri

Azure Machine Learning aşağıdaki etkinlik türlerini yayır:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.MachineLearningServices.ModelRegistered | Yeni bir Model veya Model sürümü başarıyla kaydedildiğinde yükseltilir. |
| Microsoft.MachineLearningServices.ModelDeployed | Model(ler) bir Bitiş Noktası'na başarıyla dağıtıldığında yükseltilir. |
| Microsoft.MachineLearningServices.RunCompleted | Bir Çalışma başarıyla tamamlandığında yükseltilir. |
| Microsoft.MachineLearningServices.DatasetDriftDetected | Bir Dataset drift monitörü sürüklenme algıladığında yükseltilir. |
| Microsoft.MachineLearningServices.RunStatus Değiştirildi | Çalıştırma durumu 'başarısız' olarak değiştirildiğinde yükseltilir. |

## <a name="the-contents-of-an-event-response"></a>Olay yanıtının içeriği

Bir olay tetiklendiğinde, Olay Izgara hizmeti bitiş noktasına abone olmak için bu olayla ilgili verileri gönderir.

Bu bölümde, bu verilerin her olay için nasıl görüneceğine dair bir örnek yer almaktadır.

### <a name="microsoftmachinelearningservicesmodelregistered-event"></a>Microsoft.MachineLearningServices.ModelRegistered olay

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "models/sklearn_regression_model:20",
  "eventType": "Microsoft.MachineLearningServices.ModelRegistered",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "ModelName": "sklearn_regression_model",
    "ModelVersion": 20,
    "ModelTags": {
        "area": "diabetes",
        "type": "regression"
    },
    "ModelProperties": {
        "type": "test"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesmodeldeployed-event"></a>Microsoft.MachineLearningServices.ModelDeployed olay

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "endpoints/my-sklearn-service",
  "eventType": "Microsoft.MachineLearningServices.ModelDeployed",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "ServiceName": "my-sklearn-service",
    "ServiceComputeType": "ACI",
    "ModelIds": "sklearn_regression_model:1,sklearn_regression_model:2",
    "ServiceTags": {
        "area": "diabetes",
        "type": "regression"
    },
    "ServiceProperties": {
        "type": "test"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesruncompleted-event"></a>Microsoft.MachineLearningServices.RunCompleted olay

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "experiments/0fa9dfaa-cba3-4fa7-b590-23e48548f5c1/runs/AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5",
  "eventType": "Microsoft.MachineLearningServices.RunCompleted",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "ExperimentId": "0fa9dfaa-cba3-4fa7-b590-23e48548f5c1",
    "ExperimentName": "automl-local-regression",
    "RunId": "AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5",
    "RunType": null,
    "RunTags": {},
    "RunProperties": {
        "runTemplate": "automl_child",
        "pipeline_id": "5adc0a4fe02504a586f09a4fcbb241f9a4012062",
        "pipeline_spec": "{\"objects\": [{\"class_name\": \"StandardScaler\", \"module\": \"sklearn.preprocessing\", \"param_args\": [], \"param_kwargs\": {\"with_mean\": true, \"with_std\": false}, \"prepared_kwargs\": {}, \"spec_class\": \"preproc\"}, {\"class_name\": \"LassoLars\", \"module\": \"sklearn.linear_model\", \"param_args\": [], \"param_kwargs\": {\"alpha\": 0.001, \"normalize\": true}, \"prepared_kwargs\": {}, \"spec_class\": \"sklearn\"}], \"pipeline_id\": \"5adc0a4fe02504a586f09a4fcbb241f9a4012062\"}",
        "training_percent": "100",
        "predicted_cost": "0.062226144097381045",
        "iteration": "5",
        "run_template": "automl_child",
        "run_preprocessor": "StandardScalerWrapper",
        "run_algorithm": "LassoLars",
        "conda_env_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/conda_env_v_1_0_0.yml",
        "model_name": "AutoMLad912b2d65",
        "scoring_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/scoring_file_v_1_0_0.py",
        "model_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/model.pkl"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesdatasetdriftdetected-event"></a>Microsoft.MachineLearningServices.DatasetDriftDetected olay

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "datadrifts/{}/runs/{}",
  "eventType": "Microsoft.MachineLearningServices.DatasetDriftDetected",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "DataDriftId": "01d29aa4-e6a4-470a-9ef3-66660d21f8ef",
    "DataDriftName": "myDriftMonitor",
    "RunId": "01d29aa4-e6a4-470a-9ef3-66660d21f8ef_1571590300380",
    "BaseDatasetId": "3c56d136-0f64-4657-a0e8-5162089a88a3",
    "TargetDatasetId": "d7e74d2e-c972-4266-b5fb-6c9c182d2a74",
    "DriftCoefficient": 0.83503490684792081,
    "StartTime": "2019-07-04T00:00:00+00:00",
    "EndTime": "2019-07-05T00:00:00+00:00"
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesrunstatuschanged-event"></a>Microsoft.MachineLearningServices.RunStatusChanged olay

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "experiments/0fa9dfaa-cba3-4fa7-b590-23e48548f5c1/runs/AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5",
  "eventType": "Microsoft.MachineLearningServices.RunCompleted",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "ExperimentId": "0fa9dfaa-cba3-4fa7-b590-23e48548f5c1",
    "ExperimentName": "automl-local-regression",
    "RunId": "AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5",
    "RunType": null,
    "RunTags": {},
    "RunProperties": {
        "runTemplate": "automl_child",
        "pipeline_id": "5adc0a4fe02504a586f09a4fcbb241f9a4012062",
        "pipeline_spec": "{\"objects\": [{\"class_name\": \"StandardScaler\", \"module\": \"sklearn.preprocessing\", \"param_args\": [], \"param_kwargs\": {\"with_mean\": true, \"with_std\": false}, \"prepared_kwargs\": {}, \"spec_class\": \"preproc\"}, {\"class_name\": \"LassoLars\", \"module\": \"sklearn.linear_model\", \"param_args\": [], \"param_kwargs\": {\"alpha\": 0.001, \"normalize\": true}, \"prepared_kwargs\": {}, \"spec_class\": \"sklearn\"}], \"pipeline_id\": \"5adc0a4fe02504a586f09a4fcbb241f9a4012062\"}",
        "training_percent": "100",
        "predicted_cost": "0.062226144097381045",
        "iteration": "5",
        "run_template": "automl_child",
        "run_preprocessor": "StandardScalerWrapper",
        "run_algorithm": "LassoLars",
        "conda_env_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/conda_env_v_1_0_0.yml",
        "model_name": "AutoMLad912b2d65",
        "scoring_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/scoring_file_v_1_0_0.py",
        "model_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/model.pkl"
    },
   "RunStatus": "failed"
   },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```




## <a name="event-properties"></a>Olay özellikleri

Bir olay aşağıdaki üst düzey verilere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| konu başlığı | string | Olay kaynağına tam kaynak yolu. Bu alan yazılamaz. Event Grid bu değeri sağlar. |
| Konu | string | Olay konusunun yayımcı tarafından tanımlanan yolu. |
| Eventtype | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Olayın sağlayıcının UTC zamanına bağlı olarak oluşturulan süre. |
| id | string | Etkinlik için benzersiz tanımlayıcı. |
| veri | object | Blob depolama olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

Veri nesnesi her olay türü için aşağıdaki özelliklere sahiptir:

### <a name="microsoftmachinelearningservicesmodelregistered"></a>Microsoft.MachineLearningServices.ModelRegistered

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| ModelName | string | Kayıtlı modelin adı. |
| ModelSürümü | string | Kayıtlı modelin sürümü. |
| ModelEtiketler | object | Kayıtlı modelin etiketleri. |
| Model Özellikleri | object | Kayıtlı modelin özellikleri. |

### <a name="microsoftmachinelearningservicesmodeldeployed"></a>Microsoft.MachineLearningServices.ModelDeployed

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| ServiceName | string | Dağıtılan hizmetin adı. |
| ServiceComputeType | string | Dağıtılan hizmetin işlem türü (örn. ACI, AKS). |
  | Modelids | string | Virgülle ayrılmış model işlisi listesi. Hizmette dağıtılan modellerin disleri. |
| ServisEtiketler | object | Dağıtılan hizmetin etiketleri. |
| Hizmet Özellikleri | object | Dağıtılan hizmetin özellikleri. |

### <a name="microsoftmachinelearningservicesruncompleted"></a>Microsoft.MachineLearningServices.RunCompleted

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Deney Id | string | Çalıştırmanın ait olduğu denemenin kimliği. |
| Deneme Adı | string | Çalıştırmanın ait olduğu denemenin adı. |
| Runid | string | Tamamlanan Çalıştır kimliği. |
| RunType | string | Tamamlanan Çalıştır'ın Çalışma Türü. |
| RunTags | object | Tamamlanan Çalıştır etiketleri. |
| RunProperties | object | Tamamlanan Çalıştır'ın özellikleri. |

### <a name="microsoftmachinelearningservicesdatasetdriftdetected"></a>Microsoft.MachineLearningServices.DatasetDriftDetected

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| DataDriftid | string | Olayı tetikleyen veri sürüklenme monitörünün kimliği. |
| DataDriftName | string | Olayı tetikleyen veri sürüklenme monitörünün adı. |
| Runid | string | Veri kayması algılayan Çalıştır kimliği. |
| BaseDatasetId | string | Sürüklenme algılamak için kullanılan temel Dataset kimliği. |
| TargetDatasetId | string | Sürüklenme tespit etmek için kullanılan hedef Dataset'in kimliği. |
| Sürüklenme Katsayısı | double | Olayı tetikleyen katsayı sonucu. |
| StartTime | datetime | Sürüklenme algılamayla sonuçlanan hedef veri kümesi zaman serisinin başlangıç saati.  |
| EndTime | datetime | Sürüklenme algılamaile sonuçlanan hedef veri kümesi zaman serisinin bitiş saati. |

### <a name="microsoftmachinelearningservicesrunstatuschanged"></a>Microsoft.MachineLearningServices.RunStatus Değiştirildi

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Deney Id | string | Çalıştırmanın ait olduğu denemenin kimliği. |
| Deneme Adı | string | Çalıştırmanın ait olduğu denemenin adı. |
| Runid | string | Tamamlanan Çalıştır kimliği. |
| RunType | string | Tamamlanan Çalıştır'ın Çalışma Türü. |
| RunTags | object | Tamamlanan Çalıştır etiketleri. |
| RunProperties | object | Tamamlanan Çalıştır'ın özellikleri. |
| RunStatus | string | Run'ın durumu. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Etkinlik Izgarasına giriş için [olay ızgarası nedir?](overview.md)
* Azure Olay Ağı aboneliği oluşturma hakkında daha fazla bilgi için [Olay Ağı abonelik şemasına](subscription-creation-schema.md) bakın
* Azure Machine Learning ile Azure Olay Izgarasını kullanmaya giriş için [bkz.](/azure/machine-learning/service/concept-event-grid-integration)
* Azure Machine Learning ile Azure Olay Ağıtını kullanma örneği [için](/azure/machine-learning/service/how-to-use-event-grid) bkz.
