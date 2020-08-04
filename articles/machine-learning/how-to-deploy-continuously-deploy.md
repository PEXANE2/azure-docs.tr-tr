---
title: Azure Machine Learning modellerini sürekli dağıtın
titleSuffix: Azure Machine Learning
description: Azure Machine Learning DevOps uzantısıyla sürekli olarak modelleri dağıtmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: gopalv
author: gvashishtha
ms.date: 08/03/2020
ms.topic: conceptual
ms.custom: how-to, tracking-python
ms.openlocfilehash: c95e1012decf1e3ae4ee3fbab8aae1a4c1a71be9
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87544662"
---
# <a name="continuously-deploy-models"></a>Modelleri sürekli dağıt

Bu makalede, kayıtlı modellerin yeni sürümlerini otomatik olarak denetlemek ve bu yeni modelleri üretime göndermek için Azure DevOps 'ta sürekli dağıtımın nasıl kullanılacağı gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, Azure Machine Learning çalışma alanınızda zaten bir model kaydetmiş olduğunuz varsayılmaktadır. Eğitim ve bir scikit-öğrenme modeli kaydetme örneği için [Bu öğreticiye](how-to-train-scikit-learn.md) bakın.

## <a name="continuously-deploy-models"></a>Modelleri sürekli dağıt

[Azure DevOps](https://azure.microsoft.com/services/devops/)için Machine Learning uzantısını kullanarak, modelleri sürekli olarak dağıtabilirsiniz. Yeni bir makine öğrenimi modeli bir Azure Machine Learning çalışma alanına kaydedildiğinde bir dağıtım işlem hattı tetiklemek için Azure DevOps Machine Learning uzantısını kullanabilirsiniz.

1. [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops)için kaydolun, bu, uygulamanızı sürekli tümleştirme ve mümkün olan herhangi bir platforma veya buluta teslim eden bir uygulamadır. (Azure Pipelines [Machine Learning işlem hatları](concept-ml-pipelines.md#compare)ile aynı olmadığını unutmayın.)

1. [Azure DevOps projesi oluşturun.](https://docs.microsoft.com/azure/devops/organizations/projects/create-project?view=azure-devops)

1. [Azure Pipelines için Machine Learning uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml&targetId=6756afbe-7032-4a36-9cb6-2771710cadc2&utm_source=vstsproduct&utm_medium=ExtHubManageList)yükler.

1. Yapılarınıza erişebilmek için Azure Machine Learning çalışma alanınıza bir hizmet sorumlusu bağlantısı kurmak üzere hizmet bağlantıları 'nı kullanın. Proje Ayarları ' na gidin, **hizmet bağlantıları**' nı seçin ve ardından **Azure Resource Manager**' yi seçin:

    [![Azure Resource Manager seçin](media/how-to-deploy-and-where/view-service-connection.png)](media/how-to-deploy-and-where/view-service-connection-expanded.png)

1. **Kapsam düzeyi** listesinde **AzureMLWorkspace**' ı seçin ve ardından kalan değerleri girin:

    ![AzureMLWorkspace seçin](media/how-to-deploy-and-where/resource-manager-connection.png)

1. Makine öğrenimi modelinizi Azure Pipelines kullanarak sürekli olarak dağıtmak için, işlem hatları altında **yayın**' ı seçin. Yeni bir yapıt ekleyin ve ardından daha önce oluşturduğunuz **AzureML model** yapıtını ve hizmet bağlantısını seçin. Bir dağıtımı tetiklemek için modeli ve sürümü seçin:

    [![AzureML modelini seçin](media/how-to-deploy-and-where/enable-modeltrigger-artifact.png)](media/how-to-deploy-and-where/enable-modeltrigger-artifact-expanded.png)

1. Model yapıtında model tetikleyiciyi etkinleştirin. Tetikleyiciyi açtığınızda, söz konusu modelin belirtilen sürümü (yani, en yeni sürümü) çalışma alanınıza kaydedildiğinde, bir Azure DevOps sürümü işlem hattı tetiklenir.

    [![Model tetikleyiciyi etkinleştir](media/how-to-deploy-and-where/set-modeltrigger.png)](media/how-to-deploy-and-where/set-modeltrigger-expanded.png)

## <a name="next-steps"></a>Sonraki adımlar

ML modellerine yönelik sürekli dağıtım hakkında daha fazla örnek için GitHub 'da aşağıdaki projelere göz atın.

* [Microsoft/MLOps](https://github.com/Microsoft/MLOps)
* [Microsoft/MLOpsPython](https://github.com/microsoft/MLOpsPython)