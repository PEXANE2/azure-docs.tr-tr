---
title: ML işlem hatlarını yayımlama
titleSuffix: Azure Machine Learning
description: Machine Learning işlem hatları ve Python için Azure Machine Learning SDK ile makine öğrenimi iş akışlarını çalıştırın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: sgilley
ms.author: laobri
author: lobrien
ms.date: 8/25/2020
ms.topic: conceptual
ms.custom: how-to, contperfq1
ms.openlocfilehash: 08529d1bb50a1a5d5b3c7d0296aa36f021f45e98
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89646087"
---
# <a name="publish-and-track-machine-learning-pipelines"></a>Makine öğrenimi işlem hatlarını yayımlama ve izleme

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, bir makine öğrenimi işlem hattının iş arkadaşlarınızla veya müşterilerinizle nasıl paylaşılacağını gösterilmektedir.

Machine Learning işlem hatları, Machine Learning görevleri için yeniden kullanılabilir iş akışlarıdır. İşlem hatlarından bir avantajı daha fazla işbirliği. Ayrıca, yeni bir sürümde çalışırken müşterilerin geçerli modeli kullanmasına izin vererek işlem hatlarını de kullanabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

* Tüm işlem hattı kaynaklarınızın tutulacağı bir [Azure Machine Learning çalışma alanı](how-to-manage-workspace.md) oluşturun

* Azure Machine Learning SDK 'yı yüklemek için [geliştirme ortamınızı yapılandırın](how-to-configure-environment.md) veya SDK 'nın zaten yüklü olduğu bir [Azure Machine Learning işlem örneği](concept-compute-instance.md) kullanın

* Aşağıdaki öğreticiden yararlanarak bir makine öğrenimi işlem hattı oluşturun ve çalıştırın [: Batch Puanlama için Azure Machine Learning işlem hattı](tutorial-pipeline-batch-scoring-classification.md)oluşturun. Diğer seçenekler için bkz. [Azure MACHINE LEARNING SDK ile makine öğrenimi işlem hatları oluşturma ve çalıştırma](how-to-create-your-first-pipeline.md)

## <a name="publish-a-pipeline"></a>İşlem hattı yayımlama

İşlem hattınızı çalışır durumda yaptıktan sonra, farklı girişlerle çalışacak şekilde bir işlem hattı yayımlayabilirsiniz. Önceden yayımlanmış bir işlem hattının REST uç noktası parametreleri kabul etmek için, işlem hattınızı, `PipelineParameter` değişen bağımsız değişkenler için nesneleri kullanacak şekilde yapılandırmanız gerekir.

1. Bir işlem hattı parametresi oluşturmak için bir [pipelineparameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py&preserve-view=true) nesnesini varsayılan bir değerle kullanın.

   ```python
   from azureml.pipeline.core.graph import PipelineParameter
   
   pipeline_param = PipelineParameter(
     name="pipeline_arg",
     default_value=10)
   ```

2. Bu nesneyi işlem hattındaki `PipelineParameter` adımlardan herhangi birine bir parametre olarak aşağıdaki gibi ekleyin:

   ```python
   compareStep = PythonScriptStep(
     script_name="compare.py",
     arguments=["--comp_data1", comp_data1, "--comp_data2", comp_data2, "--output_data", out_data3, "--param1", pipeline_param],
     inputs=[ comp_data1, comp_data2],
     outputs=[out_data3],
     compute_target=compute_target,
     source_directory=project_folder)
   ```

3. Çağrıldığında bir parametreyi kabul edecek bu işlem hattını yayımlayın.

   ```python
   published_pipeline1 = pipeline_run1.publish_pipeline(
        name="My_Published_Pipeline",
        description="My Published Pipeline Description",
        version="1.0")
   ```

## <a name="run-a-published-pipeline"></a>Yayımlanmış bir işlem hattı çalıştırma

Yayımlanan tüm işlem hatları bir REST uç noktasına sahiptir. Ardışık düzen uç noktasıyla, Python olmayan istemciler de dahil olmak üzere herhangi bir dış sistemlerden işlem hattının çalıştırılmasını tetikleyebilirsiniz. Bu uç nokta, toplu Puanlama ve yeniden eğitim senaryolarında "Managed yinelenebilirlik" öğesini sunar.

