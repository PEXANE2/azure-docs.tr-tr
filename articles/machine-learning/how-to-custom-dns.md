---
title: Özel DNS sunucusu kullan
titleSuffix: Azure Machine Learning
description: Özel bir DNS sunucusunu bir Azure Machine Learning çalışma alanıyla ve özel uç noktayla çalışacak şekilde yapılandırma.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.author: jhirono
author: jhirono
ms.date: 10/05/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 1d215c9564d89e5bd410e68839807f5c2c752356
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2020
ms.locfileid: "91828635"
---
# <a name="how-to-use-your-workspace-with-a-custom-dns-server"></a>Özel bir DNS sunucusu ile çalışma alanınızı kullanma

Bir sanal ağla Azure Machine Learning kullanırken, [DNS ad çözümlemesini işlemenin birkaç yolu](/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances)vardır. Varsayılan olarak, Azure, çalışma alanınız ve özel uç noktanız için ad çözümlemesini otomatik olarak işler. Ancak, __kendi özel DNS sunucunuzu kullanırken__, çalışma alanı için el ile DNS girdileri oluşturmanız gerekir.

> [!IMPORTANT]
> Bu makalede, bu girdilerin yalnızca tam etki alanı adını (FQDN) ve IP adreslerini bulmak için DNS kayıtlarını yapılandırma hakkında bilgi sağlamayan bilgiler yer alır. Kayıt ekleme hakkında bilgi edinmek için DNS yazılımınızın belgelerine bakın.

## <a name="prerequisites"></a>Önkoşullar

- [Kendı DNS sunucunuzu](/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server)kullanan bir Azure sanal ağı.

- Özel uç noktası olan bir Azure Machine Learning çalışma alanı. Daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md).

- [Eğitim & çıkarımı sırasında ağ yalıtımının](how-to-enable-virtual-network.md)kullanılmasıyla ilgili benzerlik.

- İsteğe bağlı olarak, [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) veya [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

## <a name="find-the-ip-addresses"></a>IP adreslerini bulma

Aşağıdaki liste, çalışma alanınız ve özel uç nokta tarafından kullanılan tam etki alanı adlarını (FQDN) içerir:

* `<workspace-GUID>.workspace.<region>.api.azureml.ms`
* `<workspace-GUID>.studio.workspace.<region>.api.azureml.ms`
* `cert-<workspace-GUID>.workspace.<region>.api.azureml.ms`
* `<workspace-GUID>.workspace.<region>.experiments.azureml.net`
* `<workspace-GUID>.workspace.<region>.modelmanagement.azureml.net`
* `<workspace-GUID>.workspace.<region>.aether.ms`
* `ml-<workspace-name>-<region>-<workspace-guid>.notebooks.azure.ml`
* Bir işlem örneği oluşturursanız, için bir giriş de eklemeniz gerekir `<instance-name>.<region>.instances.azureml.ms` .

VNet 'teki FQDN 'lerin iç IP adreslerini bulmak için aşağıdaki yöntemlerden birini kullanın:

> [!NOTE]
> Tam etki alanı adları ve IP adresleri, yapılandırmanıza göre farklı olacaktır. Örneğin, etki alanı adı içindeki GUID değeri çalışma alanınıza özgü olacaktır.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az network private-endpoint show --endpoint-name <endpoint> --resource-group <resource-group> --query 'customDnsConfigs[*].{FQDN: fqdn, IPAddress: ipAddresses[0]}' --output table
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell
$workspaceDns=Get-AzPrivateEndpoint -Name <endpoint> -resourcegroupname <resource-group>
$workspaceDns.CustomDnsConfigs | format-table
```

# <a name="azure-portal"></a>[Azure portalındaki](#tab/azure-portal)

1. [Azure Portal](https://portal.azure.com), Azure Machine Learning __çalışma alanınızı__seçin.
1. __Ayarlar__ bölümünde, __Özel uç nokta bağlantıları__' nı seçin.
1. Görüntülenen __Özel uç nokta__ sütunundaki bağlantıyı seçin.
1. Çalışma alanı özel uç noktası için tam etki alanı adlarının (FQDN) ve IP adreslerinin listesi sayfanın en altında bulunur.

:::image type="content" source="./media/how-to-custom-dns/private-endpoint-custom-dns.png" alt-text="Portalda FQDN 'lerin listesi":::

---

Tüm yöntemlerden döndürülen bilgiler aynıdır; kaynaklar için FQDN ve özel IP adresi listesi.

| FQDN | IP Adresi |
| ----- | ----- |
| `fb7e20a0-8891-458b-b969-55ddb3382f51.workspace.eastus.api.azureml.ms` | `10.1.0.5` |
| `fb7e20a0-8891-458b-b969-55ddb3382f51.studio.workspace.eastus.api.azureml.ms` | `10.1.0.5` |
| `cert-fb7e20a0-8891-458b-b969-55ddb3382f51.workspace.eastus.api.azureml.ms` | `10.1.0.5` |
| `ml-myworkspace-eastus-fb7e20a0-8891-458b-b969-55ddb3382f51.notebooks.azure.net` | `10.1.0.6` |

> [!IMPORTANT]
> Bazı FQDN 'Ler özel uç nokta tarafından listelenmemektedir, ancak çalışma alanı için gereklidir. Bu FQDN 'Ler aşağıdaki tabloda listelenmiştir ve DNS sunucunuza da eklenmelidir:
>
> * `<workspace-GUID>.workspace.<region>.experiments.azureml.net`
> * `<workspace-GUID>.workspace.<region>.modelmanagement.azureml.net`
> * `<workspace-GUID>.workspace.<region>.aether.ms`
> * İşlem örneğiniz varsa, öğesini kullanın; `<instance-name>.<region>.instances.azureml.ms` burada, `<instance-name>` işlem örneğinizin adıdır.
>
> Bu IP adreslerinin tümü için, `*.api.azureml.ms` önceki adımlardan döndürülen girişlerle aynı adresi kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Sanal Ağla Azure Machine Learning kullanma hakkında daha fazla bilgi için bkz. [sanal ağa genel bakış](how-to-network-security-overview.md).