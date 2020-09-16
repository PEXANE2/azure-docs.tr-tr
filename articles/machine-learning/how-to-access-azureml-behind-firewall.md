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
ms.custom: how-to, devx-track-python
ms.openlocfilehash: 081c07be49178be2415edccbfc2026336eb8a8a5
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90604419"
---
# <a name="use-workspace-behind-a-firewall-for-azure-machine-learning"></a>Azure Machine Learning için bir güvenlik duvarının arkasındaki çalışma alanını kullan

Bu makalede, Azure Güvenlik Duvarı 'nı Azure Machine Learning çalışma alanınıza ve genel İnternet 'e erişimi denetlemek üzere nasıl yapılandıracağınızı öğrenin.   Azure Machine Learning güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure Machine Learning Için kuruluş güvenliği](concept-enterprise-security.md)

Bu belgedeki bilgiler [Azure Güvenlik Duvarı](../firewall/tutorial-firewall-deploy-portal.md)kullanımı temel alınarak, diğer güvenlik duvarı ürünleriyle de kullanabilmeniz gerekir. Güvenlik duvarınız üzerinden iletişime izin verme hakkında sorularınız varsa, lütfen kullandığınız güvenlik duvarının belgelerine başvurun.

## <a name="application-rules"></a>Uygulama kuralları

Güvenlik duvarınızda, bu makaledeki adreslere ve adreslere giden trafiğe izin veren bir _uygulama kuralı_ oluşturun.

> [!TIP]
> Ağ kuralı eklenirken, __Protokolü__ herhangi bir ve bağlantı noktası olarak ayarlayın `*` .
>
> Azure Güvenlik duvarını yapılandırma hakkında daha fazla bilgi için bkz. [Azure Güvenlik duvarını dağıtma ve yapılandırma](../firewall/tutorial-firewall-deploy-portal.md#configure-an-application-rule).

## <a name="routes"></a>Yollar

Azure Machine Learning kaynakları içeren alt ağın giden yolunu yapılandırırken, eğitim ortamının güvenliğini sağlamak için [Zorlamalı tünel](how-to-secure-training-vnet.md#forced-tunneling) bölümündeki kılavuzu kullanın.

## <a name="microsoft-hosts"></a>Microsoft Konakları

Doğru yapılandırılmamışsa, güvenlik duvarı çalışma alanınızı kullanarak sorunlara yol açabilir. Azure Machine Learning çalışma alanı tarafından her ikisi de kullanılan birçok konak adı vardır.

Bu bölümdeki konaklar Microsoft 'a aittir ve çalışma alanınızın düzgün çalışması için gerekli hizmetleri sağlar.

| **Ana bilgisayar adı** | **Amaç** |
| ---- | ---- |
| **login.microsoftonline.com** | Kimlik Doğrulaması |
| **management.azure.com** | Çalışma alanı bilgilerini almak için kullanılır |
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
| **your-acr-server-name.azurecr.io** | Yalnızca Azure Container Registry sanal ağın arkasında ise gereklidir. Bu yapılandırmada, Microsoft ortamından aboneliğinizdeki ACR örneğine özel bir bağlantı oluşturulur. Azure Machine Learning çalışma alanınız için ACR sunucu adını kullanın. |
| **\*. notebooks.azure.net** | Azure Machine Learning Studio 'daki Not defterleri için gereklidir. |
| **graph.windows.net** | Not defterleri için gerekli |

> [!TIP]
> Federal Kimlik kullanmayı planlıyorsanız [Active Directory Federasyon Hizmetleri (AD FS) makalenin güvenliğini sağlamak Için en iyi uygulamaları](/windows-server/identity/ad-fs/deployment/best-practices-securing-ad-fs) izleyin.

## <a name="python-hosts"></a>Python Konakları

Bu bölümdeki konaklar, Python paketlerini yüklemek için kullanılır. Geliştirme, eğitim ve dağıtım sırasında gereklidir. 

| **Ana bilgisayar adı** | **Amaç** |
| ---- | ---- |
| **anaconda.com**</br>**\*. anaconda.com** | Varsayılan paketleri yüklemek için kullanılır. |
| **\*. anaconda.org** | Depo verilerini almak için kullanılır. |
| **pypi.org** | Varsa, varsayılan dizinden bağımlılıkları listelemek için kullanılır ve dizin, Kullanıcı ayarları tarafından üzerine yazılmaz. Dizinin üzerine yazılırsa, ** \* . pythonhosted.org**de izin vermeniz gerekir. |

## <a name="r-hosts"></a>R Konakları

Bu bölümdeki konaklar R paketleri yüklemek için kullanılır. Geliştirme, eğitim ve dağıtım sırasında gereklidir.

> [!IMPORTANT]
> Dahili olarak, Azure Machine Learning için R SDK, Python paketlerini kullanır. Bu nedenle, güvenlik duvarı üzerinden Python konaklarına da izin vermelisiniz.

| **Ana bilgisayar adı** | **Amaç** |
| ---- | ---- |
| **cloud.r-project.org** | CRAN paketleri yüklenirken kullanılır. |

## <a name="azure-government-region"></a>Azure Kamu bölgesi

Azure Kamu bölgeleri için gerekli URL 'Ler.

| **Ana bilgisayar adı** | **Amaç** |
| ---- | ---- |
| **usgovarizona.api.ml.azure.us** | US-Arizona bölgesi |
| **usgovvirginia.api.ml.azure.us** | ABD-Virginia bölgesi |

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Azure portalını kullanarak Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma](../firewall/tutorial-firewall-deploy-portal.md)
* [Azure Sanal Ağı içindeki Azure ML denemesinin ve çıkarım işlerinin güvenliğini sağlama](how-to-network-security-overview.md)