Önceki işlem hattının çalıştırılmasını çağırmak için bir Azure Active Directory kimlik doğrulaması üst bilgisi belirtecine ihtiyacınız vardır. Bu tür bir belirteci almak, [Azurecliauthentication sınıf](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.azurecliauthentication?view=azure-ml-py&preserve-view=true) başvurusunda ve Azure Machine Learning not defterindeki [kimlik doğrulamasında](https://aka.ms/pl-restep-auth) açıklanmıştır.

```python
from azureml.pipeline.core import PublishedPipeline
import requests

response = requests.post(published_pipeline1.endpoint,
                         headers=aad_token,
                         json={"ExperimentName": "My_Pipeline",
                               "ParameterAssignments": {"pipeline_arg": 20}})
```

## <a name="create-a-versioned-pipeline-endpoint"></a>Sürümlü ardışık düzen uç noktası oluşturma

Birden çok yayınlanan işlem hattı arkasında bir ardışık düzen uç noktası oluşturabilirsiniz. Bu, size yineleme yaparken ve ML işlem hatlarınızı güncelleştirdiğinizde sabit bir REST uç noktası sağlar.

```python
from azureml.pipeline.core import PipelineEndpoint

published_pipeline = PipelineEndpoint.get(workspace=ws, name="My_Published_Pipeline")
pipeline_endpoint = PipelineEndpoint.publish(workspace=ws, name="PipelineEndpointTest",
                                            pipeline=published_pipeline, description="Test description Notebook")
```

## <a name="submit-a-job-to-a-pipeline-endpoint"></a>İşlem hattı uç noktasına iş gönderme

Bir işi bir ardışık düzen uç noktasının varsayılan sürümüne gönderebilirsiniz:

```python
pipeline_endpoint_by_name = PipelineEndpoint.get(workspace=ws, name="PipelineEndpointTest")
run_id = pipeline_endpoint_by_name.submit("PipelineEndpointExperiment")
print(run_id)
```

Ayrıca, belirli bir sürüme iş gönderebilirsiniz:

```python
run_id = pipeline_endpoint_by_name.submit("PipelineEndpointExperiment", pipeline_version="0")
print(run_id)
```

Aynı REST API kullanılarak aynı şekilde gerçekleştirilebilir:

```python
rest_endpoint = pipeline_endpoint_by_name.endpoint
response = requests.post(rest_endpoint, 
                         headers=aad_token, 
                         json={"ExperimentName": "PipelineEndpointExperiment",
                               "RunSource": "API",
                               "ParameterAssignments": {"1": "united", "2":"city"}})
```

## <a name="use-published-pipelines-in-the-studio"></a>Studio 'da yayınlanmış işlem hatlarını kullanma

Ayrıca, Studio 'dan yayımlanmış bir işlem hattı da çalıştırabilirsiniz:

1. [Azure Machine Learning Studio](https://ml.azure.com)'da oturum açın.

1. [Çalışma alanınızı görüntüleyin](how-to-manage-workspace.md#view).

1. Sol tarafta **uç noktalar**' ı seçin.

1. Üstte **ardışık düzen uç noktaları**' nı seçin.
 ![Machine Learning yayımlanmış işlem hatları listesi](./media/how-to-create-your-first-pipeline/pipeline-endpoints.png)

1. İşlem hattı uç noktasının önceki çalıştırmalarının sonuçlarını çalıştırmak, kullanmak veya gözden geçirmek için belirli bir işlem hattı seçin.

## <a name="disable-a-published-pipeline"></a>Yayımlanmış bir ardışık düzeni devre dışı bırakma

Yayınlanan işlem hatları listenizden bir işlem hattını gizlemek için, bunu Studio 'da veya SDK 'dan devre dışı bırakabilirsiniz:

```python
# Get the pipeline by using its ID from Azure Machine Learning studio
p = PublishedPipeline.get(ws, id="068f4885-7088-424b-8ce2-eeb9ba5381a6")
p.disable()
```

İle yeniden etkinleştirebilirsiniz `p.enable()` . Daha fazla bilgi için bkz. [Publishedpipeline sınıf](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.publishedpipeline?view=azure-ml-py&preserve-view=true) başvurusu.

## <a name="next-steps"></a>Sonraki adımlar

- Makine öğrenimi işlem hatlarını daha fazla incelemek için [GitHub 'da bu jupi not defterlerini](https://aka.ms/aml-pipeline-readme) kullanın.
- [Azureml işlem hatları-Core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py&preserve-view=true) paketi ve [azureml-işlem hatları-adımlar](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py&preserve-view=true) paketi için SDK başvuru yardımına bakın.
- Hata ayıklama ve işlem hatlarında sorun [giderme hakkında ipuçları için bkz](how-to-debug-pipelines.md) ..
