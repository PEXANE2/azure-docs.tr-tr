---
title: Yeni veriler için ML işlem hattını Tetikle
titleSuffix: Azure Machine Learning
description: Azure Logic Apps kullanarak ML işlem hattının çalıştırılmasını nasıl tetikleyeceğinizi öğrenin.
services: machine-learning
author: NilsPohlmann
ms.author: nilsp
ms.service: machine-learning
ms.subservice: core
ms.workload: data-services
ms.topic: how-to
ms.date: 02/07/2020
ms.custom: contperfq4
ms.openlocfilehash: 7d6222b4ea0e30997b3b55adb887c839fef6e624
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86207423"
---
# <a name="trigger-a-run-of-a-machine-learning-pipeline-from-a-logic-app"></a>Bir mantıksal uygulamadan Machine Learning işlem hattının çalıştırılmasını tetikleme

Yeni veriler göründüğünde Azure Machine Learning işlem hattının çalıştırılmasını tetikleyin. Örneğin, BLOB depolama hesabında yeni veriler göründüğünde yeni bir modeli eğitemak üzere işlem hattını tetiklemek isteyebilirsiniz. Tetikleyiciyi [Azure Logic Apps](../logic-apps/logic-apps-overview.md)ayarlayın.

## <a name="prerequisites"></a>Önkoşullar

* Azure Machine Learning çalışma alanı. Daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md).

* Yayımlanmış bir Machine Learning işlem hattı için REST uç noktası. İşlem [hattınızı oluşturun ve yayımlayın](how-to-create-your-first-pipeline.md). Ardından, bir işlem hattı KIMLIĞI kullanarak PublishedPipeline 'nizin REST uç noktasını bulun:
    
     ```
    # You can find the pipeline ID in Azure Machine Learning studio
    
    published_pipeline = PublishedPipeline.get(ws, id="<pipeline-id-here>")
    published_pipeline.endpoint 
    ```
* Verilerinizi depolamak için [Azure Blob depolama](../storage/blobs/storage-blobs-overview.md) .
* Çalışma alanınızdaki BLOB depolama hesabınızın ayrıntılarını içeren [bir veri deposu](how-to-access-data.md) .

## <a name="create-a-logic-app"></a>Mantıksal Uygulama oluşturma

Şimdi bir [Azure Logic App](../logic-apps/logic-apps-overview.md) örneği oluşturun. İsterseniz, [bir tümleştirme hizmeti ortamı (ISE) kullanın](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) ve mantıksal uygulamanız tarafından kullanılmak üzere [müşteri tarafından yönetilen bir anahtar ayarlayın](../logic-apps/customer-managed-keys-integration-service-environment.md) .

Mantıksal uygulamanız sağlandıktan sonra, işlem hattınızda bir tetikleyiciyi yapılandırmak için aşağıdaki adımları kullanın:

1. Uygulamaya Azure Machine Learning Çalışma Alanı erişimi sağlamak için [sistem tarafından atanan bir yönetilen kimlik oluşturun](../logic-apps/create-managed-service-identity.md) .

1. Logic App Designer görünümü ' ne gidin ve boş mantıksal uygulama şablonunu seçin. 
    > [!div class="mx-imgBorder"]
    > ![Boş şablon](media/how-to-trigger-published-pipeline/blank-template.png)

1. Tasarımcıda **BLOB**' u arayın. **Bir blob eklendiğinde veya değiştirildiğinde (yalnızca Özellikler)** tetikleyicisi ' ni seçin ve bu tetikleyiciyi mantıksal uygulamanıza ekleyin.
    > [!div class="mx-imgBorder"]
    > ![Tetikleyici ekleme](media/how-to-trigger-published-pipeline/add-trigger.png)

1. Blob ekleme veya değişiklik için izlemek istediğiniz BLOB depolama hesabı için bağlantı bilgilerini girin. İzlenecek kapsayıcıyı seçin. 
 
    Sizin için çalışan güncelleştirmeleri yoklamaya yönelik **aralığı** ve **sıklığı** seçin.  

    > [!NOTE]
    > Bu tetikleyici seçili kapsayıcıyı izler, ancak alt klasörleri izlemez.

1. Yeni veya değiştirilmiş bir blob algılandığında çalışacak bir HTTP eylemi ekleyin. **+ Yeni adım**' ı seçin ve ardından http eylemini arayın ve seçin.

  > [!div class="mx-imgBorder"]
  > ![HTTP eylemi ara](media/how-to-trigger-published-pipeline/search-http.png)

  Eyleminizi yapılandırmak için aşağıdaki ayarları kullanın:

  | Ayar | Değer | 
  |---|---|
  | HTTP eylemi | POST |
  | URI |bir [Önkoşul](#prerequisites) olarak bulduğunuz yayınlanan işlem hattının bitiş noktası |
  | Kimlik doğrulaması modu | Yönetilen Kimlik |

1. Sahip olduğunuz herhangi bir [DataPath Pipelineparametrelerinin](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-showcasing-datapath-and-pipelineparameter.ipynb) değerini ayarlamak için zamanlamanızı ayarlayın:

    ```json
    "DataPathAssignments": { 
         "input_datapath": { 
                            "DataStoreName": "<datastore-name>", 
                            "RelativePath": "@triggerBody()?['Name']" 
    } 
    }, 
    "ExperimentName": "MyRestPipeline", 
    "ParameterAssignments": { 
    "input_string": "sample_string3" 
    },
    ```

    `DataStoreName`Çalışma alanınıza bir [Önkoşul](#prerequisites)olarak eklediğiniz öğesini kullanın.
     
    > [!div class="mx-imgBorder"]
    > ![HTTP ayarları](media/how-to-trigger-published-pipeline/http-settings.png)

1. **Kaydet** ' i seçin ve zamanlamanız artık hazırdır.
