---
title: Dağıtım ve tüketim
titleSuffix: ML Studio (classic) - Azure
description: Makine öğrenimi iş akışlarını ve modellerini web hizmetleri olarak dağıtmak için Azure Machine Learning Studio'yı (klasik) kullanabilirsiniz. Bu web hizmetleri daha sonra gerçek zamanlı veya toplu modda tahminler yapmak için internet üzerinden uygulamalardan makine öğrenme modelleri aramak için kullanılabilir.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 04/19/2017
ms.openlocfilehash: ff6ae0de0bbd8c47b81fa5066a97eb0b3e0cf6bc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79204401"
---
# <a name="azure-machine-learning-studio-classic-web-services-deployment-and-consumption"></a>Azure Machine Learning Studio (klasik) Web Hizmetleri: Dağıtım ve tüketim

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Makine öğrenimi iş akışlarını ve modellerini web hizmetleri olarak dağıtmak için Azure Machine Learning Studio'yı (klasik) kullanabilirsiniz. Bu web hizmetleri daha sonra gerçek zamanlı veya toplu modda tahminler yapmak için Internet üzerinden uygulamalardan makine öğrenme modelleri aramak için kullanılabilir. Web hizmetleri yeniden kullanılabilir olduğundan, bunları .NET ve Java gibi çeşitli programlama dillerinden ve platformlarından ve Excel gibi uygulamalardan arayabilirsiniz.

Sonraki bölümlerde, başlangıç yardımcı olmak için izlikler, kod ve belgelere bağlantılar sağlar.

## <a name="deploy-a-web-service"></a>Bir web hizmetini dağıtma

### <a name="with-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio ile (klasik)

Studio (klasik) portalı ve Microsoft Azure Machine Learning Web Services portalı, kod yazmadan bir web hizmetini dağıtmanıza ve yönetmenize yardımcı olur.

Aşağıdaki bağlantılar, yeni bir web hizmetinin nasıl dağıtılanöğretilen hakkında genel bilgiler sağlar:

* Azure Kaynak Yöneticisi'ni temel alan yeni bir web hizmetinin nasıl dağıtılanacağına ilişkin genel bir bakış [için](deploy-a-machine-learning-web-service.md)bkz.
* Bir web hizmetinin nasıl dağıtılanabildiğini öğrenmek için [bkz.](deploy-a-machine-learning-web-service.md)
* Bir web hizmetinin nasıl oluşturulup dağıtılanın hakkında tam bir gözden geçirme için, Öğretici 1 ile [başlayın: Kredi riskini tahmin edin.](tutorial-part1-credit-risk.md)
* Bir web hizmetini dağıtan belirli örnekler için bkz:

  * [Öğretici 3: Kredi risk modelini dağıtın](tutorial-part3-credit-risk-deploy.md)
  * [Web hizmeti birden çok bölgeye nasıl dağıtılır?](deploy-a-machine-learning-web-service.md#multi-region)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a>Web hizmetleri kaynak sağlayıcısı API'leri (Azure Kaynak Yöneticisi API'leri) ile

Web hizmetleri için Azure Machine Learning Studio (klasik) kaynak sağlayıcısı, REST API çağrılarını kullanarak web hizmetlerinin dağıtımını ve yönetimini sağlar. Daha fazla bilgi için [Machine Learning Web Service (REST)](/rest/api/machinelearning/index) referansına bakın.

<!-- [Machine Learning Web Service (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) reference. -->

### <a name="with-powershell-cmdlets"></a>PowerShell cmdlets ile

Web hizmetleri için Azure Machine Learning Studio (klasik) kaynak sağlayıcısı, PowerShell cmdlets kullanarak web hizmetlerinin dağıtımını ve yönetimini sağlar.

Cmdlets'i kullanmak için öncelikle PowerShell ortamından Azure hesabınızda [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) cmdlet'i kullanarak oturum açmanız gerekir. Kaynak Yöneticisi'ni temel alan PowerShell komutlarını nasıl arayacağınızı bilmiyorsanız, Azure [Kaynak Yöneticisi ile Azure PowerShell kullanma'ya](../../azure-resource-manager/management/manage-resources-powershell.md)bakın.

Tahmine dayalı denemenizi dışa aktarmak için [bu örnek kodu](https://github.com/ritwik20/AzureML-WebServices)kullanın. Koddan .exe dosyasını oluşturduktan sonra şunları yazabilirsiniz:

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

Uygulamanın çalıştırılsa, bir web hizmeti JSON şablonu oluşturur. Bir web hizmetini dağıtmak için şablonu kullanmak için aşağıdaki bilgileri eklemeniz gerekir:

* Depolama hesabı adı ve anahtarı

    [Azure portalından](https://portal.azure.com/)depolama hesabı adı ve anahtarı alabilirsiniz.
* Taahhüt planı kimliği

    Plan adını oturum layıp tıklayarak Plan Kimliğini [Azure Machine Learning Web Services](https://services.azureml.net) portalından alabilirsiniz.

*Bunları, MachineLearningWorkspace* düğümüyle aynı düzeydeki *Özellikler* düğümünün alt bölümü olarak JSON şablonuna ekleyin.

Bir örneği aşağıda verilmiştir:

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

Ek ayrıntılar için aşağıdaki makalelere ve örnek koda bakın:

* MSDN'de [Azure Machine Learning Studio (klasik) Cmdlets](https://docs.microsoft.com/powershell/module/az.machinelearning) başvurusu
* GitHub'da örnek [walkthrough](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt)

## <a name="consume-the-web-services"></a>Web hizmetlerini tüketin

### <a name="from-the-azure-machine-learning-web-services-ui-testing"></a>Azure Machine Learning Web Services UI(Test) gönderen

Web hizmetinizi Azure Machine Learning Web Services portalından test edebilirsiniz. Buna İstek-Yanıt hizmeti (RRS) ve Toplu Yürütme hizmeti (BES) arabirimlerinin test edilmesi dahildir.

* [Yeni bir web hizmeti dağıtma](deploy-a-machine-learning-web-service.md)
* [Azure Machine Learning web hizmeti dağıtma](deploy-a-machine-learning-web-service.md)
* [Öğretici 3: Kredi risk modelini dağıtın](tutorial-part3-credit-risk-deploy.md)

### <a name="from-excel"></a>Excel'den

Web hizmetini tüketen bir Excel şablonu indirebilirsiniz:

* [Excel'den Azure Machine Learning web hizmeti tüketme](consuming-from-excel.md)
* [Azure Machine Learning Web Hizmetleri için Excel eklentisi](excel-add-in-for-web-services.md)

### <a name="from-a-rest-based-client"></a>REST tabanlı istemciden

Azure Machine Learning Web Hizmetleri, YENIDEN Değer EBM'leridir. Bu API'leri .NET, Python, R, Java gibi çeşitli platformlardan tüketebilirsiniz. [Microsoft Azure Machine Learning Web Services portalındaki](https://services.azureml.net) web hizmetinizin **Tüketim** sayfası, başlamanıza yardımcı olabilecek örnek koda sahiptir. Daha fazla bilgi için bkz. [Azure Machine Learning web hizmetini kullanma](consume-web-services.md).
