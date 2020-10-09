---
title: Özel uç nokta yapılandırma
titleSuffix: Azure Machine Learning
description: Azure Machine Learning çalışma alanınıza sanal bir ağdan güvenli bir şekilde erişmek için Azure özel bağlantısı 'nı kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 09/30/2020
ms.openlocfilehash: 4ba7ec73ac70723e21b6acad571d62d14edd250a
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2020
ms.locfileid: "91828127"
---
# <a name="configure-azure-private-link-for-an-azure-machine-learning-workspace"></a>Azure Machine Learning çalışma alanı için Azure özel bağlantısını yapılandırma

Bu belgede, Azure özel bağlantısının Azure Machine Learning çalışma alanıyla nasıl kullanılacağını öğrenirsiniz. Azure Machine Learning için sanal ağ oluşturma hakkında daha fazla bilgi için bkz. [sanal ağ yalıtımı ve gizliliğe genel bakış](how-to-network-security-overview.md)

Azure özel bağlantısı, özel bir uç nokta kullanarak çalışma alanınıza bağlanmanızı sağlar. Özel uç nokta, sanal ağınız içindeki özel IP adresleri kümesidir. Daha sonra çalışma alanınıza erişimi yalnızca özel IP adresleri üzerinden oluşacak şekilde sınırlayabilirsiniz. Özel bağlantı, veri taşması riskini azaltmaya yardımcı olur. Özel uç noktalar hakkında daha fazla bilgi için bkz. [Azure özel bağlantı](/azure/private-link/private-link-overview) makalesi.

> [!IMPORTANT]
> Azure özel bağlantısı, çalışma alanını silme veya işlem kaynaklarını yönetme gibi Azure denetim düzlemi 'ni (yönetim işlemleri) etkilemez. Örneğin, bir işlem hedefi oluşturma, güncelleştirme veya silme. Bu işlemler, genel Internet üzerinden normal olarak gerçekleştirilir. Azure Machine Learning Studio, API 'Ler (yayınlanan işlem hatları dahil) veya SDK özel uç noktasını kullanır gibi veri düzlemi işlemleri.
>
> Mozilla Firefox kullanıyorsanız, çalışma alanınızın özel uç noktasına erişmeye çalışırken sorunlarla karşılaşabilirsiniz. Bu sorun, Mozilla 'de HTTPS üzerinden DNS ile ilişkili olabilir. Google Chrome 'un Microsoft Edge 'i geçici çözüm olarak kullanmanızı öneririz.

## <a name="prerequisites"></a>Önkoşullar

Müşteri tarafından yönetilen bir anahtarla özel bağlantı etkin bir çalışma alanı kullanmayı planlıyorsanız, bu özelliği bir destek bileti kullanarak istemeniz gerekir. Daha fazla bilgi için bkz. [kotaları yönetme ve artırma](how-to-manage-quotas.md#private-endpoint-and-private-dns-quota-increases).

## <a name="limitations"></a>Sınırlamalar

Azure Kamu bölgelerinde veya Azure Çin 21Vianet bölgelerinde özel bağlantıyla birlikte bir Azure Machine Learning çalışma alanı kullanılması kullanılamaz.

## <a name="create-a-workspace-that-uses-a-private-endpoint"></a>Özel uç nokta kullanan bir çalışma alanı oluşturma

Özel bir uç nokta ile çalışma alanı oluşturmak için aşağıdaki yöntemlerden birini kullanın:

> [!TIP]
> Azure Resource Manager şablonu gerekirse yeni bir sanal ağ oluşturabilir. Diğer yöntemlerin hepsi var olan bir sanal ağ gerektirir.

# <a name="resource-manager-template"></a>[Kaynak Yöneticisi şablonu](#tab/azure-resource-manager)

' Deki Azure Resource Manager şablonu, [https://github.com/Azure/azure-quickstart-templates/tree/master/201-machine-learning-advanced](https://github.com/Azure/azure-quickstart-templates/tree/master/201-machine-learning-advanced) özel bir uç nokta ve sanal ağ ile bir çalışma alanı oluşturmanın kolay bir yolunu sağlar.

Özel uç noktalar dahil bu şablonu kullanma hakkında daha fazla bilgi için bkz. [Azure Machine Learning için bir çalışma alanı oluşturmak üzere Azure Resource Manager şablonu kullanma](how-to-create-workspace-template.md).

# <a name="python"></a>[Python](#tab/python)

Azure Machine Learning Python SDK 'Sı, özel bir uç nokta ile bir çalışma alanı oluşturmak için [çalışma alanı. Create ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#create-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--sku--basic---tags-none--friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--adb-workspace-none--cmk-keyvault-none--resource-cmk-uri-none--hbi-workspace-false--default-cpu-compute-target-none--default-gpu-compute-target-none--private-endpoint-config-none--private-endpoint-auto-approval-true--exist-ok-false--show-output-true-) Ile kullanılabilen [privateendpointconfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.privateendpointconfig?view=azure-ml-py) sınıfını sağlar. Bu sınıf, var olan bir sanal ağ gerektirir.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[Machine Learning Için Azure CLI uzantısı](reference-azure-machine-learning-cli.md) [az ml çalışma alanı oluştur](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/workspace?view=azure-cli-latest#ext_azure_cli_ml_az_ml_workspace_create) komutunu sağlar. Bu komut için aşağıdaki parametreler, özel bir ağla çalışma alanı oluşturmak için kullanılabilir, ancak var olan bir sanal ağ gerektirir:

* `--pe-name`: Oluşturulan özel uç noktanın adı.
* `--pe-auto-approval`: Çalışma alanına özel uç nokta bağlantılarının otomatik olarak onaylanıp onaylanmayacağı.
* `--pe-resource-group`: İçinde özel uç nokta oluşturulacak kaynak grubu. Sanal ağı içeren aynı grup olmalıdır.
* `--pe-vnet-name`: İçinde özel uç nokta oluşturmak için var olan sanal ağ.
* `--pe-subnet-name`: İçinde özel uç nokta oluşturulacak alt ağın adı. Varsayılan değer: `default`.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure Machine Learning Studio 'daki __ağ__ sekmesi özel bir uç nokta yapılandırmanıza olanak tanır. Ancak, var olan bir sanal ağ gerektirir. Daha fazla bilgi için bkz. [portalda çalışma alanları oluşturma](how-to-manage-workspace.md).

---

## <a name="using-a-workspace-over-a-private-endpoint"></a>Özel bir uç nokta üzerinde çalışma alanı kullanma

Çalışma alanına yönelik iletişime yalnızca sanal ağdan izin verildiğinden, çalışma alanını kullanan tüm geliştirme ortamları sanal ağın üyesi olmalıdır. Örneğin, sanal ağdaki bir sanal makine.

> [!IMPORTANT]
> Microsoft, bağlantının geçici kesintisini önlemek için özel bağlantı etkinleştirildikten sonra çalışma alanına bağlanan makinelerde DNS önbelleğini temizlemeye öneriyor. 

Azure sanal makineleri hakkında daha fazla bilgi için bkz. [sanal makineler belgeleri](/azure/virtual-machines/).


## <a name="next-steps"></a>Sonraki adımlar

* Azure Machine Learning çalışma alanınızı güvenli hale getirme hakkında daha fazla bilgi için [sanal ağ yalıtımı ve gizliliği genel bakış](how-to-network-security-overview.md) makalesine bakın.

* Sanal ağınızda özel bir DNS çözümü kullanmayı planlıyorsanız, bkz. [Özel BIR DNS sunucusu ile çalışma alanı kullanma](how-to-custom-dns.md).
