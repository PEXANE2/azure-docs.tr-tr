---
title: Dağıtım ve tüketim
titleSuffix: ML Studio (classic) - Azure
description: Makine öğrenimi iş akışlarını ve modellerini Web Hizmetleri olarak dağıtmak için Azure Machine Learning Studio (klasik) kullanabilirsiniz. Bu web Hizmetleri, ardından, makine öğrenme modellerini uygulamalardan gerçek zamanlı olarak veya toplu iş modunda tahminleri yapmak için internet üzerinden çağırmak için de kullanılabilir.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 04/19/2017
ms.openlocfilehash: 7216d2f97a52798d2609073761eb8f4a2ce9024d
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75976123"
---
# <a name="azure-machine-learning-studio-classic-web-services-deployment-and-consumption"></a>Azure Machine Learning Studio (klasik) Web Hizmetleri: dağıtım ve tüketim

Makine öğrenimi iş akışlarını ve modellerini Web Hizmetleri olarak dağıtmak için Azure Machine Learning Studio (klasik) kullanabilirsiniz. Bu web Hizmetleri, ardından, makine öğrenme modellerini uygulamalardan gerçek zamanlı olarak veya toplu iş modunda tahminleri yapmak için Internet üzerinden çağırmak için de kullanılabilir. RESTful web hizmetleri olduğu için bunları çeşitli programlama dilleri ve platformları, .NET ve Java gibi ve Excel gibi uygulamaları çağırabilirsiniz.

Sonraki bölümlerde, kılavuzlar, kod ve başlamanıza yardımcı olan belgeler için bağlantılar sağlar.

## <a name="deploy-a-web-service"></a>Bir web hizmetini dağıtma

### <a name="with-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio (klasik) ile

Studio (klasik) portalı ve Microsoft Azure Machine Learning Web Hizmetleri portalı, kod yazmadan bir Web hizmetini dağıtmanıza ve yönetmenize yardımcı olur.

Aşağıdaki bağlantılar, yeni bir web hizmeti dağıtma hakkında genel bilgiler sağlar:

* Azure Resource Manager'a bağlı yeni bir web hizmeti dağıtma hakkında genel bir bakış için bkz. [yeni bir web hizmetini dağıtma](deploy-a-machine-learning-web-service.md).
* Bir web hizmeti dağıtma hakkında kılavuz için bkz. [bir Azure Machine Learning web hizmetini dağıtma](deploy-a-machine-learning-web-service.md).
* Bir Web hizmeti oluşturma ve dağıtma hakkında tam bir anlatım için [öğretici 1: kredi riskini tahmin](tutorial-part1-credit-risk.md)edin ' i kullanmaya başlayın.
* Bir web hizmetini dağıtma belirli örnekler için bkz:

  * [Öğretici 3: Kredi risk modelini dağıtma](tutorial-part3-credit-risk-deploy.md)
  * [Bir web hizmetini birden fazla bölgeye dağıtma](deploy-a-machine-learning-web-service.md#multi-region)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a>Web Hizmetleri kaynak sağlayıcı API'leri (Azure Resource Manager API'leri)

Web Hizmetleri için Azure Machine Learning Studio (klasik) kaynak sağlayıcısı, REST API çağrılarını kullanarak Web hizmetlerinin dağıtılmasını ve yönetilmesine izin vermez. Daha fazla bilgi için [Machine Learning Web hizmeti (REST)](/rest/api/machinelearning/index) başvurusuna bakın.

<!-- [Machine Learning Web Service (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) reference. -->

### <a name="with-powershell-cmdlets"></a>PowerShell cmdlet'leri ile

Web Hizmetleri için Azure Machine Learning Studio (klasik) kaynak sağlayıcısı, PowerShell cmdlet 'lerini kullanarak Web hizmetlerinin dağıtımına ve yönetimine izin vermez.

Cmdlet 'lerini kullanmak için öncelikle [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) cmdlet 'Ini kullanarak PowerShell ortamında Azure hesabınızda oturum açmanız gerekir. Resource Manager temel alan PowerShell komutlarını çağıran nasıl aralıklarıyla bkz [Azure PowerShell kullanarak Azure Resource Manager ile](../../azure-resource-manager/management/manage-resources-powershell.md).

Tahmine dayalı denemenizi dışarı aktarmak için kullanın [Bu örnek kod](https://github.com/ritwik20/AzureML-WebServices). Koddan .exe dosyasını oluşturduktan sonra şunu yazabilirsiniz:

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

Uygulamayı çalıştıran bir web hizmeti JSON şablonu oluşturur. Bir web hizmetini dağıtmak için şablonu kullanmak için aşağıdaki bilgileri eklemeniz gerekir:

* Depolama hesabı adı ve anahtarı

    Depolama hesabı adını alın ve gelen anahtar [Azure portalında](https://portal.azure.com/).
* Taahhüt plan kimliği

    Planı Kimliğinden alabilirsiniz [Azure Machine Learning Web Hizmetleri](https://services.azureml.net) portalı oturum açma ve bir plan adı tıklatarak.

JSON şablonu altı olarak eklemediğiniz *özellikleri* düğüm aynı düzeyde *MachineLearningWorkspace* düğümü.

Bir örneği aşağıda verilmiştir:

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

Ek ayrıntılar için örnek kod ve aşağıdaki makalelere bakın:

* MSDN 'de [Azure Machine Learning Studio (klasik) cmdlet](https://docs.microsoft.com/powershell/module/az.machinelearning) başvurusu
* Örnek [izlenecek](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) github'da

## <a name="consume-the-web-services"></a>Web hizmetlerini kullanma

### <a name="from-the-azure-machine-learning-web-services-ui-testing"></a>Azure Machine Learning Web Hizmetleri kullanıcı Arabirimi (test)

Azure Machine Learning Web Hizmetleri portalında web hizmetini test edebilirsiniz. Toplu yürütme hizmeti (BES) arabirimleri ve bu istek-yanıt hizmeti (RRS) test içerir.

* [Yeni bir web hizmeti dağıtma](deploy-a-machine-learning-web-service.md)
* [Bir Azure Machine Learning web hizmetini dağıtma](deploy-a-machine-learning-web-service.md)
* [Öğretici 3: Kredi risk modelini dağıtma](tutorial-part3-credit-risk-deploy.md)

### <a name="from-excel"></a>Excel'den

Web hizmeti bir Excel şablonu indirebilirsiniz:

* [Bir Azure Machine Learning web hizmetini Excel'den kullanma](consuming-from-excel.md)
* [Azure Machine Learning Web Hizmetleri için Excel Eklentisi](excel-add-in-for-web-services.md)

### <a name="from-a-rest-based-client"></a>REST tabanlı bir istemciden

Azure Machine Learning Web Hizmetleri, RESTful apı'lerdir. Bu API 'Leri .NET, Python, R, Java vb. gibi çeşitli platformlarda kullanabilirsiniz. Web hizmetiniz için [Microsoft Azure Machine Learning Web Hizmetleri portalında](https://services.azureml.net) **kullanma sayfasında,** başlamanıza yardımcı olabilecek örnek kod bulunur. Daha fazla bilgi için bkz. [Azure Machine Learning web hizmetini kullanma](consume-web-services.md).
