---
title: Güvenlik duvarı kullanma
titleSuffix: Azure Machine Learning
description: Azure Güvenlik duvarları ile Azure Machine Learning çalışma alanlarına erişimi denetleyin. Azure Machine Learning düzgün çalışması için güvenlik duvarı üzerinden izin vermeniz gereken konaklar hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 04/27/2020
ms.openlocfilehash: 22136efc493b4838a8f538091d6272e3c167510e
ms.sourcegitcommit: b55d1d1e336c1bcd1c1a71695b2fd0ca62f9d625
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84434724"
---
# <a name="use-workspace-behind-azure-firewall-for-azure-machine-learning"></a>Azure Machine Learning için Azure Güvenlik duvarının arkasındaki çalışma alanını kullanın

Bu makalede, Azure Güvenlik duvarını bir Azure Machine Learning çalışma alanıyla kullanılmak üzere nasıl yapılandıracağınızı öğrenin.

Azure Güvenlik Duvarı, Azure Machine Learning çalışma alanınıza ve genel İnternet 'e erişimi denetlemek için kullanılabilir. Doğru yapılandırılmamışsa, güvenlik duvarı çalışma alanınızı kullanarak sorunlara yol açabilir.

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
| **\*. azureml.ms** | Azure Machine Learning API 'Leri tarafından kullanılır |
| **\*. experiments.azureml.net** | Azure Machine Learning çalıştıran denemeleri tarafından kullanılır|
| **\*. modelmanagement.azureml.net** | Modelleri kaydetmek ve dağıtmak için kullanılır|
| **mlworkspace.azure.ai** | Bir çalışma alanı görüntülenirken Azure portal tarafından kullanılır |
| **\*. aether.ms** | Azure Machine Learning işlem hatları çalıştırılırken kullanılır |
| **\*. instances.azureml.net** | Azure Machine Learning işlem örnekleri |
| **windows.net** | Azure Blob Depolama |
| **vault.azure.net** | Azure Key Vault |
| **microsoft.com** | Temel Docker görüntüleri |
| **azurecr.io** | Azure Container Registry |

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
