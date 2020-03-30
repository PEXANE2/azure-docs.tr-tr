---
title: Bir Logic App'ten ML boru hattının çalışmasını tetikle
titleSuffix: Azure Machine Learning
description: Azure Logic Apps'ı kullanarak ML ardışık bir boru hattının çalışmasını nasıl tetiklediğinizi öğrenin.
services: machine-learning
author: sanpil
ms.author: sanpil
ms.service: machine-learning
ms.subservice: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 02/07/2020
ms.openlocfilehash: 6bb976b8b310fb3eb4d0247a8d745599f688d7b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77122863"
---
# <a name="trigger-a-run-of-a-machine-learning-pipeline-from-a-logic-app"></a>Bir Mantık Uygulamasından Machine Learning ardışık hattının çalışmasını tetikleme

Yeni veriler göründüğünde Azure Machine Learning Pipeline'ınızın çalışmasını tetikle. Örneğin, blob depolama hesabında yeni veriler göründüğünde yeni bir model eğitmek için ardışık hattı tetiklemek isteyebilirsiniz. [Azure Logic Apps](../logic-apps/logic-apps-overview.md)ile tetikleyiciyi ayarlayın.

## <a name="prerequisites"></a>Ön koşullar

* Azure Machine Learning çalışma alanı. Daha fazla bilgi için [bkz.](how-to-manage-workspace.md)

* Yayınlanan bir Machine Learning boru hattı için REST bitiş noktası. [Ardışık hattınızı oluşturun ve yayımlayın.](how-to-create-your-first-pipeline.md) Ardından, pipeline ID'yi kullanarak PublishedPipeline'ınızın REST bitiş noktasını bulun:
    
     ```
    # You can find the pipeline ID in Azure Machine Learning studio
    
    published_pipeline = PublishedPipeline.get(ws, id="<pipeline-id-here>")
    published_pipeline.endpoint 
    ```
* Verilerinizi depolamak için [azure blob depolama.](../storage/blobs/storage-blobs-overview.md)
* Çalışma alanınızda blob depolama hesabınızın ayrıntılarını içeren bir [veri deposu.](how-to-access-data.md)

## <a name="create-a-logic-app"></a>Mantıksal Uygulama oluşturma

Şimdi bir [Azure Mantık Uygulaması](../logic-apps/logic-apps-overview.md) örneği oluşturun. İsterseniz, [bir entegrasyon hizmeti ortamı (ISE) kullanın](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) ve Logic App tarafından kullanılmak üzere [müşteri tarafından yönetilen bir anahtar ayarlayın.](../logic-apps/customer-managed-keys-integration-service-environment.md)

Mantık Uygulamanız sağlandıktan sonra, ardınız için bir tetikleyici yapılandırmak için şu adımları kullanın:

1. Uygulamanın Azure Machine Learning Çalışma Alanınıza erişmesini sağlamak için [sistem tarafından atanmış yönetilen bir kimlik oluşturun.](../logic-apps/create-managed-service-identity.md)

1. Logic App Designer görünümüne gidin ve Boş Mantık Uygulaması şablonuna gidin. 
    > [!div class="mx-imgBorder"]
    > ![Boş şablon](media/how-to-trigger-published-pipeline/blank-template.png)

1. Tasarımcı, **blob**için arama . Ne **zaman blob eklenir veya değiştirilir (yalnızca özellikleri)** tetikleyicisini seçin ve bu tetikleyiciyi Mantık Uygulamanıza ekleyin.
    > [!div class="mx-imgBorder"]
    > ![Tetikleyici ekleme](media/how-to-trigger-published-pipeline/add-trigger.png)

1. Blob eklemeleri veya değişiklikleri için izlemek istediğiniz Blob depolama hesabının bağlantı bilgilerini doldurun. İzlenecek Kapsayıcı'yı seçin. 
 
    Sizin için çalışan güncelleştirmeleri yoklamak için **Aralık** ve **Sıklık'ı** seçin.  

    > [!NOTE]
    > Bu tetikleyici seçili Kapsayıcıyı izler, ancak alt klasörleri izlemez.

1. Yeni veya değiştirilmiş bir leke algılandığında çalışacak bir HTTP eylemi ekleyin. **+ Yeni Adım'ı**seçin, ardından http eylemini arayın ve seçin.

  > [!div class="mx-imgBorder"]
  > ![HTTP eylemini ara](media/how-to-trigger-published-pipeline/search-http.png)

  Eyleminizi yapılandırmak için aşağıdaki ayarları kullanın:

  | Ayar | Değer | 
  |---|---|
  | HTTP eylem | POST |
  | URI |[bir Önkoşul](#prerequisites) olarak bulduğunuz yayınlanan ardışık nokta |
  | Kimlik doğrulaması modu | Yönetilen Kimlik |

1. Sahip olabileceğiniz [DataPath PipelineParametrelerinin](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-showcasing-datapath-and-pipelineparameter.ipynb) değerini ayarlamak için zamanlamanızı ayarlayın:

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

    Çalışma `DataStoreName` alanınıza eklediğiniz inizi [Önkoşul](#prerequisites)olarak kullanın.
     
    > [!div class="mx-imgBorder"]
    > ![HTTP ayarları](media/how-to-trigger-published-pipeline/http-settings.png)

1. **Kaydet'i** seçin ve zamanlamanız artık hazır.