---
title: Azure CLı kullanarak VM uzantısını etkinleştirme
description: Bu makalede, Azure CLı kullanarak hibrit bulut ortamlarında çalışan Azure Arc etkin sunucularına sanal makine uzantılarının nasıl dağıtılacağı açıklanır.
ms.date: 11/06/2020
ms.topic: conceptual
ms.custom: devx-track-azurecli
ms.openlocfilehash: 59c984f4adaec2261d1b08748aa5a91c8246418d
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2020
ms.locfileid: "94359124"
---
# <a name="enable-azure-vm-extensions-using-the-azure-cli"></a>Azure CLı kullanarak Azure VM uzantılarını etkinleştirme

Bu makalede, Azure Arc etkin sunucuları tarafından desteklenen Azure VM uzantıları 'nı Azure CLı kullanan bir Linux veya Windows karma makinesine dağıtma ve kaldırma işlemi gösterilmektedir.

[!INCLUDE [Azure CLI Prepare your environment](../../../includes/azure-cli-prepare-your-environment.md)]

## <a name="enable-extension"></a>Uzantıyı etkinleştir

Yay etkin sunucunuzda bir VM uzantısını etkinleştirmek için [az connectedmachine makine uzantısı oluştur](/cli/azure/ext/connectedmachine/connectedmachine/machine-extension#ext_connectedmachine_az_connectedmachine_machine_extension_create) ' u,,,, `--machine-name` `--extension-name` `--location` `--type` `settings` ve `--publisher` parametreleriyle kullanın.

Aşağıdaki örnek, bir yay etkin Linux sunucusunda Log Analytics VM uzantısını sunar:

```azurecli
az connectedmachine machine-extension create --machine-name "myMachineName" --name "OmsAgentforLinux" --location "eastus" --type "CustomScriptExtension" --publisher "Microsoft.EnterpriseCloud.Monitoring" --settings "{\"workspaceId\":\"workspaceId"}" --protected-settings "{\workspaceKey\":"\workspaceKey"} --type-handler-version "1.10" --resource-group "myResourceGroup"
```

Aşağıdaki örnek, bir yay etkin sunucusunda özel betik uzantısının etkinleştirilmesini sunar:

```azurecli
az connectedmachine machine-extension create --machine-name "myMachineName" --name "CustomScriptExtension" --location "eastus" --type "CustomScriptExtension" --publisher "Microsoft.Compute" --settings "{\"commandToExecute\":\"powershell.exe -c \\\"Get-Process | Where-Object { $_.CPU -gt 10000 }\\\"\"}" --type-handler-version "1.10" --resource-group "myResourceGroup"
```

Aşağıdaki örnek, bir yay etkin sunucusunda Key Vault VM uzantısını (Önizleme) sunar:

```azurecli
az connectedmachine machine-extension create --resource-group "resourceGroupName" --machine-name "myMachineName" --location "regionName" --publisher "Microsoft.Azure.KeyVault" --type "KeyVaultForLinux or KeyVaultForWindows" --name "KeyVaultForLinux or KeyVaultForWindows" --settings '{"secretsManagementSettings": { "pollingIntervalInS": "60", "observedCertificates": ["observedCert1"] }, "authenticationSettings": { "msiEndpoint": "http://localhost:40342/metadata/identity" }}'
```

## <a name="list-extensions-installed"></a>Yüklü liste uzantıları

Yay etkin sunucunuzdaki VM uzantılarının listesini almak için [az connectedmachine Machine-Extension List](/cli/azure/ext/connectedmachine/connectedmachine/machine-extension#ext_connectedmachine_az_connectedmachine_machine_extension_list) öğesini `--machine-name` ve `--resource-group` parametrelerini kullanın.

Örnek:

```azurecli
az connectedmachine machine-extension list --machine-name "myMachineName" --resource-group "myResourceGroup"
```

Varsayılan olarak, Azure CLı komutlarının çıkışı JSON (JavaScript Nesne Gösterimi) ' dir. Örneğin, varsayılan çıktıyı bir liste veya tablo olarak değiştirmek için [az configure--output](/cli/azure/reference-index)kullanın. Ayrıca, `--output` Çıkış biçiminde tek bir zaman değişikliği için herhangi bir komuta ekleyebilirsiniz.

Aşağıdaki örnek, komutundan gelen kısmi JSON çıkışını gösterir `az connectedmachine machine-extension -list` :

```json
[
  {
    "autoUpgradingMinorVersion": "false",
    "forceUpdateTag": null,
    "id": "/subscriptions/subscriptionId/resourceGroups/resourceGroupName/providers/Microsoft.HybridCompute/machines/SVR01/extensions/DependencyAgentWindows",
    "location": "eastus",
    "name": "DependencyAgentWindows",
    "namePropertiesInstanceViewName": "DependencyAgentWindows",
```

## <a name="remove-an-installed-extension"></a>Yüklü bir uzantıyı kaldırma

Arc etkin sunucunuzda yüklü bir VM uzantısını kaldırmak için [az connectedmachine Machine-Extension Delete](/cli/azure/ext/connectedmachine/connectedmachine/machine-extension#ext_connectedmachine_az_connectedmachine_machine_extension_delete) `--extension-name` öğesini `--machine-name` ve `--resource-group` parametrelerini kullanın.

Örneğin, Linux için Log Analytics VM uzantısını kaldırmak için aşağıdaki komutu çalıştırın:

```azurecli
az connectedmachine machine-extension delete --machine-name "myMachineName" --name "OmsAgentforLinux" --resource-group "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Portal](manage-vm-extensions-portal.md)veya [Azure Resource Manager ŞABLONLARıNDAN](manage-vm-extensions-template.md) [Azure PowerShell](manage-vm-extensions-powershell.md)kullanarak VM uzantılarını dağıtabilir, yönetebilir ve kaldırabilirsiniz.

- Sorun giderme bilgileri, [VM genişletmeleri sorunlarını gider kılavuzunda](troubleshoot-vm-extensions.md)bulunabilir.
