---
title: Özel Python paketlerini kullanma
titleSuffix: Azure Machine Learning
description: Özel Python paketlerine Azure Machine Learning ortamlarından güvenli bir şekilde erişin.
services: machine-learning
author: rastala
ms.author: roastala
ms.reviewer: laobri
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 07/10/2020
ms.openlocfilehash: 314f6a45bf688125e79f0b8ce0099a8326b339dc
ms.sourcegitcommit: 648c8d250106a5fca9076a46581f3105c23d7265
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88958159"
---
# <a name="use-private-python-packages-with-azure-machine-learning"></a>Azure Machine Learning ile özel Python paketleri kullanma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning içinde özel Python paketlerini güvenli bir şekilde kullanmayı öğrenin. Özel Python paketleri için kullanım örnekleri şunları içerir:

 * Herkese açık bir şekilde paylaştırmak istemediğiniz bir özel paket geliştirdiniz.
 * Kurumsal güvenlik duvarı içinde depolanan paketlerin seçkin bir deposunu kullanmak istiyorsunuz.

Önerilen yaklaşım, tek bir Azure Machine Learning çalışma alanı için birkaç pakete veya bir kuruluştaki tüm çalışma alanları için tüm paket deposuna sahip olmanıza bağlı olarak değişir.

Özel paketler, [ortam](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment) sınıfı aracılığıyla kullanılır. Bir ortam içinde, özel olanlar dahil olmak üzere hangi Python paketlerinin kullanılacağını bildirirsiniz. Azure Machine Learning ortam hakkında genel bilgi edinmek için bkz. [ortamları kullanma](how-to-use-environments.md). 

## <a name="prerequisites"></a>Önkoşullar

 * [Python için Azure Machine Learning SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
 * [Azure Machine Learning çalışma alanı](how-to-manage-workspace.md)

## <a name="use-small-number-of-packages-for-development-and-testing"></a>Geliştirme ve test için az sayıda paket kullanın

Tek bir çalışma alanı için az sayıda özel paket için, statik [`Environment.add_private_pip_wheel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#add-private-pip-wheel-workspace--file-path--exist-ok-false-) yöntemi kullanın. Bu yaklaşım, çalışma alanına hızlıca özel bir paket eklemenize olanak sağlar ve geliştirme ve test amaçlarıyla idealdir.

Dosya yolu bağımsız değişkenini yerel bir tekerlek dosyasına getirin ve ```add_private_pip_wheel``` komutunu çalıştırın. Komutu, çalışma alanınızdaki paketin konumunu izlemek için kullanılan bir URL döndürür. Depolama URL 'sini yakalayın ve `add_pip_package()` metodunu geçirin.

```python
whl_url = Environment.add_private_pip_wheel(workspace=ws,file_path = "my-custom.whl")
myenv = Environment(name="myenv")
conda_dep = CondaDependencies()
conda_dep.add_pip_package(whl_url)
myenv.python.conda_dependencies=conda_dep
```

Dahili olarak, Azure Machine Learning hizmet URL 'YI güvenli SAS URL 'si ile değiştirir, bu nedenle tekerlek dosyanız özel ve güvenli tutulur.

## <a name="use-a-repository-of-packages-from-azure-devops-feed"></a>Azure DevOps akışından paket deposu kullanma

Machine Learning uygulamanız için Python paketlerini etkin bir şekilde geliştiriyorsanız, bunları bir Azure DevOps deposunda yapıtlar olarak barındırabilir ve akış olarak yayımlayabilirsiniz. Bu yaklaşım, Azure Machine Learning Çalışma Alanı paket oluşturmak için DevOps iş akışını tümleştirmenize olanak sağlar. Azure DevOps kullanarak Python akışlarını ayarlamayı öğrenmek için [Azure Artifacts 'Da Python paketlerini kullanmaya başlayın](https://docs.microsoft.com/azure/devops/artifacts/quickstarts/python-packages?view=azure-devops) makalesini okuyun

Bu yaklaşım, depoya karşı kimlik doğrulaması yapmak için kişisel erişim belirtecini kullanır. Aynı yaklaşım, özel GitHub depoları gibi belirteç tabanlı kimlik doğrulaması olan diğer depolar için de geçerlidir. 

 1. Azure DevOps örneğiniz için [bir kişisel erişim belirteci (Pat) oluşturun](https://docs.microsoft.com/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&tabs=preview-page#create-a-pat) . Belirtecin kapsamını __paketlemek > okundu__olarak ayarlayın. 

 2. Workspace [. set_connection](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#set-connection-name--category--target--authtype--value-) yöntemini kullanarak Azure DEVOPS URL 'SINI ve Pat 'yi çalışma alanı özellikleri olarak ekleyin.

     ```python
    from azureml.core import Workspace
    
    pat_token = input("Enter secret token")
    ws = Workspace.from_config()
    ws.set_connection(name="connection-1", 
        category = "PythonFeed",
        target = "https://<my-org>.pkgs.visualstudio.com", 
        authType = "PAT", 
        value = pat_token) 
     ```

 3. Azure Machine Learning ortamı oluşturun ve akıştan Python paketleri ekleyin.
    
    ```python
    from azureml.core import Environment
    from azureml.core.conda_dependencies import CondaDependencies
    
    env = Environment(name="my-env")
    cd = CondaDependencies()
    cd.add_pip_package("<my-package>")
    cd.set_pip_option("--extra-index-url https://<my-org>.pkgs.visualstudio.com/<my-project>/_packaging/<my-feed>/pypi/simple")
    env.python.conda_dependencies=cd
    ```

Ortam artık eğitim çalıştırmaları veya Web hizmeti uç noktası dağıtımlarında kullanılmak üzere hazırdır. Ortamı oluştururken Azure Machine Learning hizmeti, eşleşen temel URL ile akışa karşı kimlik doğrulaması yapmak için PAT 'yi kullanır.

## <a name="use-a-repository-of-packages-from-private-storage"></a>Özel depolamadan paket havuzu kullan

Paketleri kuruluşunuzun güvenlik duvarında bir Azure Storage hesabından kullanabilirsiniz. Depolama hesabı, seçkin bir paket kümesini veya genel kullanıma açık paketlerin iç yansıtmasını tutabilir.

Bu tür özel depolamayı ayarlamak için:

1. [Çalışma alanını bir sanal ağ (VNet) Içine yerleştirin](how-to-enable-virtual-network.md).
1. Bir depolama hesabı oluşturun ve [genel erişime izin vermeyin](https://docs.microsoft.com/azure/storage/common/storage-network-security).
1. Kullanmak istediğiniz Python paketlerini depolama hesabı içindeki bir kapsayıcıya yerleştirin 
1. [Çalışma alanı VNet 'ten depolama hesabı erişimine izin ver](https://docs.microsoft.com/azure/storage/common/storage-network-security#grant-access-from-a-virtual-network)
1. [Sanal ağın arkasındaki çalışma alanı için Azure Container Registry (ACR) koyun](how-to-enable-virtual-network.md#azure-container-registry).

    > [!IMPORTANT]
    > Özel paket deposunu kullanarak modeller eğitmek veya dağıtmak için bu adımı tamamlamalısınız.

Bu konfigürasyonları tamamladıktan sonra, Azure Machine Learning ortam tanımındaki paketlere Azure Blob depolamada tam URL 'SI ile başvurabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

 * [Azure Machine Learning 'de kurumsal güvenlik](concept-enterprise-security.md) hakkında daha fazla bilgi edinin
