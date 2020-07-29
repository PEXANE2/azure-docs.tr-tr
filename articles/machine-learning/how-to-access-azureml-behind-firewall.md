---
title: Güvenlik duvarı kullanma
titleSuffix: Azure Machine Learning
description: Azure Güvenlik duvarları ile Azure Machine Learning çalışma alanlarına erişimi denetleyin. Azure Machine Learning düzgün çalışması için güvenlik duvarı üzerinden izin vermeniz gereken konaklar hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 07/17/2020
ms.custom: how-to, tracking-python
ms.openlocfilehash: 75e2ffcad4caeb6500f05ce3801797084c18a043
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87321658"
---
# <a name="use-workspace-behind-azure-firewall-for-azure-machine-learning"></a>Azure Machine Learning için Azure Güvenlik duvarının arkasındaki çalışma alanını kullanın

Bu makalede, Azure Güvenlik duvarını bir Azure Machine Learning çalışma alanıyla kullanılmak üzere nasıl yapılandıracağınızı öğrenin.

Azure Güvenlik Duvarı, Azure Machine Learning çalışma alanınıza ve genel İnternet 'e erişimi denetlemek için kullanılabilir. Doğru yapılandırılmamışsa, güvenlik duvarı çalışma alanınızı kullanarak sorunlara yol açabilir. Hem Azure Machine Learning çalışma alanı tarafından hem de bu makalede açıklanan birçok konak adı vardır.

## <a name="network-rules"></a>Ağ kuralları

Güvenlik duvarınızda, bu makaledeki adreslere ve adreslere yönelik trafiğe izin veren bir ağ kuralı oluşturun.

> [!TIP]
> Ağ kuralı eklenirken, __Protokolü__ herhangi bir ve bağlantı noktası olarak ayarlayın `*` .
>
> Azure Güvenlik duvarını yapılandırma hakkında daha fazla bilgi için bkz. [Azure Güvenlik duvarını dağıtma ve yapılandırma](../firewall/tutorial-firewall-deploy-portal.md#configure-a-network-rule).

## <a name="microsoft-hosts"></a>Microsoft Konakları

Bu bölümdeki konaklar Microsoft 'a aittir ve çalışma alanınızın düzgün çalışması için gerekli hizmetleri sağlar.

| **Konak adı** | **Amaç** |
| ---- | ---- |
| **\*. batchai.core.windows.net** | Eğitim kümeleri |
| **ml.azure.com** | Azure Machine Learning Studio |
| **default.exp-tas.com** | Azure Machine Learning Studio tarafından kullanılır |
| **\*. azureml.ms** | Azure Machine Learning API 'Leri tarafından kullanılır |
| **\*. experiments.azureml.net** | Azure Machine Learning çalıştıran denemeleri tarafından kullanılır |
| **\*. modelmanagement.azureml.net** | Modelleri kaydetmek ve dağıtmak için kullanılır|
| **mlworkspace.azure.ai** | Bir çalışma alanı görüntülenirken Azure portal tarafından kullanılır |
| **\*. aether.ms** | Azure Machine Learning işlem hatları çalıştırılırken kullanılır |
| **\*. instances.azureml.net** | Azure Machine Learning işlem örnekleri |
| **\*. instances.azureml.ms** | Çalışma alanında özel bağlantı etkinleştirildiğinde işlem örneklerinin Azure Machine Learning |
| **windows.net** | Azure Blob Depolama |
| **vault.azure.net** | Azure Key Vault |
| **azurecr.io** | Azure Container Registry |
| **mcr.microsoft.com** | Temel Docker görüntüleri için Microsoft Container Registry |

## <a name="python-hosts"></a>Python Konakları

Bu bölümdeki konaklar, Python paketlerini yüklemek için kullanılır. Geliştirme, eğitim ve dağıtım sırasında gereklidir. 

| **Konak adı** | **Amaç** |
| ---- | ---- |
| **anaconda.com** | Conda paketleri yüklenirken kullanılır |
| **pypi.org** | PIP paketleri yüklenirken kullanılır |


## <a name="r-hosts"></a>R Konakları

Bu bölümdeki konaklar R paketleri yüklemek için kullanılır. Geliştirme, eğitim ve dağıtım sırasında gereklidir.

> [!IMPORTANT]
> Dahili olarak, Azure Machine Learning için R SDK, Python paketlerini kullanır. Bu nedenle, güvenlik duvarı üzerinden Python konaklarına da izin vermelisiniz.

| **Konak adı** | **Amaç** |
| ---- | ---- |
| **cloud.r-project.org** | CRAN paketleri yüklenirken kullanılır. |

Sonraki adımlar

* [[Azure Güvenlik duvarını dağıtma ve yapılandırma](../firewall/tutorial-firewall-deploy-portal.md)]
* [Azure sanal ağı içindeki Azure ML deneme ve çıkarım işlerinin güvenliğini sağlama](how-to-enable-virtual-network.md)
